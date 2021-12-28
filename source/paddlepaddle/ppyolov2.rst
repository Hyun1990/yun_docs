PP-YOLOV2
==========

论文地址： `PP-YOLOv2: A Practical Object Detector <https://arxiv.org/abs/2104.10419>`_

`PaddleDetection <https://github.com/PaddlePaddle/PaddleDetection>`_

该文档记录了在Baidu AI Studio上使用PP-YOLOV2


准备数据集
-----------

参考百度文档 `数据准备 <https://github.com/PaddlePaddle/PaddleDetection/blob/release/2.1/docs/tutorials/PrepareDataSet.md>`_

使用VOC数据集，需要的数据集结果如下：

        .. image:: ./img_ppyolo/data.png


* **annotations** xml标注文件

* **images** 图片文件夹

* **label_list.txt** 标签名

* **valid.txt** 验证集文件, 格式如图右侧显示

* **train.txt** 测试集文件，格式如图右侧显示


| 如何生成train.txt格式的文件：
|
| 1. annotations 中的文件按行写入到a.txt
|
| 2. copy a.txt 为 b.txt 并将xml后缀修改成jpg
|
| 3. 按行合并a.txt 和 b.txt 为c.txt
|
| 4. 按训练集和测试集比例划分c.txt为train.txt和valid.txt

将上述文件打包成img.zip


上传数据集
-----------

* 打开 `PP AI Studio <https://aistudio.baidu.com/aistudio/datasetoverview>`_

* 登陆并创建数据集，将img_weld.zip上传上去


创建项目
---------

参考百度文档 `项目概述 <https://ai.baidu.com/ai-doc/AISTUDIO/0k3e2tfzm>`_ 中的创建并运行一个项目

* 打开 `创建项目页面 <https://ai.baidu.com/ai-doc/AISTUDIO/0k3e2tfzm>`_

* 创建Notebook项目

* 添加数据集


部署Notebook项目
----------------

参考百度文档 `Notebook环境使用说明 <https://ai.baidu.com/ai-doc/AISTUDIO/sk3e2z8sb>`_

| 1. 打开已创建的项目，点击进入
|
| 2. 按照官方指导挂载数据集，进行持久化安装
|
| 3. 解压数据集
|    ``!unzip ~/data/data86830/img_weld.zip``
|
| 4. `安装PaddlePaddleDetection <https://github.com/PaddlePaddle/PaddleDetection/blob/release/2.1/docs/tutorials/INSTALL_cn.md>`_
|   * 安装PaddlePaddle
|   ``!python -m pip install paddlepaddle-gpu==2.1.0.post101 -f https://paddlepaddle.org.cn/whl/mkl/stable.html``
|   * 安装PaddleDetection
|   ``!git clone https://github.com/PaddlePaddle/PaddleDetection.git``
|   ``%cd ~/PaddleDetection/``
|   ``!python setup.py install``
|   ``!pip install -r requirements.txt``
|   * 确认是否安装成功
|   ``!python ppdet/modeling/tests/test_architectures.py``


修改配置文件
-------------

参考 `配置文件说明 <https://github.com/PaddlePaddle/PaddleDetection/blob/release/2.1/docs/tutorials/GETTING_STARTED_cn.md>`_ 和 `YOLO参数配置教程 <https://github.com/PaddlePaddle/PaddleDetection/blob/release/2.1/docs/tutorials/config_annotation/ppyolo_r50vd_dcn_1x_coco_annotation.md>`_

| 1. 将data目录下的数据集copy到PaddleDetection下
| ``!cp ~/data/data86830/img_weld ~/PaddleDetection/dataset/``
|
| 2. 我们选择configs/ppyolo/ppyolov2_r50vd_dcn_365e_coco.yml配置进行训练。PaddleDetection 2.0 采用了模块解耦设计，如下图所示，依赖其他配置文件

    .. image:: ./img_ppyolo/total.png
            :width: 450


| 3. 根据自定义的模型修改对应的配置文件
|
|   * 我们采用VOC数据集，因此将上述配置文件修改成
|   ``'../datasets/voc.yml'``

    .. image:: ./img_ppyolo/new_total.png
            :width: 450

|
|   * 根据数据集修改 ``voc.yml``中的 ``num_class``, ``dataset_dir``, ``anno_path``

    .. image:: ./img_ppyolo/voc.png
            :width: 500

|
|   * 根据需要修改 ``runtime.yml``

    .. image:: ./img_ppyolo/runtime.png
            :width: 250

|
|   * 根据需要修改 ``ppyolov2_r50vd_dcn.yml``
|    anchors 可根据数据集计算出来
|    ``!python tools/anchor_cluster.py -c configs/ppyolo/ppyolov2_r50vd_dcn_365e_coco.yml -n 9 -s 640 -m v2 -i 1000``

    .. image:: ./img_ppyolo/dcn.png
            :width: 800

|
|   * 根据需要修改 ``optimizer_365e.yml`` 中的 ``epoch`` 等

    .. image:: ./img_ppyolo/optimizer.png
            :width: 300

|
|   * 根据需要修改 ``ppyolov2_reader.yml``

    .. image:: ./img_ppyolo/reader.png
            :width: 800


训练
-----

参考百度文档 `快速开始 <https://github.com/PaddlePaddle/PaddleDetection/blob/release/2.1/docs/tutorials/GETTING_STARTED_cn.md>`_

| ``%cd ~/PaddleDetection/``
| ``!CUDA_VISIBLE_DEVICES=0``
| ``!python tools/train.py -c configs/yolo/ppyolov2_r50vd_dcn_365e_coco.yml``

恢复训练

| ``%cd ~/PaddleDetection/``
| ``!CUDA_VISIBLE_DEVICES=0``
| ``!python tools/train.py -c configs/yolo/ppyolov2_r50vd_dcn_365e_coco.yml -r output/ppyolov2_r50vd_dcn_365e_coco/20``


评估
-----

默认训练生成的模型保存在当前 ``output`` 文件夹下

| ``!python tools/eval.py -c configs/yolo/ppyolov2_r50vd_dcn_365e_coco.yml -o weights=output/yolo/ppyolov2_r50vd_dcn_365e_coco/model_final.pdparams``

.. note::
    eval.py 会报错，把 ``ppyolov2_reader.yml`` batch_size设置成1


预测
-----

| ``!python tools/infer.py -c configs/yolo/ppyolov2_r50vd_dcn_365e_coco.yml --infer_img=demo/000001.jpg -o weights=output/yolo/ppyolov2_r50vd_dcn_365e_coco/model_final.pdparams``


模型压缩，部署
---------------

参考百度文档 `进阶教程 <https://github.com/PaddlePaddle/PaddleDetection>`_
