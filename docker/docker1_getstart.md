# Docker教程

## 相关网站

DockerDocs: https://docs.docker.com/

DockerHub: https://hub.docker.com/

## Docker工作原理

Docker是一个Server-Client结构的系统，Docker的守护进程运行在主机上，通过socket从客户端访问。DockerServer接收到DockerClient的指令，就会执行这个命令。

## Docker安装

### Ubuntu下在线安装

推荐安装方式

https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

docker-ce:社区版本

docker-ee:企业版本

- 更新软件包

```bash
sudo apt update
sudo apt upgrade
```

- 安装依赖

```bash
apt-get install ca-certificates curl gnupg lsb-release
```

- 安装官方密钥

```bash
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

- 添加docker软件源

```bash
sudo add-apt-repository "deb [arch=$(dpkg --print-architecture)] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"


# 或者Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] http://mirrors.aliyun.com/docker-ce/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

- 安装docker

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

> 如果需要安装特定版本，需要先通过如下命令查看相关版本信息：
>
> ```bash
> apt list -a docker-ce
> ```
>
> 然后根据所需版本安装，根据版本信息对应替换`<VERSION>`即可：
>
> ```bash
> apt install docker-ce=<VERSION> docker-ce-cli=<VERSION> containerd.io
> ```

- 配置用户组（可选）

默认情况下，只有root用户和docker组的用户才能运行Docker命令。我们可以将当前用户添加到docker组，以避免每次使用Docker时都需要使用sudo。命令如下：

```bash
sudo usermod -aG docker $USER
```

> 重新登录才能使更改生效。

### Ubuntu下离线安装

参考：

https://blog.csdn.net/fy512/article/details/123257474

https://blog.csdn.net/qq_44858888/article/details/124084408



下载安装包https://download.docker.com/linux/static/stable/x86_64/，例如选择**docker-18.06.0-ce.tgz**进行下载。

### 卸载

```bash
# 如果在运行，先停止docker
systemctl stop docker
 
# 卸载docker软件包
apt purge docker-ce docker-ce-cli containerd.io
 
# 删除docker项目目录，我还要多删除一个自定义的缓存目录
rm -rf /var/lib/docker
rm -rf /etc/docker
 
# 删除docker用户组
groupdel docker
```



## 基本配置

### 配置国内镜像源

创建或修改 /etc/docker/daemon.json 文件，修改为如下形式

```shell
{
    "registry-mirrors" : [
    "https://registry.docker-cn.com",
    "https://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com",
    "https://cr.console.aliyun.com/"
  ]
}
```

或者直接通过如下命令修改

```bash
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://dockerpull.com",
        "https://docker.anyhub.us.kg",
        "https://dockerhub.jobcher.com",
        "https://dockerhub.icu",
        "https://docker.awsl9527.cn"
    ]
}
EOF
```



重启docker

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

查看是否生效

```shell
docker info
```

### 安装路径

docker的安装路径为：`/var/lib/docker`，一般镜像下载了之后都会在这个位置。

### 启动/停止docker

#### WSL2下相关命令

```shell
$ sudo service docker stop
$ sudo service docker start
$ sudo service docker restart
```

#### 一般Ubuntu下相关命令

```shell
$ systemctl start docker
$ sudo systemctl restart docker
$ docker version
```

>Note:
>
>启动后出现“Got permission denied...."问题，解决方法是将
>
>当前用户添加到docker用户组。
>
>```shell
>$ sudo groupadd docker
>$ sudo gpasswd -a $USER docker
>$ newgrp docker
>```
>
>然后重新启动docker: `systemctl restart docker`

### Hello World

```shell
$docker run hello-world
```

正常运行结果如下：

```bash
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```



## docker使用

### docker命令

```python
systemctl start docker      #启动docker
systemctl restart docker    #重启docker
systemctl daemon-reload     #重载服务配置文件
docker version   #查看版本
docker images    #查看当前镜像
```

### 文件目录

镜像文件将保存在 /var/lib/docker/image/overlay2 目录下。
容器数据将保存在 /var/lib/docker/containers 目录下。
数据卷和持久化数据将保存在 /var/lib/docker/volumes 目录下。
Docker 的运行时信息和日志可以在 /var/log/docker 目录下找到。

## docker原理

docker是client-server运行架构，启动docker服务端后才能够正常使用docker，服务端会启动docker daemon守护进程。

## 典型dockers镜像

### Ubuntu

```bash
docker pull ubuntu:focal-20241011
docker pull ubuntu:20.04
```



## docker hub无法连接问题

如果docker因网络问题无法连接其服务器，可以尝试通过国内一些镜像地址访问，只需要，例如`hub.geekery.cn`等。

```bash
$ docker pull hub.geekery.cn/hello-world
$ docker pull hub.geekery.cn/ubuntu:20.04
$ docker search hub.geekery.cn/ros2:foxy
```























