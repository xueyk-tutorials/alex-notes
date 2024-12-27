# Ubuntu

## 简介

Ubuntu是Mark Shuttleworth在2004年基于Debian 创建的发行版。

## 常用命令

### 查看版本信息

命令`lsb_release -a`

```
Distributor ID: Ubuntu           //类别是ubuntu
Description:  Ubuntu 16.04.3 LTS //16年3月发布的稳定版本，LTS是Long Term Support：长时间支持版本
Release:    16.04               //发行日期或者是发行版本号
Codename:   xenial              //ubuntu的代号名称
```

命令`uname -a`

显示linux的内核版本和系统是多少位的：X86_64代表系统是64位的。

### 查看CPU信息

```bash
dpkg --print-architecture
```



```
cat /proc/cpuinfo
```

## 镜像源

通过编辑`/etc/apt/sources.list`配置文件，添加镜像源。特别注意的是，一定将sources.list的所有内容删除后再写入新的镜像源。

```
sudo vim /etc/apt/sources.list
#添加阿里云等镜像源
sudo apt update
sudo apt upgrade
```

### 镜像源18.04

#### 阿里云镜像源

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

#### 清华镜像源

```
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
```

#### 中科大

```
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
 deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
 deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
 deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
 deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
 deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
 deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
 deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
 deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
 deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
```

### 镜像源20.04
#### 中科大
```shell
deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted
deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted
deb https://mirrors.ustc.edu.cn/ubuntu/ focal universe
deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates universe
deb https://mirrors.ustc.edu.cn/ubuntu/ focal multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu focal-security main restricted
deb http://security.ubuntu.com/ubuntu focal-security universe
deb http://security.ubuntu.com/ubuntu focal-security multiverse
```
#### 清华源

https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/
```shell
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
```

