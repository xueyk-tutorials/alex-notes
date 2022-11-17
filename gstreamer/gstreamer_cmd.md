## 从设备视频获取

### rtsp吊舱

- 解析H264格式视频流

  ```shell
  gst-launch-1.0 rtspsrc location=rtsp://192.168.0.119/554 latency=2000 ! rtph264depay ! h264parse ! omxh264dec ! nveglglessink
  ```

- 解析H265格式视频流
  
  ```shell
  ##
  gst-launch-1.0 rtspsrc location=rtsp://admin:123456@192.168.10.57/stream0 latency=2000 ! rtph264depay ! h264parse ! omxh264dec ! nveglglessink
  ##
  gst-launch-1.0 rtspsrc location=rtsp://192.168.0.152/live latency=200 ! rtph265depay ! h265parse ! omxh265dec ! nveglglessink
  
  ##
  ```

### USB网络摄像头

- 打开并显示

```shell
gst-launch-1.0 v4l2src device="/dev/video0" ! "video/x-raw, width=640, height=480, format=(string)I420" ! xvimagesink -e
```
### 板载RPi摄像头

```shell
gst-launch-1.0 nvarguscamerasrc ! 'video/x-raw(memory:NVMM),  width=1920, height=1080, format=(string)NV12,  framerate=(fraction)30/1' ! nvoverlaysink -e
```

## 从文件获取

参考Jetson_TX1_and_TX2_Accelerated_GStreamer_User_Guide.pdf

- 本地mp4文件

```shell
gst-launch-1.0 filesrc location=/home/stoner/Desktop/Video3.mp4 !  qtdemux name=demux demux.video_0 ! queue ! h264parse ! omxh264dec !  nveglglessink -e
```



