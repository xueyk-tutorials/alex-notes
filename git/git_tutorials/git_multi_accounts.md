# git——多秘钥管理

## 简介

我们在实际开发过程中，可以需要维护多个git服务器上的项目仓库，例如在gitee上放置了开源项目，在内网搭建了gitlab服务器用于项目组内部的开发，这个时候就需要我们个人计算机配置多个ssh密钥去连接不同的服务器！

安装好git后，使用git的第一步是生成ssh-key，用于拷贝至远程仓库的以建立连接。这个密钥是默认存放在`~/.ssh`文件夹下的，故第一次生成默认密钥后目录结构为：

```python
|-- .ssh/
	|-- known_hosts
	|-- id_rsa.pub    # 秘钥
	|-- id_rsa
    |-- config        # ssh远程连接的配置文件
```

由于ssh默认读取的是`id_rsa`，这里面只能存放一个密钥，本教程将详细描述如何配置多个密钥。



## 配置步骤

### 生成key

使用命令`$ ssh-keygen -t rsa -C "简要描述"`生成key，在提示保存文件时，根据这个git账号对应的取个名字，且要以**id_rsa**开头，例如id_rsa_wujin，其中wujin就是该git账号的描述，方便区分。

> 注意要输入完整的文件路径和名称！
>
> 简要描述可以使用gitee注册邮箱，例如wujin@163.com

我们根据需要，依次生成多个gitee账号对应的秘钥。

生成秘钥的操作如下：

```shell
$ ssh-keygen -t rsa -C "wujin@163.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/alex/.ssh/id_rsa): /c/Users/alex/.ssh/id_rsa_wujin
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/alex/.ssh/id_rsa_jizhi_group
Your public key has been saved in /c/Users/alex/.ssh/id_rsa_jizhi_group.pub
The key fingerprint is:
SHA256:gKQQOrWl/PKZTiq4zFvqtsqaGuCM38qQwicp504hggE cetcs_jizhi_group@163.com
The key's randomart image is:
+---[RSA 3072]----+
|E.. o            |
|o+ * .           |
|+ = . .          |
|.o .   .         |
|= o .   S        |
|Bo.+ o           |
|O*oo=            |
|O@==             |
|%%X.o            |
+----[SHA256]-----+
```

### 添加新的key至ssh

SSH默认只会读取id_rsa，所以为了让SSH识别新的私钥，需要将其添加到SSH agent
使用命令：`ssh-add ~/.ssh/id_rsa_wujin`。

> 如果报错：Could not open a connection to your authentication agent.无法连接到ssh agent
> 可执行`ssh-agent bash`命令后再执行`ssh-add`命令。
>
> 使用命令`ssh-add -l`显示当前的代理。

```shell
$ ssh-add ~/.ssh/id_rsa_wujin
Could not open a connection to your authentication agent.
$ ssh-agent bash
$ ssh-add ~/.ssh/id_rsa_wujin
Identity added: /c/Users/alex/.ssh/id_rsa_wujin (wujin@163.com)
```

### 配置config

打开`~/.ssh/config`配置文件

```shell
Host wujin
  HostName gitee.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_wujin
  
# 如果重复前面步骤，又添加了一个新帐号
Host jg
  HostName gitee.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_jg
  
# 如果git服务器搭建到内网一个计算机上，且知道其IP，可以直接使用IP
Host alex
  HostName 192.168.1.110
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_alex
```

### git服务器添加SSH key

打开生成的文件~/.ssh/id_rsa_wujin.pub，拷贝其中的key，添加至gitee的SSH Key中。

### 测试连接

```shell
$ ssh -T git@wujin
Hi 无烬! You've successfully authenticated, but GITEE.COM does not provide shell access.
```



## 使用

我们知道仓库地址一般为：`git@gitee.com:<gitee_name>/<repo_name>.git`，在git使用中**gitee.com**是默认的git远程服务器主机名字，但如果存在多个git账号，我们配置了多个秘钥并且在`~/.ssh/config`中重新配置了**主机名**、**主机地址**、**对应的秘钥**，故我们需要通过新配置的主机名去访问对应git账号的仓库。

**1. 示例**

我们要操作的git账号**wujin@163.com**，对应的秘钥保存在**id_rsa_wujin**，通过config文件可知，其主机名为wujin。

在该git账号中，某个仓库地址默认为`git@gitee.com:wujinDrone/flocking_foxy_ws.git`

那么在实际访问中应该将这个地址改为：

`git@wujin:wujinDrone/flocking_foxy_ws.git`

**2. 主机名在拉取远程仓库到本地操作中的影响**

接下来我们看下，修改和不修改访问地址的区别。

```shell
###################################################
### 使用默认主机名无法下载
###################################################
git clone git@gitee.com:wujinDrone/flocking_foxy_ws.git
Cloning into 'flocking_foxy_ws'...
[session-424e5187] Auth error: Access deined: authorize failure.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
###################################################
### 更改后可以下载
###################################################
$ git clone git@wujin:wujinDrone/flocking_foxy_ws.git
Cloning into 'flocking_foxy_ws'...
remote: Enumerating objects: 47, done.
remote: Counting objects: 100% (47/47), done.
remote: Compressing objects: 100% (43/43), done.
remote: Total 47 (delta 10), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (47/47), 22.20 KiB | 189.00 KiB/s, done.
Resolving deltas: 100% (10/10), done.
```

