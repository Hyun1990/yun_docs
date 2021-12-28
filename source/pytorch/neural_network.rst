Neural Networks
================
**Created** : 2020/09/17

可以用 ``torch.nn`` 包来构建神经网络。

一个 ``nn.Module`` 包含各个层及一个返回output的forward(input)方法。

一个神经网络典型的训练过程如下：

| * 定义神经网络及一些可学习的参数

| * 在输入数据集上迭代

| * 通过网络处理输入

| * 计算loss

| * 将梯度反向传播给网络参数

| * 更新网络权重，一般使用一个简单的规则: ``weight = weight - learning_rate * gradient``

一个简单的前馈网络如下

	.. image:: ./imgs/mnist.png
		:align: center

定义网络
--------

	.. code:: python

		import torch
		import torch.nn as nn
		import torch.nn.functional as F

		class Net(nn.Module):
			def __init__(self):
				super(Net, self).__init__()
				# 1 input image channel, 6 output channels, 3*3 square convolution kernel
				self.conv1 = nn.Conv2d(1, 6, 3)
				self.conv2 = nn.Conv2d(6, 16, 3)
				# an affine operation: y = Wx + b
				self.fc1 = nn.Linear(16 * 6 * 6, 120) # 6*6 from image dimension
				self.fc2 = nn.Linear(120, 84)
				self.fc3 = nn.Linear(84, 10)

			def forward(self, x):
				# Max pooling over a (2, 2) window
				x = F.max_pool2d(F.relu(self.conv1(x)), (2, 2))
				# If the size is a square you can only specific a single number
				x = F.max_pool2d(F.relu(self.conv2(x)), 2)
				x = x.view(-1, self.num_flat_features(x))
				x = F.relu(self.fc1(x))
				x = F.relu(self.fc2(x))
				x = self.fc3(x)
				return x

			def num_flat_features(self, x):
				size = x.size()[1:] # all dimensions except the batch dimension
				num_features = 1
				for s in size:
					num_features *= s
				return num_features

		net = Net()
		print(net)

	
	Out:
	
	::

		Net(
		  (conv1): Conv2d(1, 6, kernel_size=(3, 3), stride=(1, 1))
		  (conv2): Conv2d(6, 16, kernel_size=(3, 3), stride=(1, 1))
		  (fc1): Linear(in_features=576, out_features=120, bias=True)
		  (fc2): Linear(in_features=120, out_features=84, bias=True)
		  (fc3): Linear(in_features=84, out_features=10, bias=True)
		)
		
	我们只需要定义 ``forward`` 函数， ``backward`` 函数会在使用 ``autograd`` 时自动定义。

	一个模型的学习参数可以通过 ``net.parameters()`` 返回

	.. code:: python

		params = list(net.parameters())
		print(len(params))
		print(params[0].size()) # conv1's weights

	Out:

	::

		10
		torch.Size([6, 1, 3, 3])

	试用一个随机 32*32 作输入

	.. code:: python

		input = torch.randn(1, 1, 32, 32)
		out = net(input)
		print(out)

	Out:

	::

		tensor([[-0.0052,  0.0509,  0.0998,  0.0420,  0.0676, -0.0151, -0.0805,  0.1161,
				 -0.1910,  0.0688]], grad_fn=<AddmmBackward>)

		
	清零所有参数的梯度缓存，然后进行随机梯度的反向传播：
	
	.. code:: python

		net.zero_grad()
		out.backward(torch.randn(1, 10))

	.. note::
		``torch.nn`` 仅支持mini-batch的输入，例如 ``nn.Conv2d`` 接受一个4维张量，即 ``nSamples x nChannels x Height x Width`` 。如果是单独的样本，只需要使用 ``input.unsqueeze(0)`` 来添加一个假的批大小维度。

损失函数
--------------

损失函数接受一对(output, target)作为输入。

	.. code:: python

		output = net(input)
		target = torch.randn(10) # 本例使用模拟数据
		target = target.view(1, -1) # 使目标值于数据尺寸一致
		criterion = nn.MSELoss()

		loss = criterion(output, target)
		print(loss)

	Out:

	::

		tensor(1.0889, grad_fn=<MseLossBackward>)

	如果使用loss的 ``.grad_fn`` 属性跟踪反向传播过程，会看到如下计算图：

	::

		input -> conv2d -> relu -> maxpool2d -> conv2d -> relu -> maxpool2d
			  -> view -> linear -> relu -> linear -> relu -> linear
			  -> MSELoss
			  -> loss
	
	为了说明，我们向后跟踪几步

	.. code:: python

		print(loss.grad_fn) # MSELoss
		print(loss.grad_fn.next_functions[0][0]) # Linear
		print(loss.grad_fn.next_functions[0][0].next_functions[0][0]) # ReLU

	Out:

	::

		<MseLossBackward object at 0x7fa0356f49a0>
		<AddmmBackward object at 0x7fa0356f4760>
		<AccumulateGrad object at 0x7fa0356f4760>

		
反向传播
--------

我们只需调用 ``loss.backward()`` 来反向传播误差。需要清零现有梯度，否则梯度将会与已有的梯度累加。

	.. code:: python

		net.zero_grad()
		
		print('conv1.bias.grad before backward')
		print(net.conv1.bias.grad)
		
		loss.backward()

		print('conv1.bias.grad after backward')
		print(net.conv1.bias.grad)

	Out:

	::

		conv1.bias.grad before backward
		None
		conv1.bias.grad after backward
		tensor([-0.0048,  0.0081,  0.0060, -0.0035,  0.0019,  0.0129])


更新权重
--------

最简单的更新规则是随机梯度下降法(SGD):

``weight = weight - learning_rate * gradient``

使用如下Python代码实现

	.. code:: python

		learning_rate = 0.01
		for f in net.parameters():
			f.data.sub_(f.grad.data * learning_rate)

然而，在使用神经网络时，我们希望使用各种不同的更新规则如 SGD，Nesterov-SGD, Adam, RMSProp 等。为此，我们构建了一个较小的包 ``torch.optim`` 来实现这些方法。
 
	.. code:: python

		import torch.optim as optim
		
		# 创建优化器
		optimizer = optim.SGD(net.parameters(), lr=0.01)

		# in your training loop
		optimizer.zero_grad()
		output = net(input)
		loss = criterion(output, target)
		loss.backward()
		optimizer.step()



参考文档
--------

| `NEURAL NETWORKS <https://pytorch.org/tutorials/beginner/blitz/neural_networks_tutorial.html>`_


 
