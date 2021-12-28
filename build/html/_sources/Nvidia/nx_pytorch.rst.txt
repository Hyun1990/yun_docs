Jetson NX Pytorch 部署
=======================

Install pyenv
--------------

| 克隆pyenv仓库
| ``$ git clone https://github.com/yyuu/pyenv.git ~/.pyenv``
|
| 将PYENV_ROOT和pyenv init 加入bash的~/.bashrc
| ``$ echo 'export PATH=~/.pyenv/bin:$PATH' >> ~/.bashrc``
| ``$ echo 'export PYENV_ROOT=~/.pyenv' >> ~/.bashrc``
| ``$ echo 'eval "$(pyenv init -)"' >> ~/.bashrc``
|
| 激活pyenv
| ``$ source ~/.bashrc``
|
| 安装python环境
| ``$ pyenv versions``
| ``$ mkdir /home/robot/.pyenv/cache``
| ``$ cd /home/robot/.pyenv/cache``
| ``$ wget https://www.python.org/ftp/python/3.6.9/Python-3.6.9.tar.xz``
| ``$ sudo apt-get install -y libssl-dev libbz2-dev libreadline-dev libsqlite3-dev libffi-dev liblzma-dev``
| ``$ pyenv install 3.6.9``
|
| 配置python环境
| ``$ cd /home/robot/hyun/``
| ``$ sudo apt-get install python3-pip libopenblas-base libopenmpi-dev libopenblas-dev``
| ``$ pyenv shell 3.6.9``
|
| 安装pytorch
| `下载对应的pytorch whl文件 <https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-7-0-now-available/72048>`_
| ``$ pip3 install --user ./pytorch/torch-1.6.0-cp36-cp36m-linux_aarch64.whl``
|
| 安装torchvision(版本需要与 torch 匹配，https://github.com/pytorch/vision 有版本匹配列表)
| ``$ cd hyun``
| 下载源码
| ``$ git clone https://github.com/pytorch/vision.git``
| ``$ cd vision``
| ``$ git checkout -b e_v0.7.0 v0.7.0``
| 安装pillow
| ``$ pip3 install pillow --user -i https://pypi.tuna.tsinghua.edu.cn/simple``
| 编译
| ``$ export FORCE_CUDA=1``
| ``$ pip3 install whell``
| ``$ sudo python3 setup.py bdist_wheel``
| 安装
| ``$ pip3 install --user dist/torchvision-0.7.0a0+78ed10c-cp38-cp38-linux_aarch64.whl``

**生成的torch和torchision的.whl文件可以保存起来，以便下次环境安装**

参考文档
---------
| `Jetson Xavier NX上安装torchvision编译报错 <https://blog.csdn.net/weixin_41010198/article/details/109862917>`_
| `Python多版本环境管理之pyenv <https://blog.51cto.com/rainbird/2480244>`_




