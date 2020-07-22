opencv install
===============

该文档实现在 ubuntu18.04 上安装opencv3

源码安装 opencv 和 opencv_contrib
-----------------------------------
| https://github.com/opencv/opencv/archive/3.4.7.tar.gz
| https://github.com/opencv/opencv_contrib/archive/3.4.7.tar.gz
|
| 可参考 `官方安装教程 <https://docs.opencv.org/3.4.7/d7/d9f/tutorial_linux_install.html>`_
| 

安装依赖
--------
| 编译器
| ``$ sudo apt-get install build-essential``
|
| 编译环境
| ``$ sudo apt-get install cmake git libgtk2.0-dev``
| ``pkg-config libavcodec-dev libavformat-dev libswscale-dev``
|
| 可选库

 ::
	
   python-dev 
   python-numpy 
   libtbb2 
   libtbb-dev 
   libjpeg-dev 
   libpng-dev 
   libtiff-dev 
   libjasper-dev 
   libdc1394-22-dev

| 其中安装了
| ``$ sudo apt-get install libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev``
|

编译
-----
| ``$ tar -zx -f opencv-3.4.7.tar.gz``
| ``$ tar -zx -f opencv_contrib-3.4.7.tar.gz``
| ``$ mkdir -p opencv-3.4.7/.cache``
| ``$ cp -r 3.4.7-download/cache/* opencv-3.4.7/.cache``
| ``$ cd opencv-3.4.7``
| ``$ mkdir build``
| ``$ cd build``
| 将 C++ 库安装在 /home/hyun/libraries/opencv-3.4.7
| 将 python3 cv2 安装在系统目录

  ::

	$ cmake \
		-D CMAKE_BUILD_TYPE=Release \
		-D OPENCV_ENABLE_NONFREE=ON \
		-D OPENCV_EXTRA_MODULES_PATH=/home/hyun/soft/opencv/opencv_contrib-3.4.7/modules \
		-D PYTHON3_EXECUTABLE=/usr/bin/python3 \
		-D PYTHON3_INCLUDE_DIR=/usr/include/python3.6m \
		-D PYTHON3_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.6m.so \
		-D PYTHON3_PACKAGES_PATH=/usr/lib/python3.6/dist-packages \
		-D CMAKE_INSTALL_PREFIX=/home/hyun/libraries/opencv-3.4.7 \
		-D ENABLE_PRECOMPILED_HEADERS=OFF \
		-D BUILD_JASPER=ON \
		-D BUILD_EXAMPLES=OFF \
		-D BUILD_TESTS=OFF \
		-D BUILD_opencv_java=OFF \
		-D BUILD_opencv_python2=OFF \
		-D BUILD_opencv_python3=ON \
		-D INSTALL_C_EXAMPLES=OFF \
		-D INSTALL_PYTHON_EXAMPLES=OFF \
		-D INSTALL_TESTS=OFF \
		-D WITH_OPENMP=ON \
		../

| ``$ make -j4``
| ``$ sudo make install``
|

 .. note::

	| 配置OpenCV
	| -D ENABLE_PRECOMPILED_HEADERS=OFF 是解决编译时找不到 Eigen/Core 文件的问题(eigen3已正确安装)
	| -D WITH_OPENMP=ON 是打开 OpenMP
	| -D WITH_OPENCL=ON 是打开 OpenCL
	| -D OPENCV_ENABLE_NONFREE=ON 是打开非自由算法的编译
	| -D WITH_OPENCL=ON 是打开 OpenCL 支持，需要提前安装好对应的显卡驱动，比较麻烦，先不开了。   
	| 编译 python-opencv 需要设置以下选项
	| (如果仅编译python2版本的或仅编译python3版本的，都设置成 PYTHON_xxxx 应该也可以加，建议单独设置)
	| PYTHON2(3)_EXECUTABLE = <path to python>
	| PYTHON_INCLUDE_DIR = /usr/include/python<version>
	| PYTHON_INCLUDE_DIR2 = /usr/include/x86_64-linux-gnu/python<version>
	| PYTHON_LIBRARY = /usr/lib/x86_64-linux-gnu/libpython<version>.so
	| PYTHON2(3)_NUMPY_INCLUDE_DIRS = /usr/lib/python<version>/dist-packages/numpy/core/include/
	| PYTHON2(3)_PACKAGES_PATH = /usr/lib/python<version>/dist-packages
	| python2 和 python3 可以单独设置 INCLUDE_DIR 和 LIBRARY
	| 实测在仅设置 PYTHON3_EXECUTABLE 时，能找到 PYTHON3_INCLUDE_DIR 和 PYTHON3_LIBRARY
	| python 的 cv2 模块的默认安装路径 CMAKE_INSTALL_PREFIX/lib/python3.6/dist-packages
	| PYTHON_EXECUTABLE 指定 python 路径
	| 可以使用以下命令获取
	| $ python3 -c "import sys; print(sys.executable)"
	| /usr/bin/python3
	| 首先 C++ 的 opencv 动态连接库需要编译出来，安装在某个目录 (只使用 python 版本，应该有不用安装 C++ 动态库的方法，未研究)
