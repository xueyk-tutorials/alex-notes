# Nginx

## 安装

```shell
apt-get install nginx
```

Nginx的根目录为：/etc/nginx

## 基本命令

### 设置开机启动

```shell
root@hecs-164752:~# systemctl enable nginx.service
Synchronizing state of nginx.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable nginx
```

### 重启nginx服务

一般配置文件更改后需要重启服务。

```shell
$ sudo nginx -s reload
```

### 停止运行

```shelll
$ sudo nginx -s stop
```

## 配置

### 配置说明

Nginx的主配置文件为/etc/nginx/nginx.conf，其默认会引入/etc/nginx/conf.d路径下其余的.conf配置文件。

### server配置

这个是最关键的配置，主要用来配置域名、服务器中网站放置的目录等。

server配置项可以在`/etc/nginx/nginx.conf`下配置，也可以放在`/etc/nginx/conf.d`路径下新建一个.conf配置文件中（**推荐**）。

server配置项位于http之内之所以分开是为了方便管理，其基本说明如下：

```python
server {
    listen       80;
    server_name  _;   #监听的域名，例如www.boolpi.com，_代表所有
    root /home/filename/;  # 站点根目录
    #charset koi8-r;  指定编码方式
    #access_log  /var/log/nginx/host.access.log  main;  #单独指定该服务的日志路径
    # 转发路径
    location / {                       # 10.0.0.11  == http://10.0.0.11:80/   /表示跟
        root   /usr/share/nginx/html;  # 访问路径为/时 到/usr/share/nginx/html下找文件将被替换为 root 后的路径
        index  index.html index.htm;   # 默认的主页文件 
      # 该配置表示当访问了地址为10.0.0.11时将返回
                                       # /usr/share/nginx/html/index.html 或/ htm文件
    }
    #error_page  404              /404.html; # 遇到404时要返回的页面
    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html; # 当遇到5xx服务器错误时 返回
    location = /50x.html { #/usr/share/nginx/html/50x.html
        root   /usr/share/nginx/html;     
    }
  # 一个server中可以包含多个location配置项
```

## 示例

1. 配置`/etc/nginx/nginx.conf`

```shell
http {
		include /etc/nginx/conf.d/*.conf;
        #include /etc/nginx/sites-enabled/*;  #注释本行，不然会一直访问到nginx的默认网页
}
```

2. 进入配置目录`/etc/nginx/conf.d/`，新建一个配置文件：

```shell
# cd /etc/nginx/conf.d/
# vi wujin.conf
```

3. 内容如下：

```shell
server {
  listen 80;
  server_name _; // 你的域名或者ip，或者localhost
  root /wujin/www; // 你的克隆到的项目路径
  index index.html; // 显示首页
  location ~* ^.+\.(jpg|jpeg|gif|png|ico|css|js|pdf|txt){
    root /wujin/www;
  }
}
```

在目录`/wujin/www`下新建一个index.html如下：

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>第一个静态文件</title>
</head>
<body>
Hello world！
</body>
</html>
```

4. 重新加载nginx的配置即可

```shell
sudo nginx -s reload
```



## 访问

配置完成后可以直接在IE中输入服务器地址，例如119.3.41.86便可以访问Nginx的默认网页。如下：

![image-20220126100842663](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20220126100842663.png)