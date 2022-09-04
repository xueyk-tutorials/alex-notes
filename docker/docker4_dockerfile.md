# DockerFile

dockerfile是用来构建docker镜像的文件！命令参数脚本！

构建步骤：

1、编写一个dockefile文件

2、docker build构建成为一个镜像

3、docker run运行镜像

4、docker push发布镜像（DockerHub，阿里云镜像仓库）

## 语法

### CMD

为启动的容器指定默认要运行的程序，程序运行结束，容器也就结束。CMD 指令指定的程序可被 docker run 命令行参数中指定要运行的程序所覆盖。

**注意**：如果 Dockerfile 中如果存在多个 CMD 指令，仅最后一个生效。

格式如下：

```bash
CMD <shell 命令> 
CMD ["<可执行文件或命令>","<param1>","<param2>",...] 
CMD ["<param1>","<param2>",...]   # 该写法是为 ENTRYPOINT 指令指定的程序提供默认参数
```



## 示例

### 示例1：helloworld

1. 建立一个**Dockerfile.test**文件：

```shell
FROM ubuntu
RUN echo "hello docker, this is dockerfile test"
RUN mkdir /home/alex_personal
CMD ["bash"]
WORKDIR /home
```

2. 构建镜像

```shell
$ docker build -t alex:v1.0 -f ./Dockerfile.test .
```

### 示例2：ROS2镜像

一个ROS2镜像

https://registry.hub.docker.com/r/tiryoh/ros2

