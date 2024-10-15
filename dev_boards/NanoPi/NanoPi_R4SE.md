# 简介

## 结构

产品底部有四个螺丝孔（黑色垫片封住了），可以进行拆卸。

### 尺寸



## 电气

### 电路板布局

![NanoPi_R4SE_Layout](imgs/NanoPi_R4SE_Layout.jpg)

### 调试串口

开发板提供了一路调试串口（UART2），3V电平, 波特率为1500000bps。

| 引脚 | 定义        | 描述   |
| ---- | ----------- | ------ |
| 1    | GND         | 0V     |
| 2    | UART2DBG_TX | output |
| 3    | UART2DBG_RX | intput |



# 如何使用

## 首次开机

开发板默认安装的是FriendlyWrt 23.05.2系统。

### 参考

http://wiki.friendlyelec.com/wiki/index.php/NanoPi_R4SE/zh#.E6.96.B9.E6.B3.953:_.E9.80.9A.E8.BF.87USB.E7.83.A7.E5.86.99

### 帐户与密码

默认用户名：root

默认密码是：password(某些版本是空密码）

### 连接

1. 开发板上电，连接交换机/路由器；
2. 准备一个电脑，连接交换机/路由器，设置IP地址为192.168.2.xxx；
3. 在电脑浏览器上输入以下网址即可进入FriendlyWrt管理页面:
   - [http://friendlywrt/](http://friendlywrt/)
   - http://192.168.2.1/

开发板默认IP为192.168.2.1

### 系统版本

默认安装的系统版本是：FriendlyWrt 23.05.2, r23630-842932a63d

### 网页终端

![image-20240930092157998](imgs/image-20240930092157998.png)