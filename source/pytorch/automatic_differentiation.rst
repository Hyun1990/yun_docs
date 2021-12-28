Autogard
===================================
**Created** : 2020/09/16

Pytorch中，所有神经网络的核心是 ``autograd`` 包。

``autograd`` 包为所有张量上的操作提供自动求导功能。它是一个在运行时定义(define-by-run)的框架，这意味着反向传播是根据代码如何运行来决定的，并且每次迭代可以是不同的。


Tensor
------

``torch.Tensor`` 是这个包的核心类。如果设置 ``.requires_grad=True``，他将追踪这个张量的所有操作。当完成计算后可以调用 ``.backward()``，来自动计算所有梯度。这个张量的梯度将会自动累加到 ``.grad`` 属性。

``.detach()`` 可以将张量与计算历史分离，并阻止以后的计算被追踪。

为了防止跟踪历史记录（和使用内存），可以将代码块包装在 ``with torch.no_grad():`` 中。在评估模型时特别有用，因为模型可能具有 ``requires_grad=True`` 的可训练参数，但该过程中不需要进行梯度计算。

``Tensor`` 和 ``Function`` 互相连接生成了一个无圈图(acyclic graph)，它编码了完整的计算历史。每个张量都有一个 ``.grad_fn`` 属性，该属性引用了创建Tensor自身的Function（除非这个张量是用户手动创建的， ``.grad_fn`` 为 None）。

如果需要计算导数，可以在Tensor上调用 ``.backward()``。如果Tensor是一个标量，则不需要为 ``.backward()`` 指定任何参数，如果有更多的元素，则需要指定一个 gradient 参数，该参数是形状匹配的张量。


* 创建一个张量并追踪其计算历史

	.. code:: python
		
		import torch 
		x = torch.ones(2, 2, requires_grad=True)
		print(x)

	Out:

	:: 

		tensor([[1., 1.],
			[1., 1.]], requires_grad=True)

	张量计算

	.. code:: python

		y = x + 2
		print(y)

	Out:

	::

		tensor([[3., 3.],
			[3., 3.]], grad_fn=<AddBackward0>)

	y是计算结果，所以它有 ``grad_fn`` 属性

	.. code:: python

		print(y)

	Out:

	::

		<AddBackward0 object at 0x7f5595fc2700>

	y的更多操作

	.. code:: python

		z = y * y * 3
		out = z.mean()
		print(z, out)

	Out:

	::

		tensor([[27., 27.],
			[27., 27.]], grad_fn=<MulBackward0>) tensor(27., grad_fn=<MeanBackward0>)


* ``.requires_grad_(...)`` 原位改变一个张量的 ``requires_grad`` 标志。如果没有指定，输入标签默认为False。

	.. code:: python

		a = torch.randn(2, 2)
		a = ((a * 3) / (a - 1))
		print(a.requires_grad)
		a.requires_grad_(True)
		print(a.requires_grad)
		b = (a * a).sum()
		print(b.grad_fn)

	Out:

	::

		False
		True
		<SumBackward0 object at 0x7f5595fc2700>


Gradients
----------

现在开始进行反向传播，因为out是一个标量，因此 ``out.backward()`` 等价于 ``out.backward(torch.tensor(1.))``

	.. code:: python

		out.backward()
		# 输出导数 ``d(out)/dx``
		print(x.grad)

	Out:

	::

		tensor([[4.5000, 4.5000],
        	[4.5000, 4.5000]])


* 雅可比向量积

	.. code:: python

		x = torch.randn(3, requires_grad=True)
		y = x * 2
		while y.data.norm() < 1000:
			y = y * 2
		print(y)

	Out:
	
	::

		tensor([1440.4628,  111.4445,  108.5023], grad_fn=<MulBackward0>)

	在这种情况下，y不再是标量。``torch.autograd`` 不能直接计算完整的雅可比矩阵，但是如果我们只想要雅可比向量积，只需将这个向量作为参数传给 ``backward`` :

	.. code:: python

		v = torch.tensor([0.1, 1.0, 0.0001], dtype=torch.float)
		y = backward(v)
		print(x.grad)

	Out:

	::

		tensor([ 51.2000, 512.0000,   0.5120])

* 可以通过将代码块包装在 ``with torch.no_grad():`` 中，来阻止autograd跟踪设置了 ``.requires_grad=True`` 的张量的历史记录。

	.. code:: python

		print(x.requires_grad)
		print((x ** 2).requires_grad)

		with torch.no_grad()
			print((x ** 2).requires_grad)

	Out:

	::
	
		True
		True
		False

	或者使用 ``.detach()``

	.. code:: python

		print(x.requires_gard)
		y = x.detach()
		print(y.requires_grad)
		print(x.eq(y).all())

	Out:

	::

		True
		False
		tensor(True)


参考文档
--------

| `AUTOGRAD: AUTOMATIC DIFFERENTIATION <https://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html>`_


 
