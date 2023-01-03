

# Git基础

## github用户名和密码

用户名：AlexHAHA

密码：ALEXxue0610

邮箱：xueyuankui.good@163.com



## 初步配置使用教程

### 配置git

1、 安装git

- 如果是Linux，默认已经安装了git；

- 如果是Windows则需要进行git工具安装，在git官网下载并安装git：<https://git-scm.com/downloads>，安装完成，桌面会有git bash快捷图标。

2、为了可以跟GitHub远程服务器上的仓库连接，首先需要创建本地ssh key：

```
ssh-keygen -t rsa -C "xueyuankui.good@163.com"
```

后面的`your_email@youremail.com`改为你在github上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在`~/`下生成`.ssh`文件夹，进去，打开`id_rsa.pub`，复制里面的`key`。

回到github上，进入 Account Settings（账户配置），在页面左边选择SSH Keys，点击Add SSH Key按钮，title随便填，粘贴在你电脑上生成的key。

在Linux下生成的秘钥路径为：`/home/alex/.ssh/id_rsa.pub`。

创建了ssh key后，就能够拉取私有仓库到本地了。

3、为了验证是否成功，在git bash下输入：

```
$ ssh -T git@github.com
```

如果是第一次的会提示是否continue，输入yes就会看到：You've successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。

4、设置username和email

```
$ git config --global user.name "your name"
$ git config --global user.email "your_email@youremail.com"
```

 再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置。 

### GitHub新建仓库

建立好仓库会有如下提示：

```
…or create a new repository on the command line
echo "# alex_tutorials" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
git push -u origin master

…or push an existing repository from the command line
git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
git push -u origin master

…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.
```

### 本地代码上传至GitHub

新建好GitHub仓库后，可以根据提示进行代码上传，上传的方式主要有如下几种。

#### 方式1：远程仓库没有任何文件

首先创建一个与GitHub仓库同样名字的文件夹，例如alex_tutorials，打开git bash进入该文件夹后，根据如下命令进行操作

```
echo "# alex_tutorials" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
git push -u origin master
```

然后在本地仓库文件夹放入新的代码文件后，执行如下操作：

```
git add *
git commit -m "first add files and code"
git push origin master
```

特别注意：为了保证仓库项目的大小不至于太大，尽量只放代码，不要放较大的压缩包、参数文件等。

#### 方式3：远程仓库有文件

如果远程仓库有README.md之类的文件，根据如下步骤执行：

首先创建一个与GitHub仓库同样名字的文件夹，例如alex_tutorials，打开git bash进入该文件夹后，根据如下命令进行操作

```
git init
git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
git pull origin master
```

然后在添加本地Untracked的文件，执行如下操作：

```
git add *
git commit -m "first add files and code"
git push origin master
```

#### 方式3：上传本地现有仓库

打开git bash，进入仓库文件夹后，输入如下命令：

```
git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
git push -u origin master
```

### 多个git管理

#### ssh-keygen

