# 安装cuda

## 显卡计算性能

https://developer.nvidia.com/cuda-gpus 

## windows开发环境安装

### CUDA部分的安装

在安装cuda之前，一定要退出杀毒软件和安全卫士！

#### nvidia user and code

xueyuankui.good@163.com

UESTC.dream2013

#### cuda

- 下载cuda安装程序

下载cuda前首先确认系统和版本，Windows系统的win10版本和win7版本对应的cuda不能通用！

下载网站为<https://developer.nvidia.com/cuda-downloads>，根据pytorch支持情况选择cuda版本，目前pytorch最高支持10.1，那么就下载cuda10.1版本（可以在网页中Legacy Releases中找到历史版本），下载后文件例如为：**cuda_10.1.105_418.96_win10.exe**。

以管理员权限运行该安装程序，其安装过程中的默认解压路径：C:\Users\ADMINI~1\AppData\Local\Temp\CUDA，解压完毕后进入安装阶段，默认安装路径：C:\Program Files\NVIDIA GPU Computing Toolkit。

- 安装测试

测试下CUDA是否安装完成。打开cmd命令行窗口输入nvcc -V。

- 环境变量

一般情况下（win7系统），安装后自动如下添加环境变量：

`CUDA_PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1`

`CUDA_PATH_V10_1=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1`

然后再添加其余环境变量：

 CUDA_LIB_PATH = %CUDA_PATH%\lib\x64
 CUDA_BIN_PATH = %CUDA_PATH%\bin

CUDA_SDK_PATH = C:\ProgramData\NVIDIA Corporation\CUDA Samples\v10.1

 CUDA_SDK_BIN_PATH = %CUDA_SDK_PATH%\bin\win64
 CUDA_SDK_LIB_PATH = %CUDA_SDK_PATH%\common\lib\x64

最后在cmd窗口输入set cuda查看环境变量的设置是否正确。

注意：环境变量配置完成后，cmd窗口需要重启才能确保环境变量配置生效。

#### cudnn

- 下载

下载cudnn前首先确认系统和版本，Windows系统的win10版本和win7版本对应的cudnn不能通用，而且要安装与cuda对应的版本！

<https://developer.nvidia.com/rdp/cudnn-download>

- 解压添加

将CUDA\bin、CUDA\include、CUDA\lib中的内容拷贝到相应的C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1文件路径下即可。

将bin所在的目录添加到环境变量 PATH 中即可。

### 多版本cuda

一台计算机可以安装多个版本的cuda而不会相互影响，只需要根据使用的cuda版本修改**环境变量**即可。



## Ubuntu开发环境

### 安装显卡驱动

1. 禁用Nouveau

   打开一个Linux terminal 中输入以下指令,没有内容输出,说明没有Nouveau驱动,可跳过这部分直接进入到NVIDIA驱动安装，反之,如果有打印nouveau信息,则需要先进行禁用nouveau;

   ```shell
   $ lsmod | grep nouveau
   ```

   打开一个Linux terminal 中输入以下指令

   ```shell
   $ sudo gedit /etc/modprobe.d/blacklist.conf
   ```

   在文件最后加入以下内容
   ```shell
   blacklist nouveau
   options nouveau modeset=0
   ```

   更新使其生效

   ```shell
   $ sudo update-initramfs -u
   ```

   重启计算机

   ```shell
   $ reboot
   ```

   

2. 查看NVIDA驱动信息

   运行命令`ubuntu-drivers devices`

   ```shell
   alex@alex-Mi-Gaming-Laptop-15-6:~$ ubuntu-drivers devices
   == /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
   modalias : pci:v000010DEd00002191sv00001D72sd00001807bc03sc00i00
   vendor   : NVIDIA Corporation
   driver   : nvidia-driver-460 - distro non-free
   driver   : nvidia-driver-470 - distro non-free recommended
   driver   : nvidia-driver-418-server - distro non-free
   driver   : nvidia-driver-495 - distro non-free
   driver   : nvidia-driver-460-server - distro non-free
   driver   : nvidia-driver-470-server - distro non-free
   driver   : nvidia-driver-450-server - distro non-free
   driver   : xserver-xorg-video-nouveau - distro free builtin
   
   == /sys/devices/pci0000:00/0000:00:14.3 ==
   modalias : pci:v00008086d0000A370sv00008086sd00000034bc02sc80i00
   vendor   : Intel Corporation
   manual_install: True
   driver   : backport-iwlwifi-dkms - distro free
   ```

   ubuntu自己检测nvidia显卡的可选驱动，并给出推荐（用recommend标出）

3. 安装NVIDIA驱动（添加源安装方式）

   打开`系统设置 -> 软件和更新 -> 附加驱动（Additional Drivers）`，勾选一个合适的驱动版本。然后等待安装完成。

   > 注意：
   >
   > 如果列表中没有相关的选项,请添加源后重试:
   > 打开一个Linux terminal 中输入以下指令
   >
   > ```shell
   > $ sudo add-apt-repository ppa:graphics-drivers/ppa
   > $ sudo apt-get update
   > ```

​		安装完毕一定要再次重启计算机！

4. 查看显卡驱动和支持的cuda版本信息

```shell
alex@alex-Mi-Gaming-Laptop-15-6:~$ nvidia-smi
Sat Nov 20 21:02:01 2021       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 470.82.00    Driver Version: 470.82.00    CUDA Version: 11.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   38C    P8     1W /  N/A |      6MiB /  5944MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      1603      G   /usr/lib/xorg/Xorg                  4MiB |
+-----------------------------------------------------------------------------+

```

