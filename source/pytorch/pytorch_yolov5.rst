Pytorch_yolov5
===============
**Created** : 2020/07/09

环境部署
--------
| 1. 创建anaconda独立环境
| ``$ conda create -name pytorch_env_38 python=3.8``
| 2. 激活环境
| ``$ conda activate pytorch_env_38``
| 3. 下载yolov5版本库
| ``$ git clone https://github.com/ultralytics/yolov5 # clone repo``

 ::

   yolov5
    ├── data
    │   ├── coco128.yaml
    │   ├── coco.yaml
    │   ├── get_coco2017.sh
    │   ├── get_voc.sh
    │   └── voc.yaml
    ├── detect.py
    ├── Dockerfile
    ├── hubconf.py
    ├── inference
    │   ├── images
    │   │   ├── bus.jpg
    │   │   └── zidane.jpg
    │   └── output
    │       └── 003241.jpg
    ├── LICENSE
    ├── models
    │   ├── common.py
    │   ├── experimental.py
    │   ├── export.py
    │   ├── hub
    │   │   ├── yolov3-spp.yaml
    │   │   ├── yolov5-fpn.yaml
    │   │   └── yolov5-panet.yaml
    │   ├── __init__.py
    │   ├── __pycache__
    │   │   ├── common.cpython-36.pyc
    │   │   ├── experimental.cpython-36.pyc
    │   │   ├── __init__.cpython-36.pyc
    │   │   └── yolo.cpython-36.pyc
    │   ├── yolo.py
    │   ├── yolov5l.yaml
    │   ├── yolov5m.yaml
    │   ├── yolov5s.yaml
    │   └── yolov5x.yaml
    ├── README.md
    ├── requirements.txt
    ├── test.py
    ├── train.py
    ├── tutorial.ipynb
    ├── utils
    │   ├── activations.py
    │   ├── datasets.py
    │   ├── google_utils.py
    │   ├── __init__.py
    │   ├── __pycache__
    │   │   ├── datasets.cpython-36.pyc
    │   │   ├── google_utils.cpython-36.pyc
    │   │   ├── __init__.cpython-36.pyc
    │   │   ├── torch_utils.cpython-36.pyc
    │   │   └── utils.cpython-36.pyc
    │   ├── torch_utils.py
    │   └── utils.py
    └── weights
        ├── best.pt
        └── download_weights.sh

| ``$ cd yolov5``
| 4. 根据yolov5/requirements.txt安装必要的库
| ``$ conda update -yn base -c defaults conda``
| ``$ conda install -yc anaconda numpy opencv matplotlib tqdm pillow ipython``
| ``$ conda install -yc conda-forge scikit-image pycocotools tensorboard``
| ``$ conda install -yc spyder-ide spyder-line-profiler``
| ``$ conda install -yc pytorch pytorch torchvision``
| ``$ conda install -yc conda-forge protobuf numpy && pip install onnx==1.6.0``
|
| 5.若只推理，只需安装如下库
| ``$ conda install -y opencv=4.4.0``
| ``$ conda install -y pytorch=1.6.0``
| ``$ conda install -y pillow``
| ``$ conda install -y tqdm``
| ``$ conda install -y matplotlib``
| ``$ conda install -y pyyaml=5.3.1``
| ``$ conda install -y scipy=1.5.2``
| ``$ conda install -y torchvision=0.7.0``
|

准备数据集
-----------
 ::

	VOC
	├── *2007_test.txt*              #生成的测试集名单
	├── *2007_train.txt*
	├── *2007_val.txt*
	├── combination_train_val.py
	├── *train.txt*                  # 生成的训练集名单
	├── VOCdevkit
	│   └── VOC2007
	│       ├── Annotations          # 使用LableImg生成的.xml文件
	│       │   ├── 004148.xml
	│       │   ├── 004149.xml
	│       │   ├── 004150.xml
	│       │   ├── 004151.xml
	│       │   └── 004152.xml
	│       ├── ImageSets
	│       │   ├── Layout
	│       │   ├── Main
	│       │   │   ├── *test.txt*
	│       │   │   ├── *train.txt*
	│       │   │   ├── *trainval.txt*
	│       │   │   └── *val.txt*
	│       │   └── Segmentation
	│       ├── JPEGImages
	│       │   ├── 004148.jpg
	│       │   ├── 004149.jpg
	│       │   ├── 004150.jpg
	│       │   ├── 004151.jpg
	│       │   └── 004152.jpg
	│       ├── *labels*		 # 由xml转成符合yolo要求的txt格式
	│       │   ├── *004148.txt*
	│       │   ├── *004149.txt*
	│       │   ├── *004150.txt*
	│       │   ├── *004151.txt*
	│       │   └── *004152.txt*
	│       └── xml_to_txt.py
	└── voc_lable.py


 1. 把原始图片及对应的xml文件分别上传到JPEGImages和Annotations下
 #. 将xml文件转换成yolo格式 ``$ python3 xml_to_txt.py``
 #. 生成训练集，测试集和验证集 ``$ python3 voc_label.py``
 #. 将验证集和训练集合并成一个train.txt ``$ python3 combination_train_val.py``


 .. note::
	\*xxx\*为生成的文件

