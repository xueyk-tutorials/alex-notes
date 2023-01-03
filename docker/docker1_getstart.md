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
service docker restart
```

查看是否生效

```shell
docker info
```

### 安装路径

docker的安装路径为：`/var/lib/docker`，一般镜像下载了之后都会在这个位置。

### 启动docker

#### WSL2启动

```shell
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
$docker images
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



























