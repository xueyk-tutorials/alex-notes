# Linux

## 基本工具

- python

  安装

  ```shell
  $ sudo apt-get update
  $ sudo apt-get install python3
  $ sudo apt-get install pip
  ```

- net-tools

  安装：`sudo apt-get install net-tools`



## 网络常用端口号

> 21/tcp FTP 文件传输协议
>
> 22/tcp SSH 安全登录、文件传送（SCP）和端口重定向
>
> 23/tcp Telnet 不安全的文本传送
>
> 25/tcp SMTP Simple Mail Transfer Protocol (E-mail)
>
> 69/udp TFTP Trivial File Transfer Protocol
>
> 79/tcp finger Finger
>
> 80/tcp HTTP 超文本传送协议 （WWW）
>
> 88/tcp Kerberos Authenticating agent
>
> 110/tcp POP3 Post Office Protocol (E-mail)
>
> 113/tcp ident old identification server system
>
> 119/tcp NNTP used for usenet newsgroups
>
> 220/tcp IMAP3
>
> 443/tcp HTTPS used for securely transferring web pages



## 账号

- 切换账号：

  ```shell
  $ su - alex     #切换至alex账号
  ```


### 设置用户权限

#### sudo不需要密码

有时候设置开机启动脚本时，需要sudo运行某个脚本，为了避免输入密码，可以给用户添加权限。

```shell
$ sudo  vim  /etc/sudoers
### 添加权限设置
# 使用户操作都不需要密码
你的账户名  ALL=(ALL:ALL) NOPASSWD: ALL
# 特定操作不需要密码
你的账户名  ALL=(ALL:ALL) NOPASSWD: /bin/sh /usr/local/sbin/start_docker.sh
```



## 时间日期

查看时间日期命令：`date`

设置时间：

```shell
$ sudo date -s "2022-7-14 09:18:00"
```

### 时间同步

参考：

https://blog.csdn.net/Geeker_boy/article/details/110244749?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-110244749-blog-114752452.pc_relevant_multi_platform_whitelistv1_exp2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-110244749-blog-114752452.pc_relevant_multi_platform_whitelistv1_exp2&utm_relevant_index=2



https://blog.csdn.net/qq_41943585/article/details/89679198?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-89679198-blog-114752452.pc_relevant_multi_platform_whitelistv1_exp2&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-89679198-blog-114752452.pc_relevant_multi_platform_whitelistv1_exp2&utm_relevant_index=1



实现计算机A和B的时间同步：

```shell
# A
sudo apt-get install ntp
sudo apt-get install ntpdate
ifconfig                        //查看ip地址
# B
sudo ntpdate -q 电脑A的ip地址    //检查和电脑A的时间差
sudo ntpdate -d 电脑A的ip地址    //和电脑A进行时间同步
```



## scp免交互

我们需要在gitlab-runner使用scp命令自动将相关代码拷贝至目标服务器，故需要配置下免交互。

```shell
# //gitlab-runner
# scp-copy-id root@<目标服务器IP>
```