训练
-----
| 1. 创建data.yaml

 ::

   # train and val datasets (image directory or *.txt file with image paths)
   train: ../darknet/light_train.txt #对应数据集中的train.txt(注意路径)
   val: ../darknet/light_test.txt    #对应数据集中的2007_test.txt

   # number of classes
   nc: 1
   # class names
   names: ['light']

| 2. 选择模型
| 以yolov5/models下yolov5s.yaml为模板，更新 ``nu:1``，生成custom_yolov5s.yaml

 ::

	# parameters
	nc: 1  # number of classes   <------------------  UPDATE to match your dataset
	depth_multiple: 0.33  # model depth multiple
	width_multiple: 0.50  # layer channel multiple

	# anchors
	anchors:
	  - [10,13, 16,30, 33,23]  # P3/8
	  - [30,61, 62,45, 59,119]  # P4/16
	  - [116,90, 156,198, 373,326]  # P5/32

	# YOLOv5 backbone
	backbone:
	  # [from, number, module, args]
	  [[-1, 1, Focus, [64, 3]],  # 1-P1/2
	   [-1, 1, Conv, [128, 3, 2]],  # 2-P2/4
	   [-1, 3, Bottleneck, [128]],
	   [-1, 1, Conv, [256, 3, 2]],  # 4-P3/8
	   [-1, 9, BottleneckCSP, [256, False]],
	   [-1, 1, Conv, [512, 3, 2]],  # 6-P4/16
	   [-1, 9, BottleneckCSP, [512, False]],
	   [-1, 1, Conv, [1024, 3, 2]], # 8-P5/32
	   [-1, 1, SPP, [1024, [5, 9, 13]]],
	   [-1, 12, BottleneckCSP, [1024, False]],  # 10
	  ]

	# YOLOv5 head
	head:
	  [[-1, 1, nn.Conv2d, [na * (nc + 5), 1, 1, 0]],  # 12 (P5/32-large)

	   [-2, 1, nn.Upsample, [None, 2, 'nearest']],
	   [[-1, 6], 1, Concat, [1]],  # cat backbone P4
	   [-1, 1, Conv, [512, 1, 1]],
	   [-1, 3, BottleneckCSP, [512, False]],
	   [-1, 1, nn.Conv2d, [na * (nc + 5), 1, 1, 0]],  # 16 (P4/16-medium)

	   [-2, 1, nn.Upsample, [None, 2, 'nearest']],
	   [[-1, 4], 1, Concat, [1]],  # cat backbone P3
	   [-1, 1, Conv, [256, 1, 1]],
	   [-1, 3, BottleneckCSP, [256, False]],
	   [-1, 1, nn.Conv2d, [na * (nc + 5), 1, 1, 0]],  # 21 (P3/8-small)

	   [[], 1, Detect, [nc, anchors]],  # Detect(P3, P4, P5)
	  ]

| 3. 训练
| 将data.yaml和custom_yolov5s.yaml上传到与yolov5同级目录
| ``$ python train.py --img 640 --batch 16 --epochs 200 --data ./data.yaml``
| ``--cfg ./custom_yolov5s.yaml --weights ' '``
|
| 4. 训练完生成的权重在weights目录下best.pt和last.pt
|

 .. Tip::
	使用GoogleColab训练，参考 `该文档 <https://github.com/ultralytics/yolov5/>`_ 的Tutorials->Colab部分


推理
-----
| ``$python detect.py --weights weights/best.pt --img 640 --conf 0.4 --source xxx.jpg``
| 测试输出的图片在inference/output/下
|

Pytorch --> ONNX
-----------------
 将训练好的Pytorch YOLOV5 模型转换成 ONNX 格式

| ``$ pip install onnx``
| ``$ cd yolov5``
| ``$ export PYTHONPATH="$PWD"``
| ``$ python models/export.py --weights weights/best.pt --img 640 --batch 1``

 转换后的模型为weights/best.onnx

代码解析
---------

yaml
~~~~~



参考文档
--------

| https://github.com/ultralytics/yolov5
| https://github.com/ultralytics/yolov5/wiki/Train-Custom-Data



