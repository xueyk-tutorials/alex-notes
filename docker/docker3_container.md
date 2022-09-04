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

### 使用数据卷

- 方式一：使用命令挂载

```shell
### 命令
docker run -it -v 主机目录:容器目录 镜像名 /bin/bash
#主机和docker容器下如果没有对应目录则都会被自动创建
```

## 容器访问外部设备

### USB设备

```shell
###
#在启动docker时，通过设置--device将外部设备传入容器。
alex@alex_laptop:~$ docker run -t -i --device=/dev/ttyUSB0 tiryoh/ros2 /bin/bash
ubuntu@2a7104f82ea5:~$ ls /dev/tty*
/dev/tty  /dev/ttyUSB0
```

