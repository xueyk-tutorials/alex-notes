# Linux

## 基本工具

- python

  安装

  ```shell
  $ sudo apt-get update
  $ sudo apt-get install python3
  $ sudo apt-get install pip
  ```

- net-tools

  安装：`sudo apt-get install net-tools`

## 进程管理

### 杀死占用端口

```shell
$ kill 9508
```



## 网络常用端口号

> 21/tcp FTP 文件传输协议
>
> 22/tcp SSH 安全登录、文件传送（SCP）和端口重定向
>
> 23/tcp Telnet 不安全的文本传送
>
> 25/tcp SMTP Simple Mail Transfer Protocol (E-mail)
>
> 69/udp TFTP Trivial File Transfer Protocol
>
> 79/tcp finger Finger
>
> 80/tcp HTTP 超文本传送协议 （WWW）
>
> 88/tcp Kerberos Authenticating agent
>
> 110/tcp POP3 Post Office Protocol (E-mail)
>
> 113/tcp ident old identification server system
>
> 119/tcp NNTP used for usenet newsgroups
>
> 220/tcp IMAP3
>
> 443/tcp HTTPS used for securely transferring web pages



## 账号与权限

### 账号

- 切换账号：

  ```shell
  $ su - alex     #切换至alex账号
  ```


### 设置用户权限

#### sudo不需要密码

有时候设置开机启动脚本时，需要sudo运行某个脚本，为了避免输入密码，可以给用户添加权限。

```shell
$ sudo  vim  /etc/sudoers
### 添加权限设置
# 使用户操作都不需要密码
你的账户名  ALL=(ALL:ALL) NOPASSWD: ALL
# 特定操作不需要密码
你的账户名  ALL=(ALL:ALL) NOPASSWD: /bin/sh /usr/local/sbin/start_docker.sh
```
### 权限

改变boolpi文件夹内所有文件的权限为：所有人都可以rwx

```
sudo chmod -R a=rwx boolpi
```



## 时间日期

查看时间日期命令：`date`

设置时间：

```shell
$ sudo date -s "2022-7-14 09:18:00"
```

### 时间同步

参考：

https://blog.csdn.net/Geeker_boy/article/details/110244749?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-110244749-blog-114752452.pc_relevant_multi_platform_whitelistv1_exp2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-110244749-blog-114752452.pc_relevant_multi_platform_whitelistv1_exp2&utm_relevant_index=2



https://blog.csdn.net/qq_41943585/article/details/89679198?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-89679198-blog-114752452.pc_relevant_multi_platform_whitelistv1_exp2&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-89679198-blog-114752452.pc_relevant_multi_platform_whitelistv1_exp2&utm_relevant_index=1



实现计算机A和B的时间同步：

```shell
# A
sudo apt-get install ntp
sudo apt-get install ntpdate
ifconfig                        //查看ip地址
# B
sudo ntpdate -q 电脑A的ip地址    //检查和电脑A的时间差
sudo ntpdate -d 电脑A的ip地址    //和电脑A进行时间同步
```



## scp免交互

我们需要在gitlab-runner使用scp命令自动将相关代码拷贝至目标服务器，故需要配置下免交互。

```shell
# //gitlab-runner
# scp-copy-id root@<目标服务器IP>
```



## ssh

### 开启ssh服务

```shell
$ sudo apt-get install openssh-server
```

查看是否启动，确保sshd进程存在就说明启动了。

```shell
$ sudo ps -e |grep ssh
   1435 ?        00:00:00 ssh-agent
 129810 ?        00:00:00 ssh-agent
 132645 ?        00:00:00 sshd
```

重启ssh服务

```shell
sudo service ssh restart
```



如果客户端无法连接，则编辑ssh主机配置文件`sudo vi /etc/ssh/sshd_config`，查看如下配置项是否开启：

```shell
PasswordAuthentication yes
PermitRootLogin yes
```



## 网络调试

### tcpdump

```shell
 sudo tcpdump udp port 123
```



### iperf

#### 示例

**UDP-服务端**

```bash
$ iperf -u -s
```

每秒打印一次

```bash
$ iperf -u -s -i 1
```



**UDP-客户端**

- 测试带宽、延时

```bash
$ iperf -c 192.168.1.180 -u -b 20M
```

- 记录到文件

```bash
$ iperf -c 192.168.1.180 -u -b 20M > test.txt
```



## 查看流量

### 使用ip

```bash
ip -s link show eth0
```



### 使用iftop

#### 启动命令

```bash
sudo iftop -i eth0
```

#### 界面

![image-20240805142911272](imgs\image-20240805142911272.png)



第一行：带宽显示

中间部分：外部连接列表，即记录了哪些ip正在和本机的网络连接
中间部分右边：实时参数分别是该访问ip连接到本机2秒，10秒和40秒的平均流量
=>代表发送数据，<= 代表接收数据
底部三行：表示发送，接收和全部的流量
底部三行第二列：为你运行iftop到目前流量
底部三行第三列：为高峰值
底部三行第四列：为平均值

TX：发送流量
RX：接收流量
TOTAL：总流量
Cumm：运行iftop到目前时间的总流量
peak：流量峰值
rates：分别表示过去 2s 10s 40s 的平均流量

#### 交互

iftop启动后会提供交互方式，用户可以输入如下命令：

- t：切换显示方式，只显示收、只显示发、收发都显示；
- d：切换是否显示远端目标主机信息；
- p：切换是否显示端口信息；
- S：切换是否显示本机端口；
- D：切换是否显示远端目标主机端口信息；
- 按<根据左边的本机名或IP排序；
- 按>根据远端目标主机的主机名或IP排序；
- 按o切换是否固定只显示当前的连接；



## 文件操作

### 挂载U盘

**1、查找U盘**

在Linux操作系统下，如果想要挂载U盘，首先需要找到U盘所在的设备。可以通过以下命令来查看：

```
sudo fdisk -l
```

该命令会列出电脑中所有的储存设备，通过观察设备的大小来确定哪一个是U盘。

**2、创建挂载点**

为了能够挂载U盘，需要在电脑上创建一个挂载点。可以使用以下命令：

```
sudo mkdir /media/usb
```

在这个例子中，我们将挂载点设置为“/media/usb”，但是具体位置可以根据需求进行更改。

**3、挂载U盘**

在确定了U盘所在的设备以及创建挂载点之后，就可以进行挂载操作了。使用以下命令：

```
sudo mount /dev/sdb1 /media/usb
```

该命令中，“/dev/sdb1”是U盘所在的设备的路径，在不同的电脑上可能会有所不同。如果在挂载的同时想要指定文件系统的格式，可以使用“-t”参数。例如：

```
sudo mount -t ntfs /dev/sdb1 /mnt/usb
```

**4、卸载U盘**

在使用完U盘后，需要对其进行卸载操作，使用以下命令：

```
sudo unmount /media/usb
```

在卸载之前请确保没有程序在使用U盘。

### tar

```shell
# 创建压缩包
$ tar -cvf 生成的打包文件名.tar 待打包的文件
# 解压
$ tar -xvf 要解压的压缩包.tar
```

