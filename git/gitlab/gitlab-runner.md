# gitlab-runner

## 安装与注册

### 使用docker

1. 下载gitlab-runner镜像

```shell
#可以使用如下命令先搜索一下镜像
$ docker search gitlab-runner
#下载镜像
$ docker pull gitlab/gitlab-runner
```

2. 运行gitlab-runner镜像

```shell
$ docker run -d --name gitlab-runner \
 -v /srv/gitlab-runner/config:/etc/gitlab-runner \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /home/cetcs:/home/cetcs \
 gitlab/gitlab-runner
```

> - 添加端口映射和数据卷挂载
>
> ```shell
> docker run -d --name runner-v1 \
>  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
>  -v /var/run/docker.sock:/var/run/docker.sock \
>  -v /home/cetcs:/home/cetcs \
>  -p 0.0.0.0:22:22 \
>  -p 0.0.0.0:443:443 \
> gitlab/runner-v1
>```
> 
>- 如果是生产环境，容器退出时需要自动重启，则需要将--restart配置为always：
> 
>```shell
> $ docker run -d --name gitlab-runner --restart always \
>  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
>  -v /var/run/docker.sock:/var/run/docker.sock \
>  gitlab/gitlab-runner:latest
> ```

3. 进入docker

```shell
$ docker exec -it 0c790a7b6563 bash
```

4. gitlab-runner注册

使用免交互注册方式

```shell
$ gitlab-runner register \
--non-interactive \
--executor "shell" \
--url "https://git.cetcs.com" \
--registration-token "eoaw2Aeon--uWYvwV_qP" \
--description "main-page_docker" \
--tag-list "build,deploy" \
--run-untagged="true" \
--locked="false"

$ gitlab-runner register \
--non-interactive \
--executor "docker" \
--docker-image gitlab-runner:latest \
--url "https://git.cetcs.com" \
--registration-token "eoaw2Aeon--uWYvwV_qP" \
--description "main-page" \
--tag-list "build,deploy" \
--run-untagged="true" \
--locked="false"
--access-level="not_protected"
```



添加证书配置

```shell
gitlab-runner register \
 --non-interactive \
 --executor "shell" \
 --url "https://git.cetcs.com" \
 --registration-token "eoaw2Aeon--uWYvwV_qP" \
 --tls-ca-file=/etc/gitlab-runner/ssl/git.cetcs.com.crt \
 --description "main-page" \
 --tag-list "build" \
 --run-untagged="true" \
 --locked="false"
```



gitlab-runner register \
 --non-interactive \
 --executor "shell" \
 --url "https://git.cetcs.com" \
 --registration-token "eoaw2Aeon--uWYvwV_qP" \
 --tls-ca-file=/home/cetcs/Desktop/ssl/git.cetcs.com.crt \
 --description "main-page" \
 --tag-list "build" \
 --run-untagged="true" \
 --locked="false"



docker run -d --name runner-v1 \
 -v /srv/gitlab-runner/config:/etc/gitlab-runner \
 -v /var/run/docker.sock:/var/run/docker.sock \

-v /home/cetcs:/home/cetcs \

gitlab/runner-v1



docker run -d --name runner-v1 \
 -v /srv/gitlab-runner/config:/etc/gitlab-runner \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /home/cetcs:/home/cetcs \
 -p 0.0.0.0:53:53 \
 -p 0.0.0.0:443:443 \
gitlab/runner-v1

## 问题与解决

#### ssl证书问题

如果遇到证书问题，请先下载gitlab的证书，然后在免交互注册时使用`--tls-ca-file`配置参数传入crt证书。

例如：

```shell
--tls-ca-file=/home/gitlab-runner/ssl/git.cetcs.com.crt 
```

#### X509

参考：https://docs.gitlab.com/runner/configuration/tls-self-signed.html

注册时返回如下错误：

```shell
ERROR: Registering runner... failed                 runner=eoaw2Aeo status=couldn't execute POST against https://git.cetcs.com/api/v4/runners: Post "https://git.cetcs.com/api/v4/runners": x509: certificate relies on legacy Common Name field, use SANs instead
PANIC: Failed to register the runner.
```

解决方法：

1. 下载好gitlab证书
2. 将gitlab的证书拷贝至默认目录下

```shell
$ mkdir /etc/gitlab-runner/certs
$ cp git.cetcs.com.crt /etc/gitlab-runner/certs/git.cetcs.com.crt
```

3. 创建

```shell
$ openssl s_client -showcerts -connect git.cetcs.com:443 -servername git.cetcs.com < /dev/null 2>/dev/null | openssl x509 -outform PEM > /etc/gitlab-runner/certs/git.cetcs.com.crt
```

4. 确认

```shell
echo | openssl s_client -CAfile /etc/gitlab-runner/certs/git.cetcs.com.crt -connect git.cetcs.com:443 -servername git.cetcs.com
```

#### 无法注册

```shell
ERROR: Registering runner... failed                 runner=eoaw2Aeo status=couldn't execute POST against https://git.cetcs.com/api/v4/runners: Post "https://git.cetcs.com/api/v4/runners": EOF
PANIC: Failed to register the runner.
```

应该是网络无法连接的问题，确定是否可以正常Ping同gitlab，可以通过配置hosts使其正确连接。



#### 网络连接问题

```shell
dial tcp 10.16.65.230:443: connect: connection refused
```



## 移除

```shell
$ gitlab-runner verify --delete --name=main-page
```

或者

```shell
$ gitlab-runner unregister --url http://gitlab.example.com/ --token t0k3n
# 或
$ gitlab-runner unregister --name test-runner
```



## 脚本

在gitlab项目主页，增加一个文件，命名为.gitlab-ci.yml

### 示例

```shell
stages:
- build
- test
- deploy
build:
	stage: build
	tags:
	- build
	script:
	- echo "代码编译"
	- ls
test:
	stage: test
	tages:
	- test
	script:
	-echo "代码测试"
deploy:
	stage: deploy
	tags:
	- deploy
	script:
	- echo "项目部署"
	- scp index.html root@192.168.1.12:/usr/share/nginx/html

```





