# conda

## conda基础命令

| 命令            | 功能             | 备注 |
| --------------- | ---------------- | ---- |
| conda --version | 查看anaconda版本 |      |
|                 |                  |      |
|                 |                  |      |



### 环境相关操作

| 命令                                          | 功能                               | 备注 |
| --------------------------------------------- | ---------------------------------- | ---- |
| conda info --e                                | 查看创建的conda环境                |      |
| conda create -n xxx python=3.6                | 创建名字为xxx的python3.6环境       |      |
| conda create -n conda-env2 --clone conda-env1 | 复制环境env1并以此创建一个新的环境 |      |
| activate xxx                                  | 激活环境                           |      |
| deactivate xxx                                | 退出环境                           |      |
| conda remove -n xxx ---all                    | 删除环境                           |      |

### 软件包相关操作

| 命令                  | 功能                | 备注 |
| --------------------- | ------------------- | ---- |
| conda search pkgname  | 查找名为pkgname的包 |      |
| conda install pkgname | 安装包              |      |
| conda list            | 查看已经安装的包    |      |
| conda update pkgname  | 更新包              |      |
| codna remove pkgname  | 卸载包              |      |
|                       |                     |      |

### 镜像

| 命令                           | 功能        | 备注                                                         |
| ------------------------------ | ----------- | ------------------------------------------------------------ |
| cond config --add channels xxx | 添加镜像xxx | 清华镜像：https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ |
|                                |             |                                                              |
|                                |             |                                                              |

## conda安装

### Windows下安装

双击安装包安装即可。

默认安装路径：

```bash
C:\ProgramData\anaconda3
```



### Ubuntu下安装anaconda

#### 下载

推荐清华镜像站，下载比较快：<https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/>

#### 安装

1）打开terminal，运行下载的.sh文件。

```
bash Anaconda3-5.2.0-Linux-x86_64.sh
```

2）进入注册信息页面，输入yes；阅读注册信息，然后输入yes；查看文件即将安装的位置，按enter，即可安装。

3）安装完成后，收到加入环境变量的提示信息，输入yes

4）重启终端，即可使用Anaconda3。

5）若在终端输入 python，仍然会显示Ubuntu自带的python版本，我们执行

```
sudo gedit ~/.bashrc
export PATH="/home/xupp/anaconda3/bin:$PATH"
source ~/.bashrc
```

或者

```
echo ' export PATH="/home/ubuntu/anaconda3/bin:$PATH"'>> ~/.bashrc
source ~/.bashrc
```



### Ubuntu下安装miniconda

#### 下载

下载地址：https://docs.conda.io/en/latest/miniconda.html，这里我选择安装`Python3.7 Miniconda3 Linux 64-bit`。

#### 安装

```shell
$ bash Miniconda3-py37_4.10.3-Linux-x86_64.sh 
```

一路回车，输入yes，默认安装位置:

```shell
Miniconda3 will now be installed into this location:
/home/alex/miniconda3
```

更新环境变量

```shell
$ source ~/.bashrc
```



> 设置启动终端不进入base环境的方法:
>
> ```shell
> (base) alex@alex-Mi-Gaming-Laptop-15-6:~$ conda deactivate
> alex@alex-Mi-Gaming-Laptop-15-6:~$ conda config --set auto_activate_base false
> ```

#### 示例：创建环境

新建一个conda环境

```shell
$ conda create --name alex python=3.6
```

激活环境

```shell
$ conda activate alex
```

> 退出环境
>
> $ conda deactivate

设置conda清华源

在用户根目录编辑`.condarc`文件：`vi ~/.condarc`，添加如下内容：

```shell
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

## 基本软件包安装

```shell
# jupyter
(jizhi) allex@alex-Mi:~$ conda install jupyter

