# Jetson AGX Xaiver

## Docker配置

### 设置runtime

To enable access to the CUDA compiler (nvcc) during `docker build` operations, add `"default-runtime": "nvidia"` to your `/etc/docker/daemon.json` configuration file before attempting to build the containers:

```shell
{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    },
    "default-runtime": "nvidia"
}
```

重启docker

```shell
systemctl restart docker
```

#### docker镜像

- 拉取基础镜像

```shell
sudo docker pull nvcr.io/nvidia/l4t-base:r32.4.4
```

![image-20210909144824901](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210909144824901.png)

- 启动镜像

```shell
docker run -it -v /home/stoner/home_docker:/home nvcr.io/nvidia/l4t-base:r32.4.4 bash
```



#### 安装pip3

```shell
root@1e5015bc4c8b:/home# apt-get update
root@1e5015bc4c8b:/home# apt-get install pip3
```



#### 设置ubuntu镜像源

```shell
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse 
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse 
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse 
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse 
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse 
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse 
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic main universe restricted 
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ bionic main universe restricted

```

#### 设置pip3镜像源加速

```shell
# pip install -i https://mirrors.ustc.edu.cn/pypi/web/simple pip -U
# pip config set global.index-url https://mirrors.ustc.edu.cn/pypi/web/simple
```



## opencv源码编译与安装

参考：

https://github.com/JetsonHacksNano/buildOpenCV/blob/master/buildOpenCV.sh

https://github.com/dusty-nv/jetson-containers/blob/master/Dockerfile.opencv

### 查看CUDA相关配置

查看cudnn路径：

```shell
$ whereis cudnn_version    #注意v8.0版本前使用whereis cudnn
```

cuda安装路径

```shell
/usr/local/cuda
/usr/local/cuda-10.2
```

### 下载

这里我们下载4.1.1。

也可以在github上直接下对应版本的压缩包，具体方法是找到opencv或opencv_contrib仓库主页，点击**tags**，然后可以选择**release**，就可以找对应版本了，点击下载**.tar.gz**压缩包。

**opencv:** https://github.com/opencv/opencv/releases

**opencv-contrib:** https://github.com/opencv/opencv_contrib/releases

我们将下载的两个压缩包放到同级目录下并解压。

### 填坑

先填坑，解决下后续编译可能出现的错误。

- Eigen相关问题

```shell
# Patch the Eigen library issue ...
cd opencv-4.1.1
sed -i 's/include <Eigen\/Core>/include <eigen3\/Eigen\/Core>/g' modules/core/include/opencv2/core/private.hpp
```

- OpenGL

打开/usr/local/cuda/include/cuda_gl_interop.h

对如下几行进行注释：

```cpp
//#ifndef GL_VERSION
//#error Please include the appropriate gl headers before including cuda_gl_interop.h
//#endif
//#else
```

### 安装依赖

```shell
sudo apt-get update
sudo apt-get install -y build-essential cmake git pkg-config curl
sudo apt-get install -y libavcodec-dev 
    libavformat-dev \
    libswscale-dev \
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
    libv4l-dev \
    libtiff5-dev \
    libxvidcore-dev \
    libx264-dev \
    zlib1g-dev \
    qt5-default \
    libjasper-dev \
    libdc1394-22-dev \
    v4l-utils \
    qv4l2 \
    v4l2ucp

sudo apt-get install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
sudo apt-get install -y libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3

sudo apt-get install -y python2.7-dev python3.6-dev python-dev python-numpy python3-numpy
```

### 编译

```shell
cmake -D CPACK_BINARY_DEB=ON \ 
	-D WITH_CUDA=ON \
    -D WITH_CUDNN=ON \
	-D OPENCV_DNN_CUDA=ON \
    -D CUDNN_VERSION='8.0' \
    -D CUDNN_INCLUDE_DIR='/usr/include/' \
	-D ENABLE_PRECOMPILED_HEADERS=OFF \
	-D CUDA_ARCH_BIN="7.2" \
	-D CUDA_ARCH_PTX="" \
	-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.1.1/modules \
	-D WITH_GSTREAMER=ON \
	-D WITH_LIBV4L=ON \
	-D BUILD_opencv_python2=ON \
	-D BUILD_opencv_python3=ON \
	-D BUILD_NEW_PYTHON_SUPPORT=ON \
	-D HAVE_opencv_python3=ON \
	-D PYTHON_DEFAULT_EXECUTABLE=/usr/bin/python3 \
	-D PYTHON3_NUMPY_INCLUDE_DIRS=/usr/local/lib/python3.6/dist-packages/numpy/core/include/ \
    -D BUILD_TESTS=OFF \
	-D BUILD_PERF_TESTS=OFF \
	-D BUILD_EXAMPLES=OFF \
	-D CMAKE_BUILD_TYPE=RELEASE \
	-D CMAKE_INSTALL_PREFIX=/usr/local \
    -D WITH_CUBLAS=ON \
    -D ENABLE_FAST_MATH=ON \
    -D CUDA_FAST_MATH=ON \
    -D ENABLE_NEON=ON \
    -D WITH_OPENGL=ON \
    -D WITH_GSTREAMER_0_10=OFF \
    -D WITH_V4L=ON ..
```

