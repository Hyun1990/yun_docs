What is Pytorch?
================
**Created** : 2020/09/16

Pytorch 是一个基于Python的科学计算包，主要针对两类人群：

* Numpy的替代品，可以利用GPU的性能进行计算
* 一个高灵活性，速度快的深度学习平台

Tensor
------

Tensor (张量) 类似于Numpy的 ndarrays, 但可用于GPU的加速计算

* 构造一个 5*3 的非初始化矩阵

	.. code:: python

		x = torch.empty(5, 3)
		print(x)

	Out:

	::

		tensor([[-6.6637e-02,  3.0784e-41, -9.6613e-26],
		    [ 4.5678e-41, -1.6234e-26,  4.5678e-41],
		    [-1.6053e-26,  4.5678e-41, -9.4851e-26],
		    [ 4.5678e-41, -1.6227e-26,  4.5678e-41],
		    [-1.6234e-26,  4.5678e-41, -1.6050e-26]])


* 构造一个随机初始化矩阵

	.. code:: python
		
		x = torch.rand(5, 3)
		print(x)

	Out:

	::
		
		tensor([[0.9023, 0.4227, 0.0287],
			[0.1716, 0.1155, 0.9087],
			[0.9664, 0.6516, 0.5828],
			[0.9364, 0.4441, 0.6797],
			[0.4572, 0.9101, 0.8719]])


* 构造一个长整型的"0"矩阵

	.. code:: python

		x = torch.zeros(5, 3, dtype=torch.long)
		print(x)

	Out:

	::
		
		tensor([[0, 0, 0],
			[0, 0, 0],
			[0, 0, 0],
			[0, 0, 0],
			[0, 0, 0]])


* 直接从数据构造张量

	.. code:: python

		x = torch.tensor([5.5, 3])
		print(x)

	Out:

	::

		tensor([5.5000, 3.0000])


* 在已有张量的基础上构造张量

	.. code:: python

		x = x.new_ones(5, 3, dtype=torch.double)   # new_* methods take in sizes
		print(x)

		x = torch.randn_like(x, dtype=torch.float)   # override dtype!
		print(x)   # result has the same size

	Out:

	::

		tensor([[1., 1., 1.],
			[1., 1., 1.],
			[1., 1., 1.],
			[1., 1., 1.],
			[1., 1., 1.]], dtype=torch.float64)
		tensor([[-1.7800, -0.8903, -0.0042],
			[ 1.6515,  0.1359,  1.7481],
			[ 0.0573, -1.6840, -2.0631],
			[-0.5931, -1.3881,  2.2346],
			[ 0.2844, -1.5098,  0.2863]])


	获取大小：

	.. code:: python

		print(x.size())

	Out:

	::

		torch.Size([5, 3])


Operations
----------

一种运算有多种语法。如下示例为加法运算。

* 加法：语法 1

	.. code:: python

		y = torch.rand(5, 3)
		print(x +  y)

	Out:

	::

		tensor([[-1.2826, -0.2573,  0.3286],
			[ 2.1775,  0.5792,  2.1281],
			[ 0.8197, -0.9229, -1.8614],
			[-0.1251, -0.4874,  2.2713],
			[ 0.3530, -0.5945,  0.9644]])


* 加法：语法 2

	.. code:: python

		print(torch.add(x, y))

	Out:

	::

		tensor([[-1.2826, -0.2573,  0.3286],
			[ 2.1775,  0.5792,  2.1281],
			[ 0.8197, -0.9229, -1.8614],
			[-0.1251, -0.4874,  2.2713],
			[ 0.3530, -0.5945,  0.9644]])


* 加法：给定一个输出张量作为参数

	.. code:: python

		result = torch.empty(5, 3)
		torch.add(x, y, out=result)
		print(result)

	Out:

	::

		tensor([[-1.2826, -0.2573,  0.3286],
			[ 2.1775,  0.5792,  2.1281],
			[ 0.8197, -0.9229, -1.8614],
			[-0.1251, -0.4874,  2.2713],
			[ 0.3530, -0.5945,  0.9644]])


* 加法：原位操作(in-place)

	.. code:: python

		# adds x to y
		y.add_(x)
		print(y)

	Out:

	::

		tensor([[-1.2826, -0.2573,  0.3286],
			[ 2.1775,  0.5792,  2.1281],
			[ 0.8197, -0.9229, -1.8614],
			[-0.1251, -0.4874,  2.2713],
			[ 0.3530, -0.5945,  0.9644]])

	.. note::
		任何一个in-place改变张量的操作后面都固定一个 "_" 。例如 ``x.copy_(y)``，``x.t_()`` 将更改x

* 也可以使用像标准Numpy一样的各种索引操作：

	.. code:: python

		print(x[:, 1])

	Out:

	::

		tensor([-0.8903,  0.1359, -1.6840, -1.3881, -1.5098])


* 改变形状 ``torch.view``

	.. code:: python

		x = torch.randn(4, 4)
		y = x.view(16)
		z = x.view(-1, 8)
		print(x.size(), y.size(), z.size())

	Out:

	::

		torch.Size([4, 4]) torch.Size([16]) torch.Size([2, 8])


* 如果是仅包含一个元素的tensor，可以使用 ``.item()`` 来得到对应的python数值

	.. code::
		
		x = torch.randn(1)
		print(x)
		print(x.item())

	Out:

	::

		tensor([-0.1118])
		-0.11180663853883743
		

桥接 Numpy
----------

Torch张量和Numpy数组将共享它们底层的内存位置，因此当一个改变时，另一个也会改变。

* 将Torch张量转换成Numpy数组

	.. code:: python

		a = torch.ones(5)
		print(a)

	Out:

	::

		tensor([1., 1., 1., 1., 1.])

	.. code:: python

		b = a.numpy()
		print(b)

	Out:

	::

		[1. 1. 1. 1. 1.]

	.. code:: python

		a.add_(1)
		print(a)
		print(b)

	Out:

	::

		tensor([2., 2., 2., 2., 2.])
		[2. 2. 2. 2. 2.]


* Numpy数组转换为Torch张量

	.. code:: python

		import numpy as np
		a = np.ones(5)
		b = torch.from_numpy(a)
		np.add(a, 1, out=a)
		print(a)
		print(b)

	Out:

	::

		array([2., 2., 2., 2., 2.])
		tensor([2., 2., 2., 2., 2.], dtype=torch.float64)


CUDA Tensors
------------

张量可以使用`.to`方法移动到任何设备

	.. code:: python

		# let us run this cell only if CUDA is available
		# we will use ''torch.device'' object to movw tensors in and out of GPU
		if torch.cuda.is_available():
			device = torch.device("cuda")
			y = torch.ones_like(x, device=device)
			x = x.to(device)
			z = x + y
			print(z)
			print(z.to("cpu", torch.double))

	Out:

	::

		tensor([1.1647], device='cuda:0')
		tensor([1.1647], dtype=torch.float64)


参考文档
--------

| https://pytorch.org/tutorials/beginner/blitz/tensor_tutorial.html
| `Tensor Operations <https://pytorch.org/docs/stable/torch.html>`_


 
