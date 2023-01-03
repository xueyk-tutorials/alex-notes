# Jetson 平台关于计算机视觉开发的简介

## Jetson平台介绍

Jetson集成了硬件编解码芯片，支持Gstreamer，并且有NV通过的OpenMax硬件加速的API 用于硬件编码、解码(OMX Gstreamer plug-ins, provided by Nvidia)。

一般的应用：
**video Input**： v4l2 （CSI UVC-camera、RTP/RTSP 流）
**video Output**：Display or proprietary framebuffer

**Multimedia APIs**:
\- gstreamer(high level) ： Hawdware Scalling, CODECs（omx）
\- NVIDIA L4T Multimedia API（low level）: video input，V4L2 API ，buffer management
\- OpenCV，Deep Learning Frameworks(TensorRT Yolo )
**GPU Integraton**
\- CUDA
\- OpenGL
\- Vulkan

encode

![image-20210913155402813](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210913155402813.png)



decode

![image-20210913155522640](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210913155522640.png)

## 相关专业术语介绍

V4L2 是专门为 linux 设备设计的一套视频框架

https://github.com/econsystems/opencv_v4l2/blob/master/opencv/opencv_install_script.bash



## 参考资料

### jetson ACCELERATED GSTREAMER

讲解了如何使用jetson自带的硬件编码器和解码器（NVENC和NVDEC）配合gstreamer使用。

[NVIDIA Jetson Linux Driver Package Software Features : Multimedia | NVIDIA Docs](https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra Linux Driver Package Development Guide/accelerated_gstreamer.html)

### python+gstreamer视频显示

#### 示例1

如何在jetson上用python显示视频，主要是tegra-cam.py文件，能够显示板载RPi摄像头、USB摄像头、rtsp视频流等。

https://jkjung-avt.github.io/tx2-camera-with-python/

#### 示例2

包含了c和python示例代码，能够获取两路板载RPi摄像头并显示。

https://github.com/JetsonHacksNano/CSI-Camera

### multimedia官方示例

https://docs.nvidia.com/jetson/l4t-multimedia/group__l4t__mm__test__group.html

例如如果你有视频需要解码显示，可以参考**02_video_dec_cuda**工程，

https://docs.nvidia.com/jetson/l4t-multimedia/l4t_mm_02_video_dec_cuda.html

```shell
 # make
 $ cd /usr/src/jetson_multimedia_api/samples/02_video_dec_cuda
 $ make
 # run
 $ ./video_dec_cuda <in-file> <in-format> [options]
 $ ./video_dec_cuda ../../data/Video/sample_outdoor_car_1080p_10fps.h264 H264
```

在Xaiver AGX上有如下编译错误：

jetson libv4l2_nvvidconv 1688 error NvDdkVicConfigure Failed

### 博客

在Jetson平台上使用硬件编码器一共有三种方式：

1. 直接使用deepstream：这种方式可以非常便利的体验硬件编码器的性能。并且由于deepstream使用了zero copy的pipeline，所以是目前您可以获得的最高性能的一个参考。
2. 使用multimedia API：为了方便开发者进行开发，NV提供了底层API来对编解码直接进行控制。因此可以参考下列文档，来进行开发。您同样可以享受到硬件编解码的性能。https://docs.nvidia.com/jetson/l4t-multimedia/group__V4L2Dec.htm

3. 使用opencv和gstreamer结合的方式。

opencv3.2之后的版本在jetson上运行速度更快，是因为这些版本整合了英伟达对opencv的优化。



## 硬件编解码



### 查询视频设备信息

一般我们用的USB摄像头可以通过v4l查看设备信息，

- v4l安装命令

```shell
sudo apt-get install v4l-utils
#查询连接的设备
v4l2-ctl --list-devices
#查询设备支持的格式
v4l2-ctl -d /dev/video0 --list-formats-ext

#查询所有已连接的v4l设备：
v4l2-ctl --list-formats-ext
```





### 实例测试

#### 仿真测试

我们使用gstreamer自带的videotestsrc模块进行测试。

```shell
gst-launch-1.0 videotestsrc ! 'video/x-raw, format=(string)UYVY, width=(int)1280, height=(int)720' ! nvvidconv ! 'video/x-raw(memory:NVMM), format=(string)NV12' ! omxh264enc ! h264parse ! qtmux ! filesink location=./test.mp4 -e
```

我们可以使用**jtop**工具来查看硬件解码情况，当进行硬件解码时，在[HW Engines]->NVDEC下可以看到正在工作：

<img src="https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210913164522206.png" alt="image-20210913164522206" style="zoom:80%;" />

#### USB摄像头

我们首先插入一个USB网络摄像头，通过如下命令查看设备名称：

```shell
$ ls /dev/video*
/dev/video0
```

使用gstreamer打开摄像头

```shell
gst-launch-1.0 v4l2src device=/dev/video0 ! xvimagesink
```



## GStreamer

### 官方教程

https://gstreamer.freedesktop.org/documentation/tutorials/basic/hello-world.html?gi-language=c#

### 学习教程

