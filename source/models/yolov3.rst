yolo_v3
========
论文地址 `YOLOv3: An Incremental Improvement <https://pjreddie.com/media/files/papers/YOLOv3.pdf>`_

网络结构图
----------

 .. image:: ./images_yolov3/net.jpeg


 .. image:: ./images_yolov3/v3_net.jpg

|

 1. 整个YOLOv3结构中是没有池化层和全连接层的。前向传播中，张量尺寸变换是通过改变卷积核的步长来实现的。YOLOv2和YOLOv3一样，backbone都会将输出特征图缩小到输入的1/32，所以，通常要求输入图片是32的倍数。
 #. YOLOv3借鉴了金字塔特征(FPN)思想，小尺寸特征图用于检测大尺寸物体，大尺寸特征图检测小尺寸物体。
 #. YOLOv3总共输出3个特征图，第一个特征图下采样32倍，第二个特征图下采样16倍，第三个下采样8倍。输入图像经过DarkNet-53(无全连接层)，再经过Yoloblock生成的特征图被当作两用，第一用为经过3*3卷积层、1*1卷积之后生成特征图一，第二用为经过1*1卷积层加上采样层，与DarkNet-53网络的中间层输出结果进行拼接，产生特征图二。同样的循环之后产生特征图三。
 #. concat操作：源于DenseNet网络的设计思路，将特征图按照通道维度直接进行拼接，例如8*8*16的特征图与8*8*16的特征图拼接后生成8*8*32的特征图。
 #. 上采样层(upsample)：作用是将小尺寸特征图通过插值等方法，生成大尺寸图像。例如使用最邻近插值算法，将8*8的图像变换为16*16。上采样层不改变特征图的通道数。


Backbone
---------
YOLOv3 采用 Darknet-53 来进行特征提取。

 .. image:: ./images_yolov3/darknet-53.png
	:align: center

 |

Output
--------

**Anchor box**
 YOLOv1中，网络直接检测框的宽、高。YOLOv2中改为集于先验框的变化值，降低网络学习难度。YOLOv3为每种FPN预测特征图 (13*13,26*26,52*52) 设定3种 anchor box，共聚类出9种尺寸的anchor box。

 分配上，在最小的13*13特征图上由于其感受野最大故应用最大的anchor box (116x90)，(156x198)，(373x326)，（这几个坐标是针对416*416下的，当然要除以32把尺度缩放到13*13下），适合检测较大的目标。中等的26*26特征图上由于其具有中等感受野故应用中等的anchor box (30x61)，(62x45)，(59x119)，适合检测中等大小的目标。较大的52*52特征图上由于其具有较小的感受野故应用最小的anchor box(10x13)，(16x30)，(33x23)，适合检测较小的目标。

**Bounding Box Prediction**


Training
---------


参考文档
--------
 .. [1] `YOLOv3 深度解析 <https://blog.csdn.net/leviopku/article/details/82660381>`_
