## 基本环境准备

### 查看python版本

查看所有已经安装的python版本：**whereis python**

```shell
$ whereis python
"""
### Jetson AGX
python: /usr/bin/python3.6m /usr/bin/python3.6m-config /usr/bin/python3.6 /usr/bin/python3.6-config /usr/bin/python2.7-config /usr/bin/python /usr/bin/python2.7 /usr/lib/aarch64-linux-gnu/python2.7 /usr/lib/python3.7 /usr/lib/python3.6 /usr/lib/python3.8 /usr/lib/python2.7 /etc/python3.6 /etc/python /etc/python2.7 /usr/local/lib/python3.6 /usr/local/lib/python2.7 /usr/include/python3.6m /usr/include/python3.6 /usr/include/python2.7_d /usr/include/python2.7 /usr/share/python
"""
```

查看正在使用的python版本：**which python**

```shell
$ which python
/usr/bin/python
```

### 查看opencv版本

#### 查看安装版本

```shell
$ pkg-config opencv --modversion
```

#### 查看安装库

```shell
$ pkg-config opencv --libs
```

#### 查看安装路径

```shell
$ sudo find / -iname "*opencv*"

$ sudo find / -iname "*opencv*" > /home/ubuntu/Desktop/opencv_find.txt
```





### pip3

```
sudo apt install python3-pip
```

### CUDA

jetson nano默认已经安装了CUDA10.0，但是直接运行 nvcc -V是不会成功的，需要你把CUDA的路径写入环境变量中。

1）编辑.bashrc文件

gedit  ~/.bashrc
2）在最后添加

```
export CUDA_HOME=/usr/local/cuda-10.0
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda-10.0/bin:$PATH
#或者
export PATH=/usr/local/cuda-10.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-10.0
```


 然后保存退出

3）最后别忘了source一下这个文件。

source ~/.bashrc
到这里CUDA就导入成功了

使用nvcc -V命令

```
dnano@dnano-desktop:~$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sun_Sep_30_21:09:22_CDT_2018
Cuda compilation tools, release 10.0, V10.0.166
```

实例测试

官方镜像安装的系统中自带了一些案例，利用已有的案例进行测试。

```
sudo apt-get install libfreeimage3 libfreeimage-dev
cd /usr/src/cudnn_samples_v7/mnistCUDNN
sudo make
sudo chmod a+x mnistCUDNN
./mnistCUDNN
```

执行完上述命令，如果最后出现Test passed! 证明验证成功。

### jtop

更简单的方法是通过**jetson-stats**工具，安装：

```
$sudo apt-get update
#install
$sudo -H pip3 install jetson-stats
#uninstall
$sudo -H pip3 install -U jetson-stats
```

使用：

命令：jtop

命令：sudo jetson_release



## RPi摄像头

NANO用的树莓派需要是IMX219 sensor，选择v2.0及以上版本的，价格150左右。注意，一般淘宝上几十元的不能用，不要买。

```shell
gst-launch-1.0 nvarguscamerasrc sensor_id=0 ! nvoverlaysink
```

## OpenCV源码编译安装

jetson nano预先安装了jetpack，并且集成了opencv（4.1.1），但这个没有cuda加速（需要另外安装）。可以在终端通过如下命令确定opencv的安装情况：

```
python3
>> import cv2
>> cv2.__version__
```

### 系统镜像源配置

编辑文件`/etc/apt/sources.list`，使用中科大源如下：

```shell
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-updates main restricted
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-updates universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-updates multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-security main restricted
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-security universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-security multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-security main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-security main restricted universe multiverse
deb http://ports.ubuntu.com/ubuntu-ports/ bionic universe
```

> 最好先把默认的源备份：
>
> $ sudo cp /etc/apt/sources.list  /etc/apt/sources.list.bak

### 更新

```shell
sudo apt-add-repository universe
sudo apt-get update
```

### 安装依赖

- 基本必备

```shell
sudo apt-get install -y \
    build-essential \
    cmake \
    libavcodec-dev \
    libavformat-dev \
    libavutil-dev \
    libeigen3-dev \
    libglew-dev \
    libgtk2.0-dev \
    libgtk-3-dev \
    libjpeg-dev \
    libpng-dev \
    libpostproc-dev \
    libswscale-dev \
    libtbb-dev \
    libtiff5-dev \
    libv4l-dev \
    libxvidcore-dev \
    libx264-dev \
    qt5-default \
    zlib1g-dev \
    pkg-config
sudo apt-get install -y  make cmake-curses-gui git g++ pkg-config curl
sudo apt-get install -y libtbb2   v4l-utils qv4l2 v4l2ucp
sudo apt-get install -y libjasper-dev
sudo apt-get install -y libjpeg8-dev libjpeg-turbo8-dev libtiff-dev libpng-dev
sudo apt-get install -y libatlas-base-dev libopenblas-dev liblapack-dev liblapacke-dev gfortran
```

