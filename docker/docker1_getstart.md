# Docker教程

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

### Ubuntu下离线安装

参考：

https://blog.csdn.net/fy512/article/details/123257474

https://blog.csdn.net/qq_44858888/article/details/124084408



下载安装包https://download.docker.com/linux/static/stable/x86_64/，例如选择**docker-18.06.0-ce.tgz**进行下载。

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

重启docker

```shell
systemctl daemon-reload
systemctl restart docker
service docker restart
```

查看是否生效

```shell
docker info
```

### 安装路径

docker的安装路径为：`/var/lib/docker`，一般镜像下载了之后都会在这个位置。

### 启动/停止docker

#### WSL2启动

```shell
$ sudo service docker stop
$ sudo service docker start
```

#### 一般Ubuntu下启动

```shell
$ systemctl start docker
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



## docker原理

docker是client-server运行架构，启动docker服务端后才能够正常使用docker，服务端会启动docker daemon守护进程。

## 典型dockers镜像

### Ubuntu

```bash
docker pull ubuntu:focal-20241011
docker pull ubuntu:20.04
```

























