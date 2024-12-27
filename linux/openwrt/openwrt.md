

# 默认配置



固件文件rk3399-eflasher-friendlywrt-23.05-docker-20231223.img.gz



## 网络

### 防火墙



# docker



## docker服务控制命令

```bash
/etc/init.d/dockerd start
/etc/init.d/dockerd stop
/etc/init.d/dockerd restart
```

## 开机运行容器

### 创建容器

使用镜像创建容器，命名为"drone"：

```bash
docker run --name="drone" -it --net=host [your_image_name]:v1 /bin/bash
```

这样容器创建好后，后续每次开机只需要启动容器就行了。

### 创建开机服务

在`/etc/init.d`路径下增加文件并命名为myappdrone，内容如下：

```bash
#!/bin/sh /etc/rc.common
# Copyright (C) 2008 OpenWrt.org

START=99

start() {
    log_file="/root/Desktop/log.txt"

    touch $log_file
    current_time=$(date "+%Y-%m-%d %H:%M:%S")
    echo "[$current_time init.d/appdrone] start appdrone">>$log_file
    bash /root/Desktop/start_docker.sh
}
```

然后开启开机启动服务

```bash
/etc/init.d/myappdrone enable
```

这样开机后就会运行`/root/Desktop/start_docker.sh`脚本

### 创建自定义脚本

创建`/root/Desktop/start_docker.sh`脚本。

赋予可执行权限：

```bash
chmod 777 /root/Desktop/start_docker.sh
```

脚本内添加内容如下：

```bash
#!/bin/bash

container="drone"

log_file="/root/Desktop/log.txt"
touch $log_file
echo "[$(date "+%Y-%m-%d %H:%M:%S") start_docker.sh]: Try to start docker container">>$log_file

flag_success=false
# wait until docker start
cnt=1
while [ $cnt -le 5 ]
do
    echo "[$(date "+%Y-%m-%d %H:%M:%S") start_docker.sh]: Trying ${cnt}...">>$log_file
    echo $(docker version)>>$log_file
    is_running=$(docker version |grep "Server:" | awk '{print $1}')
    if [ -z "${is_running}" ]; then
        echo "[$(date "+%Y-%m-%d %H:%M:%S") start_docker.sh]: Docker not running">>$log_file
        echo $(docker version)>>$log_file
    else
        echo "[$(date "+%Y-%m-%d %H:%M:%S") start_docker.sh]: Docker already in running">>$log_file
        flag_success=true
        # start container
        docker start ${container}
        sleep 3s

        id=$(docker ps |grep "${container}" | awk '{print $1}')
        if [ -z "${id}" ]; then
            echo "[$(date "+%Y-%m-%d %H:%M:%S") start_docker.sh]: Start container failed">>$log_file
        else
            docker exec -it ${id} /bin/bash
            echo "[$(date "+%Y-%m-%d %H:%M:%S") start_docker.sh]: Start docker container success!">>$log_file
        fi

        break
    fi
    let cnt++
    sleep 1s
done

if [ "${flag_success}" = true ]; then
    echo "[$(date "+%Y-%m-%d %H:%M:%S") start_docker.sh]: Success">>$log_file
else
    echo "[$(date "+%Y-%m-%d %H:%M:%S") start_docker.sh]: Failed">>$log_file
fi
```





# 开机启动

## OpenWrt 启动脚本管理

在 OpenWrt 中，启动脚本是用于在系统启动时自动运行特定程序的脚本。这些脚本通常放置在` /etc/init.d` 目录下，并可以使用如下命令进行控制：

```bash
/etc/init.d/[服务脚本名称] [命令选项]
```

支持的命令选项有：

- start   # 启动服务
- stop    # 停止服务
- restart # 重启服务
- reload  # 重新载入配置文件, 如果失败则重启
- enable  # 启用开机自启动, 实际上是在/etc/rc.d/下创建S??和K??开头的软链
- disable  # 禁用开机自启动, 实际上是删除/etc/rc.d/下对应的软链

并且可以通过 *enable* 和 *disable* 命令来管理它们的自启动状态，当程序添加至启动后，会在`/etc/rc.d/`路径下生成软链接。



## 创建和管理启动脚本

要创建一个新的启动脚本，您需要在 */etc/init.d* 目录下创建一个脚本文件，并赋予它执行权限。例如，创建一个名为 *example* 的脚本，您可以按照以下步骤操作：

```bash
\#!/bin/sh /etc/rc.common
\# Example script
\# Copyright (C) 2007 OpenWrt.org

START=10
STOP=15
start() {
  echo start
  \# commands to launch application
}

stop() {
  echo stop
  \# commands to kill application
}
```

然后，您需要给脚本添加可执行权限。

```bash
chmod a+x /etc/init.d/example
```

通过 *enable* 命令来创建启动时的符号链接，这样脚本就会在系统启动时自动执行：

```bash
/etc/init.d/example enable
```

这将在 `/etc/rc.d/` 目录下创建一个名为 `S10example` 的符号链接，表示该脚本将在系统启动时执行。数字 10 表示脚本的启动顺序，数字越小，脚本越早执行。

> 注意数字不能超过100！最大99！

- 启动和停止顺序

在 OpenWrt 的启动脚本中，*START* 和 *STOP* 变量分别决定了脚本在系统启动和关闭时的执行顺序。例如，*START=50* 和 *STOP=10* 表示脚本将在启动时较晚执行，在关闭时较早执行。如果多个脚本有相同的 *START* 或 *STOP* 值，则它们的执行顺序由脚本名称的字母顺序决定。

- 自定义命令和帮助信息

您还可以在启动脚本中添加自定义命令和帮助信息，以便在需要时执行特定的操作。例如：

```bash
EXTRA_COMMANDS="custom"
EXTRA_HELP=" custom Help for the custom command"

custom() {
    echo "custom command"
    \# do your custom stuff
}
```

这样，当您使用 *custom* 参数调用脚本时，将执行 *custom()* 函数中定义的命令。

- 查询启动脚本状态

要查询所有启动脚本的自启动状态，可以使用以下命令：

```bash
for F in /etc/init.d/*; do $F enabled && echo $F on || echo $F disabled; done
```

这将显示每个脚本的启动状态，*on* 表示已启用自启动，*disabled* 表示已禁用自启动。

通过这些方法，您可以有效地管理 OpenWrt 中的启动脚本，确保所需的服务和应用程序在系统启动时自动运行。



## rc.local

在`/etc/init.d/done`文件中，设置的rc.local的启动。而done的启动优先级为95。

这里boot()表示启动。

```bash
#!/bin/sh /etc/rc.common
# Copyright (C) 2006 OpenWrt.org

START=95
boot() {
    mount_root done
    rm -f /sysupgrade.tgz && sync

    # process user commands
    [ -f /etc/rc.local ] && {
        sh /etc/rc.local
    }

    # set leds to normal state
    . /etc/diag.sh
    set_state done
}
```

