# 基于项目开发实例讲解git如何使用

git功能强大，开源免费，是程序开发的必备神器。但学习git的周期较长，初学者由于项目经验不足，只看一些git基本命令无法对git有个系统性的了解，因为git旨在为团队进行工程代码的长期维护提供一系列功能，如果没有一个系统性的项目经验或团队开发中每个角色的工作有了解，无法真正掌握git，因为你用于无法脱离项目工程实践去谈论如何掌握项目开发工具！我们只有以项目团队成员角色出发，了解正常的项目开发流程，这样才能更好的理解git提供的那些功能到底如何实际应用的。

本教程以一个实际简化项目为例，讲解git在项目开发中的使用。

## 课程配置环境

操作系统：Ubuntu18.04

云服务：gitee(码云)

工具：git(Ubuntu自带)；

编辑器：vscode，插件GitLens、插件Git Graph

> 备注：
>
> vscode默认就是自带git插件的，但功能有限，所以我们需要安装GitLens和GitGraph。

### GitLens

- 安装

搜索插件GitLens，然后点击安装即可，安装完成后会在左侧工具栏添加一个快捷图标（如黄色箭头所示）。

![image-20211103131156765](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20211103131156765.png)

- 启动

vscode打开一个包含git的目录，通过点击工具栏的快捷图标即可！根据配置不同，有时候你安装后并没有快捷图标，其实不是没有安装成功，而是gitlens可能放到了SOURCE CONTROL面板中。打开左侧工具栏第三个图标即可看到SOURCE CONTROL面板添加了如下一些内容：

![image-20211103220010164](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20211103220010164.png)

### Git Graph

- 安装

  通过插件搜索，找到Git Graph然后安装。

![image-20211103131524795](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20211103131524795.png)

- 启动

vscode打开一个包含git的目录，Git Graph是添加在vscode自带的git中，然点击左侧工具栏第三个快捷图标，就可以看到在SOURCE CONTROL面板多出了Git Graph的图标按钮！

![image-20211103132155889](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20211103132155889.png)

## 项目背景

该项目为某无人机应用开发项目，有三个成员：**Alex**，**Bill**，**Carl**，三个人的分工为：

- Alex：项目负责人，项目仓库管理者，飞控相关开发，使用项目账号：`drone_dev_group`；
- Bill：吊舱控制与应用相关开发，使用项目成员账号：`drone_dev_bill`；
- Carl：地面站开发，使用项目成员账号：`drone_dev_carl`。

我们将项目名称命名为`drone_onboard_app`。

## 第一天的工作

项目创建

各分支创建

### 项目管理员Alex的工作

#### 远程gitee仓库操作

- **创建gitee仓库**

使用项目/公司的gitee账号登录gitee，创建一个云端仓库用于存放代码。仓库名称名为drone_onboard_app，其他默认就好（闭源、不进行仓库初始化等）。

- **添加开发者**

gitee支持多个开发者共同维护一个项目仓库，进入远程仓库的配置（Settings）->开发者（Developer）界面，添加开发者，可以通过查找其他项目成员的gitee用户名（就是项目组各成员的账号）来添加开发者。

#### 本地仓库操作

- **创建本地仓库**

```shell
$ mkdir drone_onboard_app 
$ cd drone_onboard_app
$ git init
```

我们手动添加一个`README.md`文件，并且输入一些内容。当前的工程目录文件为：

```shell
$ drone_onboard_app$ ls -a
.  ..  .git  README.md
```

- **仓库配置和第一次提交**

```shell
###设置gitee用户名和邮箱
$ git config --global user.name "drone_dev_group"
$ git config --global user.email "drone_dev_group@163.com"

###
$ git add README.md
$ git commit -m ">init"
[master (root-commit) cf30871] >init
 1 file changed, 4 insertions(+)
 create mode 100644 README.md
```

- **添加gitee远程仓库**

```shell
$ git remote add origin git@gitee.com:jizhi_group/drone_onboard_app.git
```

可以使用vscode自带的git工具添加远程仓库

可以使用vscode的GitLens插件添加远程仓库

- **在本地进行ssh公钥配置**

这个步骤有非常多教程，请参考相关教程。

#### 编写程序框架

根据项目需求，进行程序框架的编写，例如创建必要的文件夹、文件，填写README.md等。

#### 推送至gitee

```shell
$ git push -u origin master      # 注意这里默认远程仓库名称为origin
```

### 项目其他成员的工作

其他项目成员登录各自gitee账号后即可看到出现了仓库**drone_onboard_app**，将代码拉到本地：

```shell
$ git clone git@gitee.com:jizhi_group/drone_onboard_app.git
```

> 项目组其他成员同样需要配置各自的ssh公钥。

## 第二天的工作

### Alex的工作

负责**dirver**部分的开发！

#### 创建分支

在本地创建分支用于开发。

```shell
$ git checkout -b dev_driver     #创建分支dev_driver并切换至分支
# 等同于如下两个命令
# $ git branch dev_driver
# $ git checkout dev_driver
```

> 注意：
>
> 1. 创建分支后的状态就是当前master分支的状态，也就是当前master有什么，创建后的分支就包含什么。

#### 进行开发

编写代码，例如添加了`driver.py`文件，并写了一些基本代码。

这时分支的文件目录如下：

```shell
|-- drone_onboard_app/
	|-- README.md
	|-- driver.py
```

将编写的代码进行了一系列测试，确认没问题后进行提交。

#### 分支本地提交

由于**driver**部分的功能只有Alex进行开发，所有只需要在本地提交即可。

```shell
$ git status
$ git add driver.py
$ git commit -m "添加了driver，实现了基本功能"
```

#### 合并至主线

```shell
$ git checkout master       # 切换至主线分支
$ git merge dev_driver      # 合并分支dev_driver->master
```

#### 将master推送至远程仓库

```shell
$ git push origin master    # 在master分支下，推送至远程
```

### Bill的工作

和Carl一起负责**app**部分的开发！

#### master分支更新

开发者养成习惯，每天工作前把master分支更新一下。

```shell
$ git pull origin master      # 在master分支下执行，拉取远程仓库更新
```

> 由于项目是多人维护的，大家的工作相互隔离又有相互依赖，有可能你今天的工作内容依赖其他人最新的代码，所以主线合并了别人代码之后你需要先拉到本地。

#### 创建分支

在本地创建分支用于开发。

```shell
$ git checkout -b dev_app     #创建分支dev_app并切换至分支
```

由于这个分支的工作内容较多，需要两个人一起开发，故只有将分支推送到远程仓库后，其他人才能够拉取同一个分支，这样才能在一个分支上协同开发。

```shell
$ git push origin dev_app     # 将分支推送至远程仓库，远程仓库会同样创建dev_app分支
```

可以通过`git branch`命令查看本地分支，通过`git branch -a`命令查看本地和远程分支。

#### 进行开发

编写代码，例如添加了`app_functions.py`文件，并写了一些基本代码。

这时分支的文件目录如下：

```shell
|-- drone_onboard_app/
	|-- README.md
	|-- driver.py
	|-- app_functions.py
```

#### 分支提交

```shell
$ git status
$ git add app_functions.py
$ git commit -m "添加了app_functions，完成初步功能实现"
```

#### 推送至远程分支

由于这个分支的工作内容较多，需要两个人一起开发，故只有将分支推送到远程仓库后，其他人才能够拉取同一个分支，这样才能在一个分支上协同开发。

```shell
$ git push origin dev_app
```









### 删除云端分支

```shell
$ git push origin -d <branch-name>
```

