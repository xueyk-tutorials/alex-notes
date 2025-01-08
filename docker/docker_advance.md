# docker

## 拷贝docker镜像

1. 将容器制作镜像，`docker commit`

   ```shell
   $ docker commit -m "gitlab runner" -a "alex" <容器ID> gitlab/runner-v1
   
   # 添加版本
   $ docker commit -m "update" -a "alex" <容器ID> drone:v2
   ```

2. 将镜像保存到文件，`docker save`

   ```shell
   $ sudo docker save -o /tmp/gitlab-runner_v1.tar gitlab/runner-v1
   ```

3. 将镜像文件拷贝

   可以使用scp命令将镜像拷贝至目的主机

4. 加载镜像文件，`docker load`

   ```shell
   $ docker load -i ./gitlab-runner_v1.tar
   ```

   

## 容器图像显示

### 使用mobaxterm

https://blog.51cto.com/u_15473553/4939407



### 使用XServer

#### 宿主机安装xserver

```shell
$ sudo apt install x11-xserver-utils
$ xhost +                             #允许客户访问
```

#### 启动容器

```shell
$ docker run -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $HOME/.Xauthority:/root/.Xauthority --name [container name] [image name] bash
```



## 修改容器的端口

参考：https://www.cnblogs.com/kingsonfu/p/11578073.html





## 网络连接问题

https://blog.csdn.net/lvshaorong/article/details/69950694
