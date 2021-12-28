yolov5s.yaml
-------------

 ::

	# parameters
	nc: 80  # number of classes  类别数
	depth_multiple: 0.33  # model depth multiple  用在Bottleneck层使用，控制模型的深度
                              # yolo5l中为1，有3个Bottleneck，则yolov5s中就只有一个Bottleneck
	width_multiple: 0.50  # layer channel multiple  控制卷积核的个数
                              # 主要用于设置arguments，如yolov5s设置为0.5，Focus就变成[32,3],
                              # Conv就变成[16,4,3,2]

	# anchors
	anchors:
	  - [10,13, 16,30, 33,23]  # P3/8
	  - [30,61, 62,45, 59,119]  # P4/16
	  - [116,90, 156,198, 373,326]  # P5/32

	# YOLOv5 backbone
        # from: -1代表从上一层获取输入，-2代表从上两层获取输入
        # number: 1表示只有一个，3表示有三个相同的模块
        # SPP、Conv、Bottleneck、BottleneckCSP的代码可以在 ./models/common.py 中获取到
	backbone:
	  # [from, number, module, args]
	  [[-1, 1, Focus, [64, 3]],  # 0-P1/2
	   [-1, 1, Conv, [128, 3, 2]],  # 1-P2/4
	   [-1, 3, BottleneckCSP, [128]],
	   [-1, 1, Conv, [256, 3, 2]],  # 3-P3/8
	   [-1, 9, BottleneckCSP, [256]],
	   [-1, 1, Conv, [512, 3, 2]],  # 5-P4/16
	   [-1, 9, BottleneckCSP, [512]],
	   [-1, 1, Conv, [1024, 3, 2]],  # 7-P5/32
	   [-1, 1, SPP, [1024, [5, 9, 13]]],
	   [-1, 3, BottleneckCSP, [1024, False]],  # 9
	  ]

	# YOLOv5 head
	head:
	  [[-1, 1, Conv, [512, 1, 1]],
	   [-1, 1, nn.Upsample, [None, 2, 'nearest']],
	   [[-1, 6], 1, Concat, [1]],  # cat backbone P4
	   [-1, 3, BottleneckCSP, [512, False]],  # 13

	   [-1, 1, Conv, [256, 1, 1]],
	   [-1, 1, nn.Upsample, [None, 2, 'nearest']],
	   [[-1, 4], 1, Concat, [1]],  # cat backbone P3
	   [-1, 3, BottleneckCSP, [256, False]],  # 17 (P3/8-small)

	   [-1, 1, Conv, [256, 3, 2]],
	   [[-1, 14], 1, Concat, [1]],  # cat head P4
	   [-1, 3, BottleneckCSP, [512, False]],  # 20 (P4/16-medium)

	   [-1, 1, Conv, [512, 3, 2]],
	   [[-1, 10], 1, Concat, [1]],  # cat head P5
	   [-1, 3, BottleneckCSP, [1024, False]],  # 23 (P5/32-large)

	   [[17, 20, 23], 1, Detect, [nc, anchors]],  # Detect(P3, P4, P5)
	  ]


参考文档

| `Yolov5代码解释 <https://www.pythonf.cn/read/125916>`_