### 安装cuda

需要去官网下载cuda和cuDNN安装包

cuda: https://developer.nvidia.com/cuda-toolkit-archive

cudnn: <https://developer.nvidia.com/rdp/cudnn-download>
> 注意，一定要下载与系统对应的安装包，Ubuntu18.04与Ubuntu20.04的版本是不同的！

1. 运行安装命令

```shell
$ mkdir ~/tmp  # 创建临时文件夹，避免安装时提示空间不够，无法安装
$ sudo sh cuda_10.0.130_410.48_linux.run --tmpdir=~/tmp
```

- 按q，然后输入accept

- 提示是否安装驱动时，选择no

- 后续根据提示输入即可！

  安装路径为`/usr/local/cuda-10.0`，并且生成一个软链接`/usr/local/cuda`。


```shell
$ sudo sh cuda_11.0.2_450.51.05_linux.run
===========
= Summary =
===========

Driver:   Not Selected
Toolkit:  Installed in /usr/local/cuda-11.0/
Samples:  Installed in /home/alex/

Please make sure that
 -   PATH includes /usr/local/cuda-11.0/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-11.0/lib64, or, add /usr/local/cuda-11.0/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-11.0/bin

Please see CUDA_Installation_Guide_Linux.pdf in /usr/local/cuda-11.0/doc/pdf for detailed information on setting up CUDA.
***WARNING: Incomplete installation! This installation did not install the CUDA Driver. A driver of version at least .00 is required for CUDA 11.0 functionality to work.
To install the driver using this installer, run the following command, replacing <CudaInstaller> with the name of this run file:
    sudo <CudaInstaller>.run --silent --driver

Logfile is /var/log/cuda-installer.log

```
2. 添加环境变量

```shell
$ gedit ~/.bashrc
# 末尾添加如下
export PATH="/usr/local/cuda-10.0/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH"
```

3. 测试是否安装成功

```shell
$ souce ~/.bashrc
$ nvcc -V

$ cd ~/NVIDIA_CUDA-_Samples/1_Utilities/bandwidthTest/
$ make
$ ./bandwidthTest
```

4. 安装位置

```shell
alex@alex-Mi-Gaming-Laptop-15-6:~$ whereis cuda
cuda: /usr/lib/cuda /usr/include/cuda.h
```

### 安装cudnn

1. 下载

   选择合适版本后，依次下载：

   - cuDNN Library for Linux
   - cuDNN Runtime Library for Ubuntu18.04 (Deb)
   - cuDNN Developer Library for Ubuntu18.04 (Deb)
   - cuDNN Code Samples and User Guide for Ubuntu18.04 (Deb)

2. 安装

   - 安装压缩包（选项一）

   切换到下载文件所在目录，解压下载好的cuDNN压缩文件到当前目录：

   ```shell
   $ tar zxvf ./cudnn-10.1-linux-x64-v7.6.5.32.tgz -C ./
   ```

   将解压出的cuda/include/cudnn.h文件复制到/usr/local/cuda/include文件夹，cuda/lib64/下所有文件复制到/usr/local/cuda/lib64文件夹:

   ```shell
   $ sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
   $ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
   ```

   为上述文件添加读取和执行权限：

   ```shell
   $ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
   ```

   - 安装三个deb文件（选项二）

   ```shell
   $ sudo dpkg -i libcudnn7_7.6.5.32-1+cuda10.0_amd64.deb
   ```

3. 查看安装版本

   ```shell
   $ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
   ```

### cuda的动态连接库路径配置

1. 配置环境变量（第一种）

   打开`~/.bashrc`，添加如下：

   ```shell
   ### cuda
   export PATH="/usr/local/cuda-10.0/bin:$PATH"
   export LD_LIBRARY_PATH="/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH"
   export LIBRARY_PATH="$LIBRARY_PATH:/usr/local/cuda-10.0/lib64"
   ```

为了避免程序（如pytorch等）无法找到cuda链接库，需要进行配置一下。根据cua版本，通过如下命令配置：

```shell
sudo echo "/usr/local/cuda-10.0/lib64" >> /etc/ld.so.conf.d/cuda.conf
sudo ldconfig
```

等效于：

```shell
$ sudo echo "/usr/local/cuda-10.0/lib64/" >> /etc/ld.so.conf  # 也可以直接编辑 /etc/ld.so.conf文件，在最后添加"/usr/local/cuda-10.0/lib64/"
$ sudo ldconfig
```

### 多版本cuda

一台计算机可以安装多个版本的cuda而不会相互影响，只需要根据使用的cuda版本修改**环境变量**（~/.bashrc文件）即可。



### 卸载

1. 卸载显卡驱动
    如果显卡驱动安装错误，可以卸载后再通过software update更新安装。
    ```shell
    $ sudo apt-get --purge remove nvidia*
    $ sudo apt autoremove
    ```
2. 卸载cuda

   ```shell
   $ sudo /usr/local/cuda-10.0/bin/uninstall_cuda_10.0.pl
   $ sudo rm -rf /usr/local/cuda-10.0/
   ```
   
3. 卸载cudnn的deb安装包

   查看安装情况

   ```shell
   $ sudo dpkg -l | grep cudnn
   ```

   对于每个安装包依次运行卸载命令，例如：

   ```shell
   $ sudo dpkg -r libcudnn7
   ```

