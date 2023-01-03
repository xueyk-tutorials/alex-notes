# gstreamer笔记

## 参考

python实现rtsp推流：https://blog.csdn.net/JulyLi2019/article/details/118898957



https://blog.csdn.net/m0_51004308/article/details/120986180



## 查看gstreamer版本

```shell
$ dpkg -l | grep gstreamer
```



```shell
$ sudo apt-get install build-essential
$ sudo apt-get install gnome-devel gnome-devel-docs
```



## 命令行安装

1、安装[gstreamer](https://so.csdn.net/so/search?q=gstreamer&spm=1001.2101.3001.7020)

```shell
$ sudo apt-get install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio
```

2、安装gst-rtsp-server

```shell
$ sudo apt-get install libgstrtspserver-1.0-dev gstreamer1.0-rtsp
```

3、安装python3-gi

```shell
$ sudo apt-get install python3-gi
```

4、再安装一堆东西

```shell
$ sudo apt-get install pkg-config libcairo2-dev gcc python3-dev libgirepository1.0-dev
```

5、安装python库gobject和PyGObject

```shell
$ pip install gobject PyGObject
```

6、最后测试一下

```python
import gi
gi.require_version('Gst', '1.0')
gi.require_version('GstRtspServer', '1.0')
from gi.repository import Gst, GstRtspServer, GObject, GLib
```



## 源码安装（未测试）

1.16版本前面较老的版本是使用makefile构建工程，较新的版本都是使用meson构建的工程。

### makefile构建

```shell
$ bash autogen.sh
$ make
```



### meson构建

#### 安装meson

```shell
$ sudo apt-get install python3 python3-pip python3-setuptools \python3-wheel ninja-build
$ pip3 install meson ninja
```

#### 编译

进入工程目录（其中包含meson.build文件）

```shell
$ meson build # 生成build文件夹
$ cd build
$ ninja        # 相当于make
```



$ meson setup build

