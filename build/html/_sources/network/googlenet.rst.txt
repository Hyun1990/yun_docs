GoogLeNet
==========

论文地址：`Going Deeper with Convolutions <https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43022.pdf>`_

GoogLeNet是谷歌研究出来的深度网络结构，获得了2014年ImageNet挑战赛第一名。
其稀疏性，高计算性能的网络结构使得在内存或计算资源有限的情况下，是较好的选择。

创新点
-------

* 引入Inception结构，融合不同尺度的特征信息

* 使用1*1卷积核进行降维

* 添加两个辅助分类器帮助训练

* 丢弃全连接层，使用平均池化，减少模型参数

Inception V1
-------------

Inception V1 是构成GoogLeNet的基础，将特征矩阵同时输入到多个分支进行处理，
并将输出的特征矩阵按深度进行拼接，得到最终输出。增加量网络深度减少了网络参数。

    .. image:: ./googlenet/inception.png
        :align: center

* 尺寸不同的卷积核增加了网络宽度及对不同尺度的适应性

* 池化层减少空间大小，降低过拟合

* 在每个卷积层后进行ReLU操作，增加网络的非线性特在

* 粉色
  :math:`1 \times 1` 的卷积核减少维度，同时用于修正线性激活

.. note::
    :math:`1 \times 1` 卷积核的作用

    假设previous layer为
    :math:`100 \times 100 \times 128`，输出为
    :math:`100 \times 100 \times 256`，若没有
    :math:`1 \times 1` 卷积，直接进行
    :math:`5 \times 5` 卷积，则卷积参数个数为
    :math:`5 \times 5 \times 128 \times 256`，若先经过
    :math:`1 \times 1` 卷积降低通道为32后再进行
    :math:`5 \times 5` 卷积，则卷积参数个数为
    :math:`1 \times 1 \times 128 \times 32 + 5 \times 5 \times 32 \times 256`，约减少4倍


GoogLeNet网络结构
------------------

网络共22层，包含9个inception模块，reduce表示卷积前使用了1*1的卷积

    .. image:: ./googlenet/struct.png
        :align: center

网络可视化
-----------

图中的(V)和(S)表示如何进行padding，V=valid表示不进行padding，S=same表示保持同样尺寸进行
zero padding

* 为了避免梯度消失，网络额外增加了2个辅助的softmax用于分类。在training的过程中，
  将两个辅助分类器的损失乘以权重0.3加到网络的整体损失上，再进行反向传播；在推理过程中，不使用辅助分类器。

    .. image:: ./googlenet/googlenet.png
        :align: center


参考文档
---------

| `大话CNN经典模型GoogLeNet <https://my.oschina.net/u/876354/blog/1637819>`_
| `GoogLeNet结构详解与模型的搭建 <https://blog.csdn.net/m0_37867091/article/details/107404735>`_
