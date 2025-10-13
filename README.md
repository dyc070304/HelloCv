ROS2和OpenCV的计算机视觉开发环境配置与学习记录

vscode安装
1.在ubuntu中安装vscode ，我通过DeepSeek使用了简单的方法，
sudo snap install --classic code
就可安装完成，
2。接下来在其中找到c++，以及中文版安装下来
3.接下来，在终端中输出code即可打开，也可以输入code --version检查版本

﻿
ROS2安装
一：
1.首先，确定版本，要求安装的是Ubuntu22.04，可以输入代码

lsb_release -a
进行版本检查。
2.进行必要工具准备：sudo apt update && sudo apt upgrade -y
这两步是对软件包进行更新。
3.安装密钥以及验证真实性；sudo apt install curl gnupg lsb-release software-properties-common
4.配置环境变量和语言环境；export LANG=en_US.UTF-8
export LANG=en_US.UTF-8
5.此代码是添加一个仓库ros2中的一些软件包在这个仓库中；sudo add-apt-repository universe

6.添加软件仓库echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
7.重新读取软件仓库（即第五步骤中的）内容，使得系统知道ros2安装包可用；
sudo apt update
8.设置环境变量，环境变量是对ros2命令进行说明；
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
9.构建colon系统，，用于编译ros2；
sudo apt install python3-colcon-common-extensions
sudo apt install python3-rosdep2
sudo rosdep init
rosdep update同时安装rosdep ros依赖于此进行管理。
以上即为安装全过程，本人是在DeepSeek指导下进行的，大部分指令为DeepSeek提供，但我进行了阅读和理解以及部分记忆，虽然尚未完全记住，但有必要进行说明。









OpenCV.4.5.x
一.opencv：
是一个开源的计算机视觉和机器学习软件库。包含了很多视觉算法。其中含有c++和java等编译语言。可以对图像进行很好处理，对视频进行分析等。
二：opencv安装：
1.系统准备：
对相应系统和工具进行准备。
2.安装依赖包：对opencv需要的库文件和编译语言进行安装

sudo apt install -y \
    libgtk-3-dev \
    libavcodec-dev libavformat-dev libswscale-dev \
    libv4l-dev libxvidcore-dev libx264-dev \
    libjpeg-dev libpng-dev libtiff-dev \
    libopenexr-dev \
    libtbb2 libtbb-dev \
    libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
sudo apt install -y python3-dev python3-numpy
sudo apt install -y gfortran libatlas-base-dev
sudo apt install -y libhdf5-dev libhdf5-serial-dev
sudo apt install -y libdc1394-dev || sudo apt install -y libdc1394-22-dev
3.下载opencv源代码：

mkdir -p ~/opencv_build
cd ~/opencv_build
git clone https://github.com/opencv/opencv.git
cd opencv
git checkout 4.5.5
cd ..
git clone https://github.com/opencv/opencv_contrib.git
cd opencv_contrib
git checkout 4.5.5
cd ..
4.配置和编译：
对opencv源代码进行编译并配置环境：

cd ~/opencv_build/opencv
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON \
    -D WITH_TBB=ON \
    -D WITH_V4L=ON \
    -D WITH_QT=ON \
    -D WITH_OPENGL=ON \
    -D WITH_CUDA=OFF \
    -D OPENCV_ENABLE_NONFREE=ON \
    -D BUILD_opencv_python3=ON \
    -D PYTHON3_EXECUTABLE=/usr/bin/python3 \
    -D BUILD_TESTS=OFF \
    -D BUILD_PERF_TESTS=OFF \
   
make -j$(nproc)
5.安装opencv

sudo make install
sudo ldconfig
6。配置环境


echo "/usr/local/lib" | sudo tee /etc/ld.so.conf.d/opencv.conf
sudo ldconfig
echo "export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:\$PKG_CONFIG_PATH" >> ~/.bashrc
source ~/.bashrc
即可完成安装。