[Python GStreamer Tutorial (brettviren.github.io)](https://brettviren.github.io/pygst-tutorial-org/pygst-tutorial.html)





[How to use Gstreamer AppSrc in Python - LifeStyleTransfer](http://lifestyletransfer.com/how-to-use-gstreamer-appsrc-in-python/)

[GitHub - jackersson/gst-python-tutorials](https://github.com/jackersson/gst-python-tutorials)

[python - Write opencv frames into gstreamer rtsp server pipeline - Stack Overflow](https://stackoverflow.com/questions/47396372/write-opencv-frames-into-gstreamer-rtsp-server-pipeline)

#### github

[GitHub - sahilparekh/GStreamer-Python: Fetch RTSP Stream using GStreamer in Python and get image in Numpy](https://github.com/sahilparekh/GStreamer-Python)



### 解码

jetson中，用gstreamer进行硬件编码使用的是**omxh264dec**或**omxh265dec**。

- 打开USB网络摄像头

```shell
gst-launch-1.0 v4l2src device=/dev/video0 ! xvimagesink
```

- 打开rtsp摄像头（h264)

```pyhton
gst-launch-1.0 rtspsrc location=rtsp://192.168.0.119/554 latency=200 ! rtph264depay ! h264parse ! omxh264dec ! nveglglessink
```

- 打开rtsp摄像头（h265)

```shell
gst-launch-1.0 rtspsrc location=rtsp://192.168.0.152/live latency=200 ! rtph265depay ! h265parse ! omxh265dec ! nveglglessink
```

> 通过设置latency的值调整视频的延时。

- rtmp推流

```shell
gst-launch-1.0 -v v4l2src device=/dev/video0 ! 'video/x-h264, width=1280, height=720, framerate=30/1' ! queue !  h264parse ! flvmux ! rtmpsink location='rtmp://192.168.1.102/live'
```

### 编码

jetson中，用gstreamer进行硬件编码使用的是**omxh264enc**或**omxh265enc**。

使用**videotestsrc**产生视频并编码保存为本地文件。

```shell
gst-launch-1.0 videotestsrc ! 'video/x-raw, format=(string)UYVY, width=(int)1280, height=(int)720' ! nvvidconv ! 'video/x-raw(memory:NVMM), format=(string)NV12' ! omxh264enc ! h264parse ! qtmux ! filesink location=./test.mp4 -e

gst-launch-1.0 videotestsrc ! 'video/x-raw, format=(string)NV12, width=(int)1280, height=(int)720' ! nvvidconv ! 'video/x-raw(memory:NVMM), format=(string)NV12' ! omxh264enc ! h264parse ! qtmux ! filesink location=./test.mp4 -e
```

### python编程

#### 示例：H265 rtsp视频流解码与显示

```python
import cv2
pipeline = 'rtspsrc location=rtsp://192.168.0.152/live latency=250 ! '\
               'rtph265depay ! h265parse ! omxh265dec ! nvvidconv ! '\
               'video/x-raw, width=(int)1280, height=(int)720, '\
               'format=(string)BGRx ! '\
               'videoconvert ! appsink'

capture = cv2.VideoCapture(pipeline, cv2.CAP_GSTREAMER)
while capture.isOpened():
    res, frame = capture.read()
    cv2.imshow("video", frame)
    #print("Frame size:{}".format(frame.shape))
    key = cv2.waitKey(1) & 0xFF
    if key == ord('q'):
        break
capture.release()
cv2.destroyAllWindows()
```



### 命令行学习

##### videotestsrc

```shell
$ gst-launch-1.0 videotestsrc ! ximagesink

$ gst-inspact-1.0 videotestsrc
$ gst-launch-1.0 videotestsrc pattern=11 ! ximagesink

$ gst-launch-1.0 videotestsrc pattern=0 ! video/x-raw, format=BGR ! autovideoconvert ! ximagesink

$ gst-launch-1.0 videotestsrc pattern=0 ! video/x-raw, format=BGR ! autovideoconvert ! videoconvert ! video/x-raw,width=1280,height=720, framerate=30/1 ! ximagesink
$ gst-launch-1.0 videotestsrc pattern=0 ! video/x-raw, format=BGR ! autovideoconvert ! videoconvert ! video/x-raw,width=1280,height=720, framerate=1/2 ! ximagesink

# RPi camera
$ gst-launch-1.0 nvarguscamerasrc ! autovideoconvert ! ximagesink
#指定用哪个板载摄像头
$ gst-launch-1.0 nvarguscamerasrc sensor-id=0 ! autovideoconvert ! ximagesink

$ gst-launch-1.0 nvarguscamerasrc ! nvvidconv flip-method=2 ! video/x-raw,width=800,height=600 ! autovideoconvert ! ximagesink

$ gst-launch-1.0 nvarguscamerasrc ! 'video/x-raw(memory:NVMM),width=3264,height=2464,framerate=21/1' ! nvvidconv flip-method=2 ! video/x-raw,width=800,height=600 ! agingtv scratch-lines=0 ! coloreffects preset=spia ! ximagesink
```

### 支持的摄像头

- CSI CAMERAS
  Jetson TX1 and Jetson TX2 currently support 2 CSI RAW BAYER sensors.
  - The platform has been validated with a single OV5693 sensor and a single IMX185
    for capture on L4T.
  - The camera module is interfaced with the Tegra platform via MIPI-CSI.
  - Tested using the nvgstcapture application.
- USB 2.0 CAMERAS
  The following cameras have been validated on Tegra platforms for Android and L4T
  with USB 2.0 ports. These cameras are UVC compliant.



## 流媒体转发

### EasyDarwin

