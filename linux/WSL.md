# WSL教程

## 安装

### 安装WSL2

请参考官网教程：https://docs.microsoft.com/zh-cn/windows/wsl/install-manual

#### 步骤 1 - 启用适用于 Linux 的 Windows 子系统

管理员方式打开power shell

```shell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

#### 步骤 2 - 检查运行 WSL 2 的要求

#### 步骤 3 - 启用虚拟机功能

```shell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

重启计算机。

#### 步骤 4 - 下载 Linux 内核更新包

下载最新包：

- [适用于 x64 计算机的 WSL2 Linux 内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
- 运行上一步中下载的更新包。 （双击以运行 - 系统将提示你提供提升的权限，选择“是”以批准此安装。）

#### 步骤 5 - 将 WSL 2 设置为默认版本

```shell
wsl --set-default-version 2
```

#### 步骤 6 - 安装所选的 Linux 分发

1. 打开 [Microsoft Store](https://aka.ms/wslstore)，并选择你偏好的 Linux 分发版。

2. 在分发版的页面中，选择“获取”。

3. 首次启动新安装的 Linux 分发版时，将打开一个控制台窗口，系统会要求你等待一分钟或两分钟，以便文件解压缩并存储到电脑上。 未来的所有启动时间应不到一秒。

   然后，需要[为新的 Linux 分发版创建用户帐户和密码](https://learn.microsoft.com/zh-cn/windows/wsl/setup/environment#set-up-your-linux-username-and-password)。

### 安装Windows终端

Windows终端用于管理所有的终端如cmd、powerShell、WSL，交互性较好。

请参考官网教程：https://docs.microsoft.com/zh-CN/windows/terminal/install

### Win11下应用界面 配置
如果是在win11系统下安装wsl2，则可以直接打开Linux下的桌面应用并在Windows下显示。
参考：https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/gui-apps

- 安装
安装完wsl后，打开powershell（管理员身份运行）：
```shell
wsl --update    #通过update更新到包含Linux GUI支持的最新版本

wsl --shutdown  #需要重启wsl，关闭后，通过Windows terminal再次打开Ubuntu即可
```
- 测试
打开Ubuntu终端，输入`gedit`即可弹出文本编辑界面。
## WSL开机启动

```shell
sudo cat > /etc/init.wsl << EOF
#!/bin/sh
/etc/init.d/ssh start
/etc/init.d/nginx start
EOF

sudo chmod +x /etc/init.wsl
```

## WSL网络

### WSL2 IP配置相关

#### WSL2固定IP

每次启动WSL前，我们可以通过windows给WSL设置分配IP

1、管理员权限打开powershell

2、给WSL添加IP地址

这里我们添加的IP地址为`192.168.50.24`

```shell
$ wsl -d Ubuntu-20.04 -u root ip addr add 192.168.50.24/24 broadcast 192.168.50.255 dev eth0 label eth0:1
```

![image-20210901103928928](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210901103928928.png)

现在启动WSL2后，查看IP地址如下：

![image-20210901104000711](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210901104000711.png)

3、给Win10添加 IP地址

```shell
$ netsh interface ip add address "vEthernet (WSL)" 192.168.50.12 255.255.255.0
```

该命令运行后，输入`ipconfig`查看Win10下网络配置如下：

![image-20210901104701726](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210901104701726.png)

由于每次手动设置很麻烦，可以将这两个设置命令写在一个.bat文件中，并且开机自启动！

开机启动设置

##### 方法一：编写启动脚本

- 打开启动文件夹

![image-20210901104259292](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210901104259292.png)

- 在`C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`文件夹中放启动脚本即可。

这里启动脚本为wsl2_ip_config.bat文件，内容如下：

```shell
@echo off
wsl -d Ubuntu-20.04 -u root ip addr add 192.168.50.16/24 broadcast 192.168.50.255 dev eth0 label eth0:1

netsh interface ip add address "vEthernet (WSL)" 192.168.50.88 255.255.255.0
```



![image-20210901104942186](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210901104942186.png)

> 注意：
>
> 1、每次开机，启动脚本运行时都会弹出终端框；如果不希望这样，参考方法二；
>
> 2、Windows的"vEthernet (WSL)"虚拟网卡，只有每次启动**WSL**后才会生效，所以你会发现，如果WSL的IP配置成功，但Win10下的IP没有添加成功，所以每次还需要启动一下WSL然后再运行一次配置命令：
>
> $ netsh interface ip add address "vEthernet (WSL)" 192.168.50.88 255.255.255.0

##### 方法二：通过vbs启动bat脚本

1、将启动脚本wsl2_ip_config.bat放到路径`E:\Develop_drone\2Simulation\`下。

2、编写一个**wsl2_setup_scripts.vbs**文件

```shell
set ws = WScript.CreateObject("WScript.Shell")
ws.Run "E:\Develop_drone\2Simulation\wsl2_ip_config.bat /start",0
```

其他参考：

https://blog.csdn.net/weixin_41301508/article/details/108939520?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&utm_relevant_index=2



### 外部计算机访问WSL2

#### 原理

##### 通过netsh配置

https://zhuanlan.zhihu.com/p/357038111

```html
netsh winsock reset
```

##### netsh使用

- 新增端口转发

```bash
netsh interface portproxy add v4tov4 listenaddress=监听地址 listenport=监听端口 connectaddress=转发到的地址 connectport=转发到的端口
#例如新增外部ssh访问转发至本机的WSL2：
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=22 connectaddress=wsl2ip connectport=22
```

- 查看设置了哪些端口转发

```bash
netsh interface portproxy show all
```

- 删除转发规则

```bash 
netsh interface portproxy delete v4tov4 listenaddress=192.168.88.110 listenport=9988
```

> Note:
>
> **使用netsh interface portproxy记得配置Windows和出口路由器防火墙规则**

#### 创建端口转发

##### 查看IP



##### 示例

新增端口转发：转发主机14550至WSL2的14550端口。

```shell
#1）使用localhost
$ netsh interface portproxy add v4tov4 listenport=14550 listenaddress=0.0.0.0 connectport=14550 connectaddress=localhost

#2）如果不用localhost，则查看WSL VM的ip地址，例如为192.168.208.1
$ netsh interface portproxy add v4tov4 listenport=14550 listenaddress=0.0.0.0 connectport=14550 connectaddress=192.168.215.89
```

> 在18945之后的版本中, 微软改进了WSL2, 使我们可以使用localhost, 访问WSL的网络服务

#### 其他方式

实现开机后自动转发主机端口到WSL2的端口,便于外网访问WSL2.

直接上地址:https://github.com/yhl452493373/WSL2-Auto-Port-Forward

## WSL桌面

### 使用XLaunch+xfce4

#### Windows下安装和配置XLaunch

- 下载并安装[VcXsrv](https://sourceforge.net/projects/vcxsrv/)

安装完成后，在桌面生成XLaunch快捷方式

- 运行XLaunch

双击XLaunch快捷方式启动，选择默认配置即可，在设置页面中输入**-ac**，如下图所示：

![image-20220907154253454](imgs\image-20220907154253454.png)

启动完成后，会弹出一个黑框，这是因为桌面服务还没有启动，需要在Ubuntu中进行xfce4服务启动。

#### Ubuntu下安装和配置xfce4

- 安装

输入如下命令安装，选择默认配置即可。

```shell
$ sudo apt update
$ sudo apt install xfce4
```

- 配置环境变量

  Windows下打开PowerShell，输入`ipconfig`，获取WSL对应的IP，如下图所示：

![image-20220907154751081](imgs\image-20220907154751081.png)

然后在Ubuntu下，在`~/.bashrc`文件末尾添加：

```shell
export DISPLAY=<WSL的IP>:0     #例如我的配置为 export DISPLAY=172.24.48.1:0
```

- 启动xfce4

```SHELL
$ startxfce4
```

![image-20220907155852780](H:\alex-github\alex-notes\linux\imgs\image-20220907155852780.png)

启动后，Windows界面中的XLaunch黑框，就会显示Ubuntu界面。

![image-20220907155936324](H:\alex-github\alex-notes\linux\imgs\image-20220907155936324.png)

### 使用xrdp实现远程桌面

使用xrdp+xfce4进行远程桌面访问

#### 安装包

```shell
$ sudo apt update
$ sudo apt install -y xfce4 xrdp
```

> Note:
>
> 安装xfce4过程中会出现选择显示管理DM选择的提示,建议用`lightdm`
>
> 如果错过了安装过程中出现的这个向导,那么可以在安装完成后执行下面的命令重新设置DM
>
> ```bash
> $ sudo dpkg-reconfigure lightdm
> ```

#### 修改xrdp默认端口

由于`xrdp`安装好后默认配置使用的是和Windows远程桌面相同的`3389` 端口,为了防止和Windows系统远程桌面冲突,建议修改成其他的端口

```bash
$ sudo vim /etc/xrdp/xrdp.ini
# 修改下面这一行,将默认的3389改成其他端口即可
port=3390
```

#### 为当前用户指定登录session类型

**注意这一步很重要,如果不设置的话会导致后面远程桌面连接上闪退**

```bash
$ vim ~/.xsession

# 写入下面内容(就一行)
xfce4-session
```

#### 启动xrdp

由于WSL2里面不能用`systemd`,所以需要手动启动

```bash
$ sudo /etc/init.d/xrdp start
```

#### 远程访问

在Windows系统中运行`mstsc`命令打开远程桌面连接,地址输入`localhost:3390`

注意这里的端口号应当与上面修改配置中一致

![img](https://pic2.zhimg.com/v2-b54f20073cc799a263e989a08ac1e96d_r.jpg)

输入WSL2中使用的账号密码即可。

![img](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/v2-d7675c5320343dafc169040d1e881594_720w.jpg)

> 备注：
>
> 账号为：alex
>
> 密码为：alex
