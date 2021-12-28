图像基础
===================

像素
-------

像素是图像的最原始组成部分。大部分像素有以下两种表示方式：

1. 灰度/单通道[0,255]
#. 彩色(RGB[0,255],HSV...)

	.. image:: ./image_fundamentals/RGB.png
	   :align: center
	   :width: 300


图像坐标系
-----------

如下图所示，原点(0,0)对应图片的左上角。

	.. image:: ./image_fundamentals/coord.png
	   :align: center
	   :width: 300


NumPy Array 表示图像
---------------------

OpenCV，scikit-image等图像处理库都是使用(height, width, depth)的NumPy arrays形式来表示RGB图片。
Opencv使用B,G,R的顺序存储RBG格式的图片。

	.. code:: python

		import cv2
		image = cv2.imread("example.png")
		print(image.shape)
		cv2.imshow("Image", image)
		cv2.waitKey(0)

		(b, g, r) = image[20, 100] # acesses pixels at x=100,y=20


图像缩放
--------

图像缩放要保持横纵比。
