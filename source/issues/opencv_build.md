# [Ubuntu 20.04][Anaconda3]OpenCV-4.7.0安装

> Nvidia Tao 4.0.1 默认安装python为3.6不支持最新Opencv 需更新为python3.7 但是Tao不支持3.7版本
> https://visionguide.readthedocs.io/zh_CN/latest/opencv/configure/4.4.0/安装/

## 安装文档

- [OpenCV installation overview](https://docs.opencv.org/4.7.0/d0/d3d/tutorial_general_install.html)
- [OpenCV configuration options reference](https://docs.opencv.org/4.7.0/db/d05/tutorial_config_reference.html)
- [Installation in Linux](https://docs.opencv.org/4.7.0/d7/d9f/tutorial_linux_install.html)

## 依赖

安装以下依赖文件，其中使用`Anaconda`安装`Python`相关依赖：

```
# For C++
[compiler] sudo apt-get install build-essential
[required] sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
[optional] sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
# For Python
pip install numpy
[required] sudo apt install cmake gcc g++ libavcodec-dev libavformat-dev libswscale-dev libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev libgtk2.0-dev libgtk-3-dev
[optional] sudo apt-get install libpng-dev libjpeg-dev libopenexr-dev libtiff-dev libwebp-dev
```

## 源码

同一路径下下载`OpenCV`以及`OpenCV_Contrib`源码

```
$ git clone https://github.com/opencv/opencv.git
$ git clone https://github.com/opencv/opencv_contrib.git
```

切换到`4.4.0`版本

```
$ cd opencv
```

## 编译

在`opencv/opencv_contrib`同一路径下新建文件夹`build/install`，分别用于存放构建文件以及编译文件

```
$ mkdir build
$ mkdir install
$ ls
build  install  opencv  opencv_contrib
```

官网给出的安装流程如下：

```shell
# Install minimal prerequisites (Ubuntu 18.04 as reference)
sudo apt update && sudo apt install -y cmake g++ wget unzip
# Download and unpack sources
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.x.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.x.zip
unzip opencv.zip
unzip opencv_contrib.zip
# Create build directory and switch into it
mkdir -p build && cd build
# Configure
cmake -DOPENCV_EXTRA_MODULES_PATH=../opencv_contrib-4.x/modules ../opencv-4.x
# Build
cmake --build .
```

在Configure阶段，我增加了一些安装选项，包括安装路径、Python安装等

```shell
cmake -D CMAKE_INSTALL_PREFIX=../install \
                -D OPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules \
                -D BUILD_opencv_python3=ON \
                -D PYTHON3_EXECUTABLE=~/anaconda3/bin/python \
                -D PYTHON_LIBRARIES=~/anaconda3/lib \
                -D PYTHON_INCLUDE_DIRS=~/anaconda3/include \
                ..
```

然后执行编译和安装

```shell
cmake --build . && make -j12
```

```bash
sudo make install	
```