讲解：

```shell
-D CPACK_BINARY_DEB=ON  #编译时打包为.deb文件，用于在其他电脑上去安装，不需要再重新编译
```



### 安装

```shell
make -j8

sudo make install
```

### 生成pkg包

编译完成后可以生成安装包，这样可以在其他电脑上无需重新编译就可以安装了。

```shell
$ cd opencv/build 
$ sudo make package
$ tar -czvf OpenCV-4.4.0-aarch64.tar.gz *.deb
```

### 通过pkg包安装

#### 解压

```shell
root@1e5015bc4c8b:/home# tar -xvf OpenCV-4.4.0-aarch64.tar.gz 
```

#### 安装

![image-20210909162301981](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210909162301981.png)

### 问题和解决

#### 查看NVIDIA显卡信息

- cuda版本

  ```shell
  $ nvcc -V
  ```

- cudnn版本

  ```shell
  cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
  ```

- 算力

  https://developer.nvidia.com/cuda-gpus

#### xfeature2d相关库无法下载

opencv在编译的过程中，会在**opencv-4.1.1**下生成一个**.cache**缓存目录，用于存放下载的内容：

```shell
.cache/xfeature2d/
    ├───boostdesc
    │ 0ae0675534aa318d9668f2a179c2a052-boostdesc_lbgm.i
    │ 0ea90e7a8f3f7876d450e4149c97c74f-boostdesc_bgm.i
    │ 202e1b3e9fec871b04da31f7f016679f-boostdesc_binboost_064.i
    │ 232c966b13651bd0e46a1497b0852191-boostdesc_bgm_bi.i
    │ 324426a24fa56ad9c5b8e3e0b3e5303e-boostdesc_bgm_hd.i
    │ 98ea99d399965c03d555cef3ea502a0b-boostdesc_binboost_128.i
    │ e6dcfa9f647779eb1ce446a8d759b6ea-boostdesc_binboost_256.i
    │
    └───vgg
        151805e03568c9f490a5e3a872777b75-vgg_generated_120.i
        7126a5d9a8884ebca5aea5d63d677225-vgg_generated_64.i
        7cd47228edec52b6d82f46511af325c5-vgg_generated_80.i
        e8d0dcd54d1bcfdc29203d011a797179-vgg_generated_48.i]
```

可以手动下载然后放到`opencv-4.1.1/.cache`即可，下载脚本如下：

```shell
#!/bin/bash
cd ./opencv-4.5.0/cache/xfeatures2d/
mkdir boostdesc
cd boostdesc

curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_lbgm.i > 0ae0675534aa318d9668f2a179c2a052-boostdesc_lbgm.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_binboost_256.i > e6dcfa9f647779eb1ce446a8d759b6ea-boostdesc_binboost_256.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_binboost_128.i > 98ea99d399965c03d555cef3ea502a0b-boostdesc_binboost_128.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_binboost_064.i > 202e1b3e9fec871b04da31f7f016679f-boostdesc_binboost_064.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_bgm_hd.i > 324426a24fa56ad9c5b8e3e0b3e5303e-boostdesc_bgm_hd.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_bgm_bi.i > 232c966b13651bd0e46a1497b0852191-boostdesc_bgm_bi.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_bgm.i > 0ea90e7a8f3f7876d450e4149c97c74f-boostdesc_bgm.i
cd ../vgg
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_120.i > 151805e03568c9f490a5e3a872777b75-vgg_generated_120.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_64.i > 7126a5d9a8884ebca5aea5d63d677225-vgg_generated_64.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_48.i > e8d0dcd54d1bcfdc29203d011a797179-vgg_generated_48.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_80.i > 7cd47228edec52b6d82f46511af325c5-vgg_generated_80.i
```



#### libjasper-dev无法安装

这个问题可以通过增加清华镜像源解决，在`/etc/apt/sources.list`最后添加：

```shell
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-security main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-security main restricted universe multiverse
```

或者在命令行中增加以上镜像源:

```shell
$ sudo add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main"
```

然后**sudo apt-get update**，再安装：

```shell
$ apt-get install libjasper1 libjasper.dev
```



# 硬件设置

## 开启风扇

```shell
sudo sh -c "echo 200 > /sys/devices/pwm-fan/target_pwm"
```



## 40Pin GPIO

jetson的40Pin GPIO与树莓派引脚一致。

### 串口

AGX的串口1就是`ttyTHS0`。

```shell
$ ls /dev/tty*
$ /dev/ttyTHS0   # UART1
```

引脚对应为：

```shell
pin 4 : V5+
pin 6 : GND
pin 8 : UART1_TX
pin 10: UART1_RX
```

> 备注：靠近电源指示灯那边是pin-1和pin-2.

调试工具：

```shell
### 安装
$sudo apt install cutecom
### 运行
$ cutecom
```

