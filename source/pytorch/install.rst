Installation
=============

编译在nvidia NX上
------------------

**拉区代码**
| ``$ sudo apt-get install python3-pip cmake libopenblas-dev``
| ``$ cd ~/esy/``
| ``$ git clone --recursive http://github.com/pytorch/pytorch.git``
| (或者使用 ``$tar -zx -f pytorch-withmodules-20200914.tar.gz``)
| ``cd pytorch``
| ``$ git checkout -b e_v1.6.0-rc2 v1.6.0-rc2``
| ``$ git submodule sync --recursive``
| ``$ git submodule update --init --recursive``
| (``cd third_party/QNNPACK/; git checkout master``)

**编译**

| ``$ pyenv shell 3.8.5``
| ``$ pip3 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple``
| ``$ pip3 install scikit-build --user -i https://pypi.tuna.tsinghua.edu.cn/simple``
| ``$ pip3 install ninja --user -i https://pypi.tuna.tsinghua.edu.cn/simple``
| ``$ patch -p1 < ~/esy/pytorch-1.6-rc2-jetpack-4.4-GA.patch``
| ``$ export USE_NCCL=0``
| ``$ export USE_DISTRIBUTED=0``
| ``$ export USE_QNNPACK=0``
| ``$ export USE_PYTORCH_QNNPACK=0``
| ``$ export TORCH_CUDA_ARCH_LIST="5.3;6.2;7.2"``
| ``$ export PYTORCH_BUILD_VERSION=1.6.0``
| ``$ export PYTORCH_BUILD_NUMBER=1``
| ``$ python3 setup.py bdist_wheel``

编译生成的 whl 在 dist 目录下

| ``$ ls dist/``
| torch-1.6.0-cp38-cp38-linux_aarch64.whl
| ``$ pip3 install --user dist/torch-1.6.0-cp38-cp38-linux_aarch64.whl``

**安装torchvision**

torchvision 版本需要与torch版本匹配，https://github.com/pytorch/vision 有版本匹配列表

| ``$ cd ~/esy/``
| ``$ git clone https://github.com/pytorch/vision.git``
| (或者使用 ``$ tar -zx -f torchvision-20201012.tar.gz``)
| ``cd vision``
| ``$ git checkout -b e_v0.7.0 v0.7.0``
|
| ``$ pyenv shell 3.8.5``
| ``$ pip3 install pillow --user -i https://pypi.tuna.tsinghua.edu.cn/simple``
| ``$ export FORCE_CUDA=1``
| ``$ python3 setup.py bdist_wheel``
| ``$ ls dist/``
| torchvision-0.7.0a0+78ed10c-cp38-cp38-linux_aarch64.whl
| ``$ pip3 install --user dist/torchvision-0.7.0a0+78ed10c-cp38-cp38-linux_aarch64.whl``

参考文档
---------

`Pytorch for Jetson <https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-7-0-now-available/72048>`_
`pytorch patch <https://gist.github.com/dusty-nv/ce51796085178e1f38e3c6a1663a93a1#file-pytorch-1-6-rc2-jetpack-4-4-ga-patch>`_