如果一台计算机需要关联多个github账号，那么需要生成多个ssh key，在使用命令key生成命令时，你需要重新输入一个文件(最好保存在默认目录下**/c/Users/admin/.ssh/**)用于保存新产生的key，不然会覆盖以前github账号对应的key：

```
$ ssh-keygen -t rsa -C "boolpi@163.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/admin/.ssh/id_rsa): /c/Users/admin/.ssh/boolpi-id_rsa
```

在GitHub添加SSH keys，并将`id_rsa_boolpi.pub`内的key拷贝进来。

#### 将秘钥交给agent管理

```bash
$ssh-agent bash
$ssh-add ~/.ssh/id_rsa_github
```

#### config

在.ssh文件夹下，新建config文件，修改如下

```
# personal
Host github.com
  HostName github.com
  User AlexHAHA
  IdentityFile ~/.ssh/id_rsa

# team
# 这里boolpi会在git remote add使用到，用来指定远程仓库对应的id_rsa
Host boolpi
  HostName github.com
  User BoolPi
  IdentityFile ~/.ssh/id_rsa_boolpi
```

#### git clone

拷贝远程仓库

```
git clone git@github.com: BoolPi/learngit.git
```

进入仓库路径，并设置局部的用户名和邮箱

```
$ git config user.name "BoolPi"
$ git config user.email "boolpi@163.com"
```

#### git remote

在仓库项目文件夹，查看远程仓库配置

```
$ git remote -v
origin  git@github.com:BoolPi/yolopi.git (fetch)
origin  git@github.com:BoolPi/yolopi.git (push)

```

默认情况下使用的是`github.com`对应的github账号，通过`config`文件可以知道这个是一个personal账号，如果该仓库属于team，那么你需要重新添加一个remote并对应至team账号

```
$ git remote add origin2 git@boolpi:boolpi/yolopi.git
```

然后你就可以正常使用了！

### 更新下载所有子仓库

进入仓库根目录进行

```shell
$ git submodule sync --recursive
$ git submodule update --init --recursive
```



## GitHub加速

### 码云

将github里的仓库拉到码云中去，然后在码云下载，速度就很快。对于一般的项目而言，这样就足够了，但是对于有很多子模块的项目而言，由于子模块链接的地址皆指向github，因此，对于git submodule update --init --recursive而言仍是龟速，因此，接下来就是方法2

### 国内镜像

通过修改github仓库的地址，使用国内镜像下载。

- cnpmjs.org

  修改仓库地址，增加cnpmjs.org后缀，例如https://github.com/pytorch/pytorch，改为https://github.com.cnpmjs.org/pytorch/pytorch

- https://github.91chi.fun/

  修改仓库地址，增加https://github.91chi.fun/前缀，例如https://github.com/pytorch/pytorch，改为https://github.91chi.fun/https://github.com/pytorch/pytorch

### 更新仓库子模块

对于子模块，可以先不要在git clone的时候加上--recursive，等主体部分下载完之后，该文件夹中有个隐藏文件称为：.gitmodules，把子项目中的url地址同样加上.cnpmjs.org后缀，然后利用`git submodule sync`更新子项目对应的url，最后再`git submodule update --init --recursive`，即可正常网速clone完所有子项目。

另外最简单的方式是通过`git config url`进行全局地址更换，就不需要手动更改.gitmodules文件了。

```shell
git config --global url."https://github.91chi.fun/https://github.com/".insteadOf https://github.com/
```



## 换行符转换

Windows下换行为CRLF，而Linux下换行为LF。为了避免同个仓库在不同系统性协同开发遇到换行符不一致被频繁更改的问题，可以通过设置`core.autocrlf`参数来解决。该参数可设置为false、input、true三种。

- 不转换

```shell
$ git config --global core.autocrlf false
```

- Git 在提交时把CRLF转换成LF，拉取和签出时不转换

```shell
$ git config --global core.autocrlf input 
```

- Git在提交时自动地把行结束符CRLF转换成LF，而在拉取和签出代码时把LF转换成CRLF

```shell
$ git config --global core.autocrlf true 
```



### 统一为Linux标准

以Linux下开发为主，本地和远程仓库的换行都保持以LF结尾。

- Linux下，设置为false
- Windows下，设置为input

## git机制

### 教程参考

官网教程比较重要的一章： https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository 



### 工作流

 Git 有三种状态，你的文件可能处于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。 

你的本地仓库由 git 维护的三棵"树"组成：

- 第一个是你的 `working directory 工作目录`，它持有实际文件；
- 第二个是 `staging area 暂存区`，它像个缓存区域，临时保存你要改动的文件名列表；
- 第三个是`.git directory(Repository)`。

我们使用`git add`命令，将文件添加至缓存区域，使用`git commit -m`实际提交改动，也即是改动已经提交到了HEAD，只有使用commit后，才会有快照保存并移动HEAD指向最新的快照，最后使用`git push`命令将改动推送至远端仓库。

<img src="https://gitee.com/bpnotes/pic-museum/raw/master/pictures/areas.png" style="zoom:60%"/>

基本的 Git 工作流程如下：

1. 在工作目录中修改文件。
2. 暂存文件，将文件的快照放入暂存区域。
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。
4. 推送至服务器，将本地的与远程的仓库协调一致。

### 文件状态

 工作目录下的每一个文件都不外乎这两种状态：已跟踪(tracked，包括 unmodified, modified, or staged )或未跟踪(untracked)。 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能是未修改，已修改或已放入暂存区。 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有被放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。 

<img src="https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210908144812389.png" alt="image-20210908144812389" style="zoom:80%;" />

### 添加文件

仓库文件夹新增了文件后的状态是Untracked，可以使用`git add <file>`命令添加：

```
#添加文件
git add README.md
#添加所有文件
git add *   
#添加文件夹
git add folder
```

如果添加了文件后希望撤销这步操作，可以使用`git restore --staged <file>`

```
#撤销所有staged的文件
git restore --staged *
```

> 注意：
>
> git的机制决定了无法**add**一个空的文件夹，只有空文件夹存在文件时才能**add**.

### 移除文件

1、从仓库stage area中移除被手工删除的文件

如果用户把仓库文件夹的文件手动删除了，那么可以通过`git rm file_name`将文件从stage area移除。

```python
#1仓库文件夹中新建文件git_test.txt
#2添加文件至stage area
$ git add git_test.txt
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   git_test.txt
#3手工移除文件
$ rm git_test.txt
$ git status
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    git_test.txt
#4从stage area中移除
$ git rm git_test.txt
rm 'git_test.txt'
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

```

2、从stage area中移除文件并且将文件从磁盘删除

使用`git rm -f file_name`，不仅可以将文件移除而且文件会从仓库文件夹删除

```python
#1仓库文件夹中新建文件git_test.txt
#2添加文件至stage area
$ git add git_test.txt
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   git_test.txt
#3移除文件的同时删除该文件
$ git rm -f git_test.txt
rm 'git_test.txt'
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

```

2、从stage area中移除文件但将文件保留在磁盘中

使用`git rm -f file_name`，只是将文件从stage area移除，但文件还在仓库文件夹中

```python
#1仓库文件夹中新建文件git_test.txt
#2添加文件至stage area
$ git add git_test.txt
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   git_test.txt
#3将文件从stage area移除，但文件还在仓库文件夹中
$ git rm --cached git_test.txt
rm 'git_test.txt'

$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        git_test.txt
nothing added to commit but untracked files present (use "git add" to track)

```

### 移动文件

移动文件其实就是将文件重命名、移除文件、添加文件三个步骤的集合。我们可以使用`git mv file_before file_after`命令来实现。

```
$ git mv README.md README
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

以上操作其实就相当于如下三个步骤：

```
$ mv README.md README
$ git rm README.md
$ git add README
```

文件夹重命名的操作是同样的，请注意，git不跟踪空文件夹，如果新增的文件夹内不包含任何文件，那么该文件夹无法在git status中显示出来，而且你可以随意手动删掉空文件夹。

### 远程仓库

#### 添加远程仓库

使用`git remote add <short name> <url>`可以添加远程仓库，并且可以使用一个short name来命名远程仓库，一般默认使用origin作为第一个远程仓库的简称。

```shell
$ git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote add git@gitee.com:jizhi_group/jizhi_smartproject.git
```

> 注意：
>
> 添加远程仓库时可以使用https或者ssh两种方式，但如果使用https，每次拉取和推送都要输入密码，推荐使用ssh方式。

#### 查看远程仓库

使用`git remote`命令可以查看当前已经添加的远程仓库名字。

```
$ git remote
origin
```

使用`git remote -v`命令可以查看远程仓库的具体信息

```
Administrator@8F5GS3NCZXCQ2IF MINGW64 /d/deeplearning/alex_notes (master)
$ git remote -v
origin  git@github.com:AlexHAHA/alex_notes.git (fetch)
origin  git@github.com:AlexHAHA/alex_notes.git (push)
```

如果想要查看某一个远程仓库的更多信息，可以使用 `git remote show [remote-name]` 命令。 如果想以一个特定的缩写名运行这个命令，例如 `origin`，会得到像下面类似的信息：

```
$ git remote show origin
* remote origin
  Fetch URL: git@github.com:AlexHAHA/alex_notes.git
  Push  URL: git@github.com:AlexHAHA/alex_notes.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

#### 从远程仓库中抓取与拉取

从远程仓库中抓取使用`git fetch [remote-shortname]`，这个命令只会下载数据到本地仓库，但不会进行代码合并的操作，你必须自己手动进行合并。

我们可以使用拉取操作`git pull [remote-shortname]`，该命令会自动使用`git fetch`命令抓取数据，并合并到当前分支中。

```
$git pull origin master
```



#### 远程仓库的移除与重命名

如果想要重命名引用的名字可以运行 `git remote rename` 去修改一个远程仓库的简写名。 例如，想要将 `pb` 重命名为 `paul`，可以用 `git remote rename` 这样做：

```console
$ git remote rename pb paul
$ git remote
origin
paul
```

值得注意的是这同样也会修改你的远程分支名字。 那些过去引用 `pb/master` 的现在会引用 `paul/master`。

如果因为一些原因想要移除一个远程仓库——你已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了——可以使用 `git remote rm` ：

```console
$ git remote rm paul
$ git remote
origin
```

如果在远程仓库调整了代码位置，例如将某个仓库移到另一个group下，则在推送时会提示修改远程仓库地址，则通过如下命令修改即可：

```shell
$ git remote set-url origin git@git.cetcs.com:cetcs/project-29base/code-onboard.git
```



### 分支

#### 创建分支

使用`git branch [branch_name]`新建分支，并使用`git checkout [branch_name]`切换至分支。

```
$ git branch v1.0
$ git checkout v1.0
Switched to branch 'v1.0'
#或者使用如下更简单的命令代替
# git checkout -b v1.0
```

#### 推送分支至远程服务器

首先切换至分支，然后` git push origin [branch name]`，即可推送。

```
$ git checkout v1.0
Switched to branch 'v1.0'
$ git push origin v1.0
```

特别注意，要推送至origin的分支名字必须与当前的分支名字一样，如果名字不一样，git不会在远程服务器端新建分支，而且会报错。

1、一般在master分支下新建分支

2、新建分支，仅仅是“新建一个指向当前HEAD的指针”，不论你修改了那些文件，只要没有commit，新建的branch就与当前master一样。

#### 从远程仓库拉取新的分支

```
git branch -a          //查看本地是否具有dev分支，这一步其实意义不大
git fetch origin dev   //把远程分支拉到本地
git checkout -b dev origin/dev   //在本地创建分支dev并切换到该分支
git pull origin dev              //把某个分支的内容拉到本地
```

> 注意：
>
> 1、关于是否把master内容同步至分支的问题
>
> 这个根据开发功能需求来定，如果分支上的开发功能不需要依赖master最新的更改就可以不用合并，如果需要那么就执行合并操作。
>
> 将master同步到分支的流程如下：
>
> - 在分支中每次推送至远程仓库之前切换至master分支然后执行拉取，使master是最新的；
> - 每次执行master拉取后，再切换回分支中执行`git merge master`使master中的修改同步至分支。

#### 删除分支

删除本地分支

```shell
$ git branch -d [branchname]
```

删除远程仓库分支

```shell
$ git push origin --delete [branchname]
```



### 合并

使用git merge用于将 **合并指定分支到当前自己的分支** 。

例如你需要将分支master合并入分支alex，那么你需要先切换至分支alex，然后使用命令**git merge master**，进行合并操作。

```
admin@LAPTOP-K5482OIO MINGW64 ~/alex_gitrepos/git_test (alex)
$ git merge master
Auto-merging alex.py
CONFLICT (content): Merge conflict in alex.py
Automatic merge failed; fix conflicts and then commit the result.

```

如果有合并冲突，放弃此次合并，则可以使用**git merge --abort**命令：

```
admin@LAPTOP-K5482OIO MINGW64 ~/Desktop/git_test (master|MERGING)
$ git merge --abort
```

如果合并有冲突，并且希望继续进行合并，那么可以使用vs code查看冲突位置进行手动修改。

修改后查看状态如下：

```
admin@LAPTOP-K5482OIO MINGW64 ~/alex_gitrepos/git_test (alex|MERGING)
$ git status
On branch alex
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Changes to be committed:
        new file:   master.py

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   alex.py

```

然后将手动merge的文件添加后，进行提交：

```
admin@LAPTOP-K5482OIO MINGW64 ~/alex_gitrepos/git_test (alex)
$ git add alex.py
admin@LAPTOP-K5482OIO MINGW64 ~/alex_gitrepos/git_test (alex|MERGING)
$ git commit -m "merge master to alex"
[alex d0ea88a] merge master to alex
```

#### 压缩合并

如果在某个分支上开发完成，且开发过程中提交了很多无太多意义的commit，在将该分支合并入maser分支，可以使用压缩合并，如下：

```shell
$ git checkout master
$ git merge --squash <branch-name>
```

### 标签

#### 创建Tag

```shell
# 基于当前状态创建Tag
$ git tag -a v1.4 -m "my version 1.4"
# 从某次commitID创建Tag，先git log查看commitID
$ git tag -a v1.2 <commitID>
```

#### 推送Tag至远程服务器

```shell
# 推送一个Tag
$ git push origin v1.5
# 推送本地所有Tag
$ git push origin --tags
```

#### 检出Tag

```shell
$ git checkout -b <branchName> <tagName>
```

#### 查看Tag

查看本地所有tag：

```shell
$ git tag
```

查看远程所有 tag：

```shell
$ git ls-remote --tags origin
```

#### 删除Tag

```shell
$ git tag -d <tagName>
```



### 修改撤销

使用`git checkout -- <file>`有如下功能：

- 修改了文件但还没有暂存，执行`checkout --`后文件恢复到没有修改前的状态；
- 修改了文件并进行了暂存，执行`checkout --`后文件退出暂存区，但仍保持了修改；
- 删除了文件，需要恢复，也可以执行`checkout --`命令。

举例：

```shell
$ git checkout -- my_file.py  #撤销文件的修改
$ git checkout -- my_src/     #撤销文件夹的修改
```

还可以使用`git restore <file>`命令撤销修改。

如果使用vscode编辑器，可以看到修改文件之后使用discard操作可以进行修改的撤销。

### commit撤销

#### 撤销方式一：git reset

git log  查看后退对应版本号
git reset --hard 【版本号】
如果需要远程推送的话 git push  --forced

- 查看以前的版本号

```
admin@LAPTOP-K5482OIO MINGW64 ~/alex_gitrepos/git_test (alex)
$ git log
commit d0ea88ad5946072daec7514025221f5177225024 (HEAD -> alex)
Merge: a1654f7 477da7b
Author: AlexHAHA <xueyuankui.good@163.com>
Date:   Sat Mar 28 16:34:39 2020 +0800

    merge master to alex

commit a1654f73eb3f520ecd7637d925d923e31d2ab6b6
Author: AlexHAHA <xueyuankui.good@163.com>
Date:   Sat Mar 28 16:27:21 2020 +0800

    change add fun, delete b
```

- 回退版本

```
admin@LAPTOP-K5482OIO MINGW64 ~/alex_gitrepos/git_test (alex)
$ git reset --hard a1654f7
HEAD is now at a1654f7 change add fun, delete b
```

#### 撤销方式二：git revert

 当 merge 以后还有别的操作和改动时，用 git revert 也能撤销 merge。

```
$ git revert -m 【要撤销的那条merge线的编号，从1开始计算（怎么看哪条线是几啊？）】 【merge前的版本号】
```

### 忽略配置.gitignore

#### 忽略规则

```bash
# 注释
*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/TODO     # 仅仅忽略项目根目录下的 TODO 文件夹，不包括 subdir/TODO

build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt,但不包括 doc/server/arch.tx
```

#### 删除需要将本地缓存

```shell
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```



#### 忽略仓库内的文件

如果文件已经在仓库中了，通过修改.gitignore文件是不起作用的，这个时候需要将文件从仓库中删除（即不在进行git托管）。

```shell
$ git rm --cached <file_name>
$ git commit -m "将文件添加至ignore"
```

然后再修改.gitignore文件添加对文件的忽略即可。

> 注意:
>
> 1. 一定要加--cached参数，否则会从硬盘上删除工程文件

## 问题与解决

### 推送错误1：两个人修改同一个分支

如果有两个人同时fetch了一个分支，第一个人修改后提交、推送，第二个人修改后无法推送，这时出现如下错误：

```
error: failed to push some refs to 'git@github.com:AlexHAHA/alex_notes.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

解决方法：

先 git fetch origin 然后git merge origin/master, 和本地分支合并, 之后再push。

### 无法连接

出现错误：git@github.com: Permission denied (publickey).

解决方法：

1. 重新生成key，名称可以不是默认的，代表账号信息，如id_rsa_cetcs_jizhi，注意一定输入全路径+名称！

```shell
$ ssh-keygen -t rsa -C "cetcs_jizhi_group@163.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/alex/.ssh/id_rsa): /c/Users/alex/.ssh/id_rsa_cetcs_jizhi
```

2. 添加key

```shell
$ ssh-agent bash
$ ssh-add ~/.ssh/id_rsa_cetcs_jizhi
Identity added: /c/Users/alex/.ssh/id_rsa_cetcs_jizhi (cetcs_jizhi_group@163.com)
alex@alex-xiaomi MINGW64 /d/Ubuntu/机载智能应用/assitant_landing (master)
$ ssh -T git@gitee.com
Hi 机智小组! You've successfully authenticated, but GITEE.COM does not provide shell access.
```

3. 服务端设置ssh

登录gitee，新增一个ssh key，将**.ssh/id_rsa_cetcs_jizhi.pub**粘贴。







