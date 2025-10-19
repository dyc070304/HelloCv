 ROS2和OpenCV的计算机视觉开发环境配置与学习记录

 环境配置总览

本项目记录了在 Ubuntu 22.04 系统中配置 ROS2 Humble 和 OpenCV 4.5.5 开发环境的完整过程。

---

 VS Code 安装

 安装步骤
1. 安装 VS Code
   ```bash
   sudo snap install --classic code
```

1. 安装扩展
   · C++ 扩展
   · Chinese (Simplified) 中文语言包
2. 验证安装
   ```bash
   code --version
   ```
   终端输入 code 即可启动 VS Code

---

ROS2 Humble 安装

系统要求

· Ubuntu 22.04

安装步骤

1. 系统版本检查
   ```bash
   lsb_release -a
   ```
2. 系统更新
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
3. 安装必要工具
   ```bash
   sudo apt install curl gnupg lsb-release software-properties-common
   ```
4. 配置语言环境
   ```bash
   export LANG=en_US.UTF-8
   export LC_ALL=en_US.UTF-8
   ```
5. 添加软件仓库
   ```bash
   sudo add-apt-repository universe
   ```
6. 添加 ROS2 仓库
   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
   ```
7. 更新软件包列表
   ```bash
   sudo apt update
   ```
8. 安装 ROS2 Humble
   ```bash
   sudo apt install ros-humble-desktop
   ```
9. 设置环境变量
   ```bash
   echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   ```
10. 安装构建工具和依赖管理
    ```bash
    sudo apt install python3-colcon-common-extensions
    sudo apt install python3-rosdep2
    sudo rosdep init
    rosdep update
    ```

---

 OpenCV 4.5.5 安装

OpenCV 简介

OpenCV 是一个开源的计算机视觉和机器学习软件库，包含大量视觉算法，支持 C++、Python 等语言，可用于图像处理、视频分析等任务。

安装步骤

1. 安装依赖包
   ```bash
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
   ```
2. 下载 OpenCV 源代码
   ```bash
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
   ```
3. 配置和编译
   ```bash
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
       -D BUILD_PERF_TESTS=OFF ..
   
   make -j$(nproc)
   ```
4. 安装 OpenCV
   ```bash
   sudo make install
   sudo ldconfig
   ```
5. 配置环境变量
   ```bash
   echo "/usr/local/lib" | sudo tee /etc/ld.so.conf.d/opencv.conf
   sudo ldconfig
   echo "export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:\$PKG_CONFIG_PATH" >> ~/.bashrc
   source ~/.bashrc
   ```

---

验证安装

验证 ROS2 安装

```bash
ros2 --version
```

验证 OpenCV 安装

```bash
pkg-config --modversion opencv4
```

---

 说明

本安装指南在 DeepSeek 的指导下完成，所有步骤均经过实际验证。作者对每个命令进行了阅读和理解，确保安装过程的正确性和安全性。

```



































解密文件的构建与运行：
(1):
文本加密解密：凯撒密码算法
文件加密解密
 
跨平台构建使用CMake管理项目
面向对象设计

项目结构

```
CryptoTool/
├── CMakeLists.txt              
├── include/                   
│   ├── Crypto.h               
│   ├── FileHandler.h          
│   └── Menu.h                 
├── src/                       
│   ├── main.cpp              
│   ├── Crypto.cpp            
│   ├── FileHandler.cpp       
│   └── Menu.cpp              
├── build/                     
├── input.txt                 
├── encrypted.txt         
└── decrypted.txt       
```

核心类设计

Crypto类 - 加密解密核心

class Crypto {
public:
    std::string caesarEncrypt(const std::string& text, int key);
    std::string caesarDecrypt(const std::string& text, int key);
};


职责：实现凯撒密码算法，处理字符串的加密和解密逻辑。

FileHandler类 - 文件操作


class FileHandler {
public:
    std::string readFile(const std::string& filepath);
    bool writeFile(const std::string& filepath, const std::string& content);
};
```

职责：负责文件的读取和写入操作，提供文件IO功能。

Menu类 - 用户界面


class Menu {
public:
    void showMainMenu();
    void handleChoice(int choice);
private:
    Crypto crypto;
    FileHandler fileHandler;
    // 私有方法处理具体功能
};


职责：显示命令行菜单，处理用户输入，协调加密解密和文件操作。



