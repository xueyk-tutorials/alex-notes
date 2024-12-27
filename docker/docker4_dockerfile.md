# DockerFile

dockerfile是用来构建docker镜像的文件！命令参数脚本！

构建步骤：

1、编写一个dockefile文件

2、docker build构建成为一个镜像

3、docker run运行镜像

4、docker push发布镜像（DockerHub，阿里云镜像仓库）

## 语法

### 指令包括如下

| Dockerfile 指令 | 说明                                                         |
| :-------------- | :----------------------------------------------------------- |
| FROM            | 指定基础镜像，用于后续的指令构建。                           |
| MAINTAINER      | 指定Dockerfile的作者/维护者。（已弃用，推荐使用LABEL指令）   |
| LABEL           | 添加镜像的元数据，使用键值对的形式。                         |
| RUN             | 在构建过程中在镜像中执行命令。                               |
| CMD             | 指定容器创建时的默认命令。（可以被覆盖）                     |
| ENTRYPOINT      | 设置容器创建时的主要命令。（不可被覆盖）                     |
| EXPOSE          | 声明容器运行时监听的特定网络端口。                           |
| ENV             | 在容器内部设置环境变量。                                     |
| ADD             | 将文件、目录或远程URL复制到镜像中。                          |
| COPY            | 将文件或目录复制到镜像中。                                   |
| VOLUME          | 为容器创建挂载点或声明卷。                                   |
| WORKDIR         | 设置后续指令的工作目录。                                     |
| USER            | 指定后续指令的用户上下文。                                   |
| ARG             | 定义在构建过程中传递给构建器的变量，可使用 "docker build" 命令设置。 |
| ONBUILD         | 当该镜像被用作另一个构建过程的基础时，添加触发器。           |
| STOPSIGNAL      | 设置发送给容器以退出的系统调用信号。                         |
| HEALTHCHECK     | 定义周期性检查容器健康状态的命令。                           |
| SHELL           | 覆盖Docker中默认的shell，用于RUN、CMD和ENTRYPOINT指令。      |

### CMD

为启动的容器指定默认要运行的程序，程序运行结束，容器也就结束。CMD 指令指定的程序可被 docker run 命令行参数中指定要运行的程序所覆盖。

**注意**：如果 Dockerfile 中如果存在多个 CMD 指令，仅最后一个生效。

格式如下：

```bash
CMD <shell 命令> 
CMD ["<可执行文件或命令>","<param1>","<param2>",...] 
CMD ["<param1>","<param2>",...]   # 该写法是为 ENTRYPOINT 指令指定的程序提供默认参数
```

CMD于RUN有很大不同：

- CMD 在docker run 时运行；
- RUN 是在 docker build过程中运行。



### VOLUME

定义匿名数据卷。在启动容器时忘记挂载数据卷，会自动挂载到匿名卷。在启动容器 docker run 的时候，我们可以通过 -v 参数修改挂载点。

作用：

- 避免重要的数据，因容器重启而丢失，这是非常致命的。
- 避免容器不断变大。

格式：

```
VOLUME ["<路径1>", "<路径2>"...]
VOLUME <路径>
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

