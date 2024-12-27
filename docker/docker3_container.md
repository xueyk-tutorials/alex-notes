# 容器

## 端口映射

```shell
```



## 数据卷

将应用和环境打包成一个镜像

数据？如果数据都在容器中，那么我们容器删除，数据就丢失！==需求：数据可以持久化==。

容器之间可以有一个数据共享技术！Docker容器产生的数据，同步到本地！

这就是卷技术，目录的挂载，将我们容器内的目录挂载到Linux上面！

**总结一句话：容器的持久化和同步操作！容器间也可以数据共享！**

### 使用命令挂载数据卷

命令格式：`docker run -it -v 主机目录:容器目录 镜像名 /bin/bash`

- 主机和docker容器下如果没有对应目录则都会被自动创建；
- 目录一定要用绝对路径；

## 容器访问外部设备

### USB设备

```shell
###
#在启动docker时，通过设置--device将外部设备传入容器。
alex@alex_laptop:~$ docker run -t -i --device=/dev/ttyUSB0 tiryoh/ros2 /bin/bash
ubuntu@2a7104f82ea5:~$ ls /dev/tty*
/dev/tty  /dev/ttyUSB0
```



## 配置启动的容器

### 查看docker主目录

```shell
$ docker info | grep Root
WARNING: No memory limit support
WARNING: No swap limit support
 Docker Root Dir: /var/lib/docker
```

### 容器配置文件

通过`docker ps`命令获取容器id

docker启动的容器都放置在目录`/var/lib/docker/container`下，故所有配置文件都在以容器id命名的文件夹。可以以管理员账户进行配置。

```shell
$ sudo su
# ls /var/lib/docker/container/<容器ID>
96969da5c154ca0c112d9bd24b7dc231372d9d0dd8f62dcea83f3d545554facc-json.log  hosts
checkpoints                                                                mounts
config.v2.json                                                             resolv.conf
hostconfig.json                                                            resolv.conf.hash
hostname
```



### 启动docker和容器

```shell
$ systemctl start docker
$ docker start 容器名
```