#### 阿里云镜像源
```shell
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

### 镜像源22.04

#### 阿里云

```bash
deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
```

#### 清华

```bash
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```



#### 中科大
```bash
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```



## 软件安装

### 下载离线安装包

各版本的Linux系统的所有软件包都可以在网站[Packages for Linux and Unix - pkgs.org](https://pkgs.org/)查询或下载。例如Ubuntu20.04-amd64版本相关软件包可以在[Ubuntu 20.04 LTS (Focal Fossa) - Ubuntu Main amd64 (pkgs.org)](https://ubuntu.pkgs.org/20.04/ubuntu-main-amd64/)链接中查看。在查询框输入软件包名称就能够列出所有软件包。

使用`apt download`命令可将软件安装包下载到本地。

- 可以指定软件包具体名称

  在软件包网站查询到某个包的具体名称，这样的好处是可以下载与目标配置环境一致的软件包。

```bash
$ apt download chrony_3.5-6ubuntu6.2_amd64.deb
```

- 也可以不指定，由系统自行判断并选择下载

  这样会根据当前计算机环境下载相符合的软件包。

```bash
$ apt download chrony
```



### 基本软件

```bash
$ apt-get install iputils-ping
$ sudo apt install net-tools
$ sudo apt-get install locales
```

python相关

```bash
$ sudo apt install python3
$ sudo apt install python3-pip
```





## Nvidia驱动安装

参考：https://blog.csdn.net/m0_54792870/article/details/112980817





## 开机自启动

### linux service

#### 示例1：基本

1. 添加服务脚本

   首先创建一个服务脚本

   1. 

   ```shell
   $ sudo vi /etc/systemd/system/rc-local.service
   ```

   内容为：

   ```shell
   [Unit]
   Description=/etc/rc.local Compatibility 
   ConditionPathExists=/etc/rc.local 
   
   [Service]
   Type=forking 
   ExecStart=/etc/rc.local start 
   TimeoutSec=0 
   StandardOutput=tty 
   RemainAfterExit=yes 
   SysVStartPriority=99 
   
   [Install]
   WantedBy=multi-user.target
   ```

2. 创建启动脚本

   创建服务脚本中对应的启动脚本`/etc/rc.local`。

   ```shell
   $ sudo vi /etc/rc.local
   ```

   内容如下：

   ```shell
   #!/bin/sh -e 
   ## rc.local
   bash /home/alex_run.sh &
   ```

   这里alex_run.sh就是对应的业务脚本，也就是开机后要做的工作。

   > 注意，这里在bash脚本后增加了&，使其后台运行，避免阻塞。
   >
   > 一定要增加权限：
   >
   > $ sudo chmod 777 /etc/rc.local

3. 编写业务脚本

   在`alex_run.sh`中正式开始编写自启动要做的具体工作，例如开机后仅仅创建一个文件夹：

   ```shell
   #! /bin/sh
   mkdir /home/alex/Desktop/autorun_test
   sudo mkdir /home/autorun_test.txt
   sudo echo "auto run" > /home/autorun_test.txt
   ```

4. 激活服务

   ```shell
   $ systemctl enable rc-local.service
   ```

5. 启动服务

   ```shell
   $ systemctl start rc-local.service
   ```

6. 重启系统

   ```shell
   $ sudo reboot
   ```

7. 检测服务运行

   ```shell
   $ systemctl status rc-local
   ```

   

## 工具

### 屏幕录制

#### kazam

安装简单，可以全屏也可以选择区域录制。

1. 安装

```shell
$ sudo apt-get install kazam
```

2. 使用

```shell
$ kazam
```



## 文件共享

### 挂载网络文件夹

在Ubuntu中，您可以使用命令行来挂载网络上的共享文件夹。以下是如何操作的步骤：

1. 安装必要的包：

```bash
sudo apt-get install cifs-utils
```

2. 创建一个本地挂载点：

```bash
mkdir ~/remote_share
```

3. 挂载共享文件夹：

```bash
sudo mount -t cifs //服务器地址/共享名称 ~/remote_share -o username=用户名,password=密码
```

将`服务器地址`、`共享名称`、`用户名`和`密码`替换为实际的值。

例如，如果服务器地址是`192.168.1.100`，共享名称是`myshare`，用户名是`user1`，密码是`pass123`，则命令将是：

```bash
sudo mount -t cifs //192.168.1.100/myshare ~/remote_share -o username=user1,password=pass123
```

注意：出于安全考虑，不推荐在命令行中直接包含密码。您可以使用`cred`选项来指定凭据文件，或者使用`keytab`文件。

要卸载共享文件夹，请使用以下命令：

```bash
sudo umount ~/remote_share
```

配置重启有效

使挂载点持久化。默认情况下，挂载点只在系统重启前有效。要使挂载点持久，请向“/etc/fstab”文件添加条目。**使用以下命令打开文件：**

```bash
$ sudo nano /etc/fstab
```

然后在文件末尾添加：

```bash
//192.168.1.100/myshare ~/remote_share cifs username=user1,password=pass123 0 0
```





### samba

使用samba，Ubuntu与win10共享文件。

```shell
$ sudo apt-get install samba
$ sudo apt-get install smbclient
#安装完成后执行 
$ samba -V
```



#### 卸载

```shell
# samba服务器卸载
apt-get remove samba
apt-get remove sambaclient
apt-get remove samba-common
```



#### 示例一:基于当前计算机用户创建共享文件夹

1. samba配置

```shell
# 创建文件夹,用于共享,并更改权限
$ mkdir /home/allex/alex_share
$ sudo chmod 777 /home/allex/alex_share
# 将现有的用户(计算机当前已经创建的用户allex)添加到smb,设置一个密码并且重复输入两次
$ sudo smbpasswd -a allex