# matplotlib
(jizhi) allex@alex-Mi:~$ conda install matplotlib
```



### 在线安装

1. 用conda install pkgname即可，pkgname为包名

2. 当找不到包的时候，尝试下面的语句

   conda install -c conda-forge pkgname

3. 依旧找不到的话，用下面的搜索包名，进行最后尝试

   anaconda search -t conda pkgname    找到需要的pkgname全名后，输入下面的指令
   anaconda show pkgname（全名），会列出channel_url
   最后根据列出来的url（就是看着像网址的那些）选择相应版本，输入下面的指令进行下载
   conda install --channel channel_url  pkgname

### 本地安装

1. 从github（或者其他来源）中下载zip

   解压后里面有一个setup.py的文件

   在命令行中进入解压路径，输入python setup.py install

   然后会出现dist文件夹，其中会生成一个.tar.gz类型文件，后续同下种方式

2. 下载.tar.gz类型文件

   根据文件的绝对路径执行命令conda install --use-local pkg

   其中，pkg为绝对路径

### pkgs

anaconda下载的pkg都放在路径`C:\ProgramData\Anaconda3\pkgs`下了，如果在其他conda环境中安装过pkg，那么你应该可以在该文件夹中找到，新建conda环境就不需要重新下载，可以直接通过conda命令安装该路径下已经下载过的pkg。

可以使用如下命令安装`tar.bz2`类型的pkg：

```
conda install xxx.tar.bz2
```



## 不同计算机迁移环境

### Windows

#### 目的

两个计算机A、B都已经安装了anaconda，如果在计算机A上创建了`my_env`环境，现在希望将它迁移至计算机B。

#### 原理

在anaconda创建的环境默认放置的路径为：`C:\Users\用户名\.conda\envs`，例如创建的`my_env`环境路径为：`C:\Users\用户名\.conda\envs\my_env`。

新创建环境后，会将该路径添加至`C:\Users\用户名\.conda\environment.txt`文件。

#### 步骤

1. 将`my_env`环境所在文件夹`C:\Users\用户名\.conda\envs\my_env`直接从计算机A拷贝至计算机B；
2. 在计算机B，编辑`C:\Users\用户名\.conda\environment.txt`文件，添加新拷贝过来的`my_env`环境路径即可。

### Linux

#### 步骤

1. 拷贝`my_env`

   可以在计算机A的conda环境安装路径下（/home/allex/miniconda3/envs），找到my_env环境文件夹。并拷贝至计算机B，例如存放在/home/my_env。

2. 拷贝计算机A的pkgs目录（/home/allex/miniconda3/pkgs），替换计算机B的pkgs目录。

   这是因为，环境中的安装包大都在pkgs目录下。

3. 创建虚拟环境

   在计算机B上基于拷贝过来的环境，创建新的环境。

   ```shell
   $ conda create -n 新建虚拟环境名字 --clone 拷贝过来的环境所在目录如/home/my_env --offline
   ```


## 在指定环境下启动juypter notebook

### 安装jupyter

激活指定环境后，安装：

```bash
# 方法1：使用conda安装
conda install jupyter

# 方法2：使用pip安装
pip install jupyter
```

### 启动

```bash
jupyter notebook
```



## 创建软链接

当在Windows安装Anaconda后，在Anaconda环境下只能使用`python`而不能使用`python3`。

### 原因

其原因为：

- 在Windows上，Anaconda默认只创建`python.exe`，不创建`python3.exe`
- 在Linux/Mac系统上，Anaconda通常会同时创建`python`和`python3`两个命令

- Anaconda的安装路径中只有`python.exe`，没有`python3.exe`
- 在Windows上，Anaconda没有自动创建`python3`的符号链接



### 解决方法

以管理员身份打开命令提示符，找到Anaconda安装目录下的python.exe。

如果是base环境，对应的安装目录为`C:\ProgramData\anaconda3`，可进入该目录创建链接：

```bash
$ cd C:\ProgramData\anaconda3
$ mklink python3.exe python.exe
```

如果是其他创建的环境，对应的安装目录一般为`C:\Users\93114\.conda\envs\环境名称`

```bash
$ C:\Users\93114\.conda\envs\环境名称
$ mklink python3.exe python.exe
```

### 确认

进入conda环境后，输入如下命令查看版本：

```bash
$ python3 --version
```



## 镜像源

### Windows添加国内镜像源

打开anaconda prompt窗口，执行如下命令：

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
# 设置搜索时显示通道地址
conda config --set show_channel_urls yes
```

在C:\Users\<你的用户名> 下就会生成配置文件.condarc，如下

```
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - defaults
ssl_verify: true
show_channel_urls: true
```

通过命令**conda info**查看配置修改是否生效

删除镜像源

```
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```