- python

```shell
# Python 2.7
sudo apt-get install -y python-dev  python-numpy  python-py  python-pytest
# Python 3.6
sudo apt-get install -y python3-dev python3-numpy python3-py python3-pytest
```

- GStreamer

```shell
# GStreamer support
sudo apt-get install -y libdc1394-22-dev libxine2-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
```

### 下载opencv

这里我们下载4.1.1。

也可以在github上直接下对应版本的压缩包，具体方法是找到opencv或opencv_contrib仓库主页，点击**tags**，然后可以选择**release**，就可以找对应版本了，点击下载**.tar.gz**压缩包。

**opencv:** https://github.com/opencv/opencv/releases

**opencv-contrib:** https://github.com/opencv/opencv_contrib/releases

我们将下载的两个压缩包放到同级目录下并解压。

```shell
$ cd /home
$ mkdir opencv_install
$ tar -xvf opencv-4.1.1
$ tar -xvf opencv_contrib-4.1.1
```

解压opencv和opencv_contrib后。

将文件夹*opencv_3rdparty-contrib_xfeatures2d_boostdesc_20161012*和文件夹*opencv_3rdparty-contrib_xfeatures2d_vgg_20160317*内的所有文件拷贝至**opencv_contrib-4.1.0/modules/xfeatures2d/src/**路径下。

注意提前将[opencv_3rdparty](https://github.com/opencv/opencv_3rdparty)下的[boostdesc相关文件](https://github.com/opencv/opencv_3rdparty/tree/contrib_xfeatures2d_boostdesc_20161012)与[vgg相关文件](https://github.com/opencv/opencv_3rdparty/tree/contrib_xfeatures2d_vgg_20160317)下载并保存至opencv_contrib/modules/xfeatures2d/src/路径下。

### 编译

编译前请安装**jetson_stats**，命令行运行jtop后查看cuda版本、cudnn版本。

```shell
cmake -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=/usr/local \
      -D WITH_CUDA=ON \
      -D WITH_CUDNN=ON \
	  -D OPENCV_DNN_CUDA=ON \
      -D CUDNN_VERSION='8.2' \
      -D CUDNN_INCLUDE_DIR='/usr/include/' \
      -D CUDA_ARCH_BIN=5.3 \
      -D CUDA_ARCH_PTX="" \
      -D WITH_CUBLAS=ON \
      -D ENABLE_FAST_MATH=ON \
      -D CUDA_FAST_MATH=ON \
      -D ENABLE_NEON=ON \
      -D WITH_GSTREAMER=ON \
      -D WITH_GSTREAMER_0_10=OFF \
      -D WITH_LIBV4L=ON \
      -D WITH_V4L=ON \
      -D BUILD_opencv_python2=OFF \
      -D BUILD_opencv_python3=ON \
	  -D HAVE_opencv_python3=ON \
	  -D PYTHON_DEFAULT_EXECUTABLE=/usr/bin/python3 \
	  -D PYTHON3_NUMPY_INCLUDE_DIRS=/usr/local/lib/python3.6/dist-packages/numpy/core/include/ \
      -D BUILD_TESTS=OFF \
      -D BUILD_PERF_TESTS=OFF \
      -D BUILD_EXAMPLES=OFF \
      -D WITH_QT=ON \
      -D WITH_OPENGL=ON \
      -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.1.1/modules ..
```

或者在build文件夹下创建my_cmake.sh文件，添加以上内容后，执行：

```
$chmod u+x my_cmake.sh
$./my_cmake.sh
```

### 安装

```shell
$ make -j8
$ sudo make install
```

安装后，在/usr/local/lib/pkgconfig文件夹下会生成opencv4.pc文件，将其克隆到/usr/lib/pkgconfig。

上面cmake指定的安装路径为： `/usr/local` ，所有文件将拷贝至如下目录下：

- `/usr/local/bin` - executable files
- `/usr/local/lib` - libraries (.so)
- `/usr/local/cmake/opencv4` - cmake package
- `/usr/local/include/opencv4` - headers
- `/usr/local/share/opencv4` - other files

### 测试

opencv自带了很多测试例程，都在**opencv-4.1.0/samples**下，包含了很多不同的文件夹，每个文件夹通过名字可以大致判断其中示例程序的功能，如gpu文件夹中包含了gpu处理如解码相关的。

但默认并不是所有示例都进行编译，需要在CMakeLists.txt下进行修改，需要编译的取消注释即可：

```C
#===================================================================================================
#  Standalone mode
#===================================================================================================

add_subdirectory(dnn)
add_subdirectory(gpu)
add_subdirectory(opencl)
# add_subdirectory(opengl)
# add_subdirectory(openvx)
add_subdirectory(tapi)
```

编译命令为：

```shell
$ cd opencv-4.1.0/samples
$ mkdir build
$ cd build
$ cmake ..
$ make
```

也可单独编译某一个，在**opencv-4.1.0/samples/cpp/example_cmake**中执行如下：

```
$ cmake .
$ make
$ sudo ./opencv_example
```

注意，如果是在`opencv-4.1.0/samples`中执行编译，这所有子文件夹内的都将依次编译，比较耗费时间。

> 编译问题与解决：
>
> 1、surf相关
>
> 提示：undefined reference to `cv::cuda::SURF_CUDA::SURF_CUDA()
>
> 解决：把opencv-4.1.0/samples/gpu/surf_keypoint_matcher.cpp删除再编译。

### 卸载

进入opencv编译时建立的build文件夹后：

```
cd /home/***/opencv/build
sudo make uninstall
cd  ..
sudo rm -r build
```

### 问题及解决

#### 缺少文件**boostdesc_bgm.i**

这是因为CMake过程中服务器无法连接很多文件无法下载导致的，可以查看opencv/

参考：<https://answers.opencv.org/question/174456/about-build-opencv_contribute-fatal-error-boostdesc_bgmi-and-vgg/>

解决方式一：

boostdesc相关文件：<https://github.com/opencv/opencv_3rdparty/tree/contrib_xfeatures2d_boostdesc_20161012>

vgg相关文件：<https://github.com/opencv/opencv_3rdparty/tree/contrib_xfeatures2d_vgg_20160317>

将下载后的文件保存至opencv_contrib/modules/xfeatures2d/src/，然后重新make

### 其他安装方式

#### apt安装

这种安装无法使用cuda。

```shell
sudo apt-get install python3-opencv
sudo apt-get remove python3-opencv
```

#### 通过JetsonHacksNano

JetsonHacks是一个专门基于NVIDIA jetson开发机器人的兴趣组织。

**step1:** 扩大jetson nano的内存

```
git clone https://github.com/JetsonHacksNano/installSwapfile
sudo ./installSwapfile.sh 
```

**step2:**下载编译OpenCV

```
git clone https://github.com/JetsonHacksNano/buildOpenCV
sudo ./buildOpenCV.sh
```

由于buildOpencv.sh中包含从GitHub下载OpenCV源码的步骤，太耗时了，我们可以预先下载好OpenCV（比如opencv3.4.0，opencv_contrib3.4.0），然后将二者放到目录**$HOME**下，即与**Desktop**所在的相同目录，然后分别将opencv3.4.0重命名为opencv，将opencv_contrib3.4.0重命名为opencv_contrib。

并且将*buildOpencv.sh*如下两行注释掉：

```
#git clone --branch "$OPENCV_VERSION" https://github.com/opencv/opencv.git
#git clone --branch "$OPENCV_VERSION" https://github.com/opencv/opencv_contrib.git
```

#### tiny-dnn下载不下来

由于一直存在的问题：在github下载东西贼慢，所以总会使得下载超时，这里给一个超级快的加速网站： https://wjx.wiki/service/accelerator/index

再将下载好的文件替换掉opencv文件夹里面的**.cahce**（查找一下就好啦）中的tiny_dnn的压缩包，注意原本.cahce中的tiny_dnn不叫这个名字，而是一长串的英文字母，把下载好的tiny_dnn放进这个文件夹后，要把名字修改成那原本的一长串英文字母。

#### failed to load module canberra-gtk-module

sudo apt-get install libcanberra-gtk-module

#### GStreamer warning

如果使用jetson自带的opencv，打开USB摄像头时可能会遇到：[WARN:0] global /home/nvidia/host/build_opencv/nv_opencv/modules/videoio/src/cap_gstreamer.cpp handleMessage opencv|GStreamer warning:Embedded video playback halted;module source reported: Could not read from resource

You are using the built-in version of OpenCV. In the case of nvidia systems, the OpenCV version installed by default has not support for GStreamer or FFMpeg, so, you have to compile and install your own version of OpenCV with those capabilities (and dependencies) if you need to open camera streams.

### opencv安装问题

#### 镜像源的问题

如果出现依赖安装的包无法找到，可以尝试切换默认的镜像源，如果使用国内的镜像源有可能导致如下问题：

```shell
Package libv4l-dev is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source

