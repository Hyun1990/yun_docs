优化方法与正则化
===================

梯度下降
---------
通过迭代的过程逐步优化参数降低loss，以获取高分类精度。

2维空间和3维空间的loss函数可用下图示意，实际情况下loss函数是多维的。

	.. image:: ./image_optimization/loss.png
	   :align: center
	   :width: 450

如何找到loss最小的点？答案是使用梯度下降，计算各维度的梯度

	.. math::
			\frac{df(x)}{dx} = lim_{h \rightarrow 0} \frac{f(x + h) - f(x)}{h}

当维度大于1的时候，梯度为偏导数向量。实际上，我们使用解析梯度来代替数值梯度。

**伪代码**
	
	.. code:: python

		while True:
			Wgradient = evaluate_gradient(loss, data, W)
				W += -alpha * Wgradient

结束循环的条件：

 1. 训练到指定的迭代次数
 #. loss降到足够低并且训练的准确率高
 #. loss在M个epoches后不再降低


随机梯度下降(SGD)
-----------------

SGD通过学习小批量数据后更新权重，而不是经过一个epoch再更新权重。

**Mini-batch SGD**

	.. code:: python

		while True:
			batch = next_training_batch(data, 256)
			Wgradient evaluate_gradient(loss, batch, W)
			W += -alpha + Wgradient

典型的 batch sizes 有32，64，128，256。

SGD的扩展
---------

**Momentum**

	.. math::
		W = W - \alpha \nabla_{W}f(W)\\
		W = W + V

**Nesterov's Acceleration**


正则化
------

**目的**

减小求出错误解的可能性，减小过拟合，提高泛化能力。

在loss function 中加入正则项

	.. math::
		L = \frac{1}{N} \sum_{i=1}^{N} L_{i} + \lambda R(W)


**正则化类型**

L2:

	.. math::
		R(W) = \sum_{i} \sum_{j} W_{i,j}^{2}

L1:

	.. math::
		R(W) = \sum_{i} \sum_{j} \vert W_{i,j} \vert

Elastic Net regularization:

	.. math::
		R(W) = \sum_{i} \sum_{j} \beta W_{i,j}^{2} + \vert W_{i,j} \vert