# 修改配置文件
$ sudo vi /etc/samba/smb.conf
###########################################
# 在文件最后添加如下:
[allex_share]
comment = alex share folder
browseable = yes
path = /home/allex/alex_share
create mask = 0700
directory mask = 0700
valid users = allex
force user = allex
force group = allex
public = yes
available = yes
writable = yes
###########################################
# 重启samba服务
$ sudo service smbd restart
```

2. windows访问

按`win+R`,输入linux主机地址即可,根据提示输入用户和密码.然后就可以看到`allex_share`文件夹了!

为了后续方面,可以在共享文件夹右键,选择映射网络驱动器!

## Snap

Snap是Ubuntu母公司Canonical于2016年4月发布Ubuntu16.04时候引入的一种安全的、易于管理的、沙盒化的软件包格式，与传统的dpkg/apt有着很大的区别。Snap可以让开发者将他们的软件更新包随时发布给用户，而不必等待发行版的更新周期；其次Snap应用可以同时安装多个版本的软件

snap安装软件后，可以在`/snap`中找到各类软件的安装目录。

由于snap软件包太多，光靠命令搜索很麻烦，可以去网站`https://uappexplorer.com/snaps`查看snap已经支持的软件包。

### 基础命令

| 命令                             |                                                         |
| -------------------------------- | ------------------------------------------------------- |
| snap whami                       | 查看你是否通过Ubuntu One登陆Snap                        |
| snap login                       | 通过Ubuntu One登陆Snap                                  |
| snap find python                 | 在snapstore中寻找软件(如python)                         |
| snap info python                 | 查看软件更多信息                                        |
| sudo snap install [yoursoftware] | 安装软件                                                |
| sudo snap list                   | 查询已经安装的软件                                      |
| sudo snap remove [yoursoftware]  | 卸载软件                                                |
| sudo snap switch --channel=xxxxx | 更换安装通道；snap默认通道stable；candidate是发行版通道 |
| sudo snap refresh [yoursoftware] | 更新软件                                                |



### 示例一：应用程序snap打包

本示例详解Linux中将应用程序打包为Snap软件包格式的方法，创建一个 snap 包并不困难。首先，你需要一个 snap 基础运行环境，能够让你的桌面环境认识并运行 snap 软件包，这个工具叫做 snapd ，默认内置于所有 Ubuntu 16.04 系统中。接着你需要创建 snap 的工具 Snapcraft，它可以通过一个简单的命令安装：

```
$ sudo apt-get install snapcraft
```

Snap 使用一个特定的 YAML 格式的文件 `snapcraft.yaml`，它定义了应用是如何打包的以及它需要的依赖。用一个简单的应用来演示一下，下面的 YAML 文件是个如何 snap 一个 moon-buggy 游戏的实际例子，该游戏在 Ubuntu 源中提供。

```
name: moon-buggy
version: 1.0.51.11
summary: Drive a car across the moon
description: |
A simple command-line game where you drive a buggy on the moon
apps:
 play:
 command: usr/games/moon-buggy
parts:
 moon-buggy:
 plugin: nil
 stage-packages: [moon-buggy]
 snap:
– usr/games/moon-buggy
```

第一部分是关于如何让你的应用可以在商店找到的信息，设置软件包的元数据名称、版本号、摘要、以及描述。apps 部分实现了 play 命令，指向了 moon-buggy 可执行文件位置。parts 部分告诉 snapcraft 用来构建应用所需要的插件以及依赖的包。在这个简单的例子中我们需要的所有东西就是来自 Ubuntu 源中的 moon-buggy 应用本身，snapcraft 负责剩下的工作。

在你的 snapcraft.yaml 所在目录下运行 snapcraft ，它会创建 moon-buggy1.0.51.11amd64.snap 包，可以通过以下命令来安装它：

```
$ snap install moon-buggy_1.0.51.11_amd64.snap
```





## 常见问题和解决

Could not open lock file /var/lib/dpkg/lock-frontend 

```shell
sudo rm -rf /var/lib/dpkg/lock
sudo rm -rf /var/cache/apt/archives/lock
sudo apt-get update
sudo dpkg --configure -a
```



dpkg: error: dpkg frontend lock is locked by another process

```shell
$ sudo rm /var/lib/dpkg/lock-frontend
```