E: Package 'libgtk2.0-dev' has no installation candidate
E: Package 'libv4l-dev' has no installation candidate
alex@alex-desktop:~/opencv_install$ 
```

#### Eigen问题

```shell
[ 21%] Built target ade
Scanning dependencies of target opencv_cudaarithm_pch_dephelp
In file included from /home/alex/opencv_install/opencv-4.1.1/modules/core/src/precomp.hpp:55:0,
                 from /home/alex/opencv_install/opencv-4.1.1/build/modules/core/opencv_core_pch_dephelp.cxx:1:
/home/alex/opencv_install/opencv-4.1.1/modules/core/include/opencv2/core/private.hpp:66:12: fatal error: Eigen/Core: No such file or directory
 #  include <Eigen/Core>
```

解决方法：

```shell
# Patch the Eigen library issue ...
cd opencv-4.1.1
sed -i 's/include <Eigen\/Core>/include <eigen3\/Eigen\/Core>/g' modules/core/include/opencv2/core/private.hpp

```

#### OpenGL问题

问题：

```shell
[ 34%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/opengl.cpp.o
In file included from /home/alex/opencv_install/opencv-4.1.1/modules/core/src/opengl.cpp:48:0:
/usr/local/cuda-10.2/include/cuda_gl_interop.h:63:2: error: #error Please include the appropriate gl headers before including cuda_gl_interop.h
 #error Please include the appropriate gl headers before including cuda_gl_interop.h
```

解决方法：

注释/usr/local/cuda/include/cuda_gl_interop.h

```cpp
//#ifndef GL_VERSION
//#error Please include the appropriate gl headers before including cuda_gl_interop.h
//#endif
//#else
```

#### 无法找到opencv

```
$ pkg-config opencv --libs
# 出现如下错误
Package opencv was not found in the pkg-config search path.
Perhaps you should add the directory containing `opencv.pc'
to the PKG_CONFIG_PATH environment variable
No package 'opencv' found
```

在opencv编译安装时：

```
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_GENERATE_PKGCONFIG=ON ..
```

最后一项很关键，会生成`opencv.pc`，安装后，在/usr/local/lib/pkgconfig文件夹下会生成opencv4.pc文件，将其克隆到/usr/lib/pkgconfig。

## pytorch

pytorch官方没有提供jetson的安装版本，但NVIDIA官方提供了编译后的版本，下载链接如下：

<https://forums.developer.nvidia.com/t/pytorch-for-jetson-nano-version-1-5-0-now-available/72048>

如果无法下载，可以通过部署在香港的阿里云进行云端下载然后转到本地。

### 安装pytorch

```
sudo pip3 install torch-1.1.0-cp36-cp36m-linux_aarch64.whl
```

### 安装torchvision

```
sudo apt-get install libjpeg-dev zlib1g-dev
# 下载torchvision
git clone -b v0.3.0 https://github.com/pytorch/vision torchvision
# 安装torchvision
cd torchvision
sudo python3 setup.py install
# 测试，安装完毕重启终端
import torchvision
print(torchvision.__version__)
```

### 问题及解决

#### numpy版本低

描述：pytorch no module named numpy.core._multiarray_umath

查看numpy版本：pip3 show numpy

解决方法：

升级pip3，升级numpy

```
python3 -m pip3 install --upgrade pip3
pip3 install -U numpy
```



## anaconda

由于jetson nano是arm架构，可以选择去下载archiconda，地址如下**https://github.com/Archiconda/build-tools/releases**，选择下载**[Archiconda3-0.2.3-Linux-aarch64.sh](https://github.com/Archiconda/build-tools/releases/download/0.2.3/Archiconda3-0.2.3-Linux-aarch64.sh)**，然后在终端运行：

```
bash Archiconda3-0.2.3-Linux-aarch64.sh
```

安装完毕，关闭终端并重新打开一个新的终端，输入如下指令验证是否安装成功：

```
conda --version
```

**备注**

anaconda安装好后会设置默认Python版本为conda内的Python，如果你需要调整，那么编辑文件**sudo gedit  ~/.bashrc**，将conda init之间的部分注释掉，然后**source ~/.bashrc**。重启终端生效。





