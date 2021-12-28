NX源码编译Pytorch
==================

参考文档
---------

| `pytorch build form source <https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-8-0-now-available/72048>`_
| `github pytorch <https://github.com/pytorch/pytorch>`_


NX设置
------

| ``$ sudo nvpmodel -m 2``
| ``$ jetson_clocks``


安装pyenv
----------

| ``$ cd ~/hyun/``
| ``$ git clone https://github.com/yyuu/pyenv.git ~/.pyenv``
| ``$ echo 'export PATH=~/.pyenv/bin:$PATH' >> ~/.bashrc``
| ``$ echo 'export PYENV_ROOT=~/.pyenv' >> ~/.bashrc``
| ``$ echo 'eval "$(pyenv init -)"' >> ~/.bashrc``
| ``$ source ~/.bashrc``

| ``$ pyenv versions``
| ``$ mkdir /home/robot/.pyenv/cache``
| ``$ cd /home/robot/.pyenv/cache``
| ``$ wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tar.xz``
| ``$ sudo apt-get install -y libssl-dev libbz2-dev libreadline-dev libsqlite3-dev libffi-dev liblzma-dev``
| ``$ pyenv install 3.8.5``

| ``$ cd /home/robot/hyun/``
| ``$ sudo apt-get install python3-pip libopenblas-base libopenmpi-dev libopenblas-dev``

| ``$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile``
| ``$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile``
| ``$ echo 'eval "$(pyenv init --path)"' >> ~/.profile``
| ``$ source ~/.profile``


编译pytorch
------------

| ``$ cd ~/hyun/``
| ``$ git clone --recursive -b v1.7.0 https://github.com/pytorch/pytorch.git``
| ``$ cd pytorch``
| ``$ git submodule sync``
| ``$ git submodule update --init --recursive``

| ``$ pyenv shell 3.8.5``
| ``$ patch -p1 < ~/hyun/pytorch-1.7-jetpack-4.4.1.patch``
| ``$ export USE_NCCL=0``
| ``$ export USE_DISTRIBUTED=0``
| ``$ export USE_QNNPACK=0``
| ``$ export USE_PYTORCH_QNNPACK=0``
| ``$ export TORCH_CUDA_ARCH_LIST="5.3;6.2;7.2"``
| ``$ export PYTORCH_BUILD_VERSION=1.7.0``
| ``$ export PYTORCH_BUILD_NUMBER=1``
| (remember to re-export these environment variables if you change terminal)

| ``$ sudo apt-get install python3-pip cmake libopenblas-dev``
| ``$ pip3 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple``
| ``$ pip3 install scikit-build -i https://pypi.tuna.tsinghua.edu.cn/simple``
| ``$ pip3 install ninja -i https://pypi.tuna.tsinghua.edu.cn/simple``
| ``$ python3 setup.py bdist_wheel``
| 编译时间较久，可能需要7个小时,编译生成的 whl 在 dist 目录下
| ``$ ls dist/``
| ``torch-1.7.0-cp38-cp38-linux_aarch64.whl``
| ``$ pip3 install dist/torch-1.7.0-cp38-cp38-linux_aarch64.whl``


编译torchvision
----------------

| 版本需要与 torch 匹配，https://github.com/pytorch/vision 有版本匹配列表
| torch1.7.0 对应的 torchvision是0.8.1

| ``$ git clone -b v0.8.1 https://github.com/pytorch/vision.git``
| ``$ cd vision``

| ``$ pyenv shell 3.8.5``
| ``$ pip3 install pillow -i https://pypi.tuna.tsinghua.edu.cn/simple``
| ``$ export FORCE_CUDA=1``
| ``$ export BUILD_VERSION=0.8.1``
| ``$ python3 setup.py bdist_wheel``
| 编译会报错，参考文档 https://blog.csdn.net/weixin_41010198/article/details/109862917
| ``$ ls dist/``
| ``torchvision-0.8.1-cp38-cp38-linux_aarch64.whl``
| ``$ pip3 install dist/torchvision-0.8.1-cp38-cp38-linux_aarch64.whl``
| （如果要卸载：pip3 uninstall torchvision)


安装yolov5
-----------

`install requirements <https://github.com/ultralytics/yolov5/blob/master/requirements.txt>`_

| ``$ cd ~/hyun/``
| ``$ git clone https://github.com/ultralytics/yolov5.git``

| ``$ pyenv shell 3.8.5``
| ``$ pip install matplotlib``
| ``$ pip install opencv-python``
| ``$ pip install scipy``
| ``$ pip install tqdm``
| ``$ pip install pyyaml``
| ``$ pip install tensorboard``
| ``$ pip install pandas``
| ``$ pip install seaborn``
| ``$ pip install thop``
| (有部分库没装)


yolov5测试
-----------

| ``$ cd yolov5``
| ``$ python3 detect.py --source data/images --img-size 1280 --weights weights/last.pt --conf-thres 0.1 --iou-thres 0.3``
| (注意：将detect.py中的check_requirements注释掉，否则会检查版本自动下载)


问题汇总
---------

1. torch 的第三方模块下载不下来

 ::

        Cloning into '/home/robot/hyun/pytorch/third_party/FXdiv'...
        fatal: unable to access 'https://github.com/Maratyszcza/FXdiv.git/': gnutls_handshake() failed: The TLS connection was non-properly terminated.
        fatal: clone of 'https://github.com/Maratyszcza/FXdiv.git' into submodule path '/home/robot/hyun/pytorch/third_party/FXdiv' failed
        Failed to clone 'third_party/FXdiv'. Retry scheduled
        ...


 | **解决**
 | 在vpn电脑上把torch clone下来，scp到NX上


2. 模块找不到

 ::

        import torchvision
        ERROR: NO module named _lzma


 | **解决**
 | ``$ sudo apt-get install liblzma-dev``
 | ``$ pip3 install backports.lzma``

 参考文档：https://github.com/ultralytics/yolov5/issues/1298

