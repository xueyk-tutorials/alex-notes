

# git命令

## 提交(commit)



### 提交类型

- `feat` 新功能
   A new feature

- `fix` 修复 bug
   A bug fix

- `docs` 仅包含文档的修改
   Documentation only changes

- `style`  格式化变动，不影响代码逻辑。比如删除多余的空白，删除分号等
   Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)

- `refactor` 重构，既不是新增功能，也不是修改 bug 的代码变动
   A code change that neither fixes a bug nor adds a feature

- `perf` 性能优化
   A code change that improves performance

- `test` 增加测试
   Adding missing tests or correcting existing tests

- `build` 构建工具或外部依赖包的修改，比如更新依赖包的版本等
   Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)

- `ci` 持续集成的配置文件或脚本的修改
   Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)

- `chore` 杂项，其他不修改源代码与测试代码的修改
   Other changes that don't modify src or test files

- `revert` 撤销某次提交
   Reverts a previous commit

## (amend)

使用amend可以覆盖上一次的提交，而不会产生新的提交信息。

```shell
git commit --amend  --no-edit
```



本地修改后如果要推到远端，git push时一定要使用 --force-with-lease

```shell
git push --force-with-lease origin master
```



## 撤销(reset)

撤销的命令为`git reset`，

1. git reset --soft head^ 软撤销，head^是指当前commit，可以改成任意commit id
2. git reset --hard head^ 硬撤销，彻底丢掉这次提交的全部修改
3. git reset 不加参数，本次修改就会回到`add`之前的状态

### 软撤销

软撤销不会删除本地的代码更改，仅仅撤销掉commit。

查看当前log

```shell
$ git log
commit b8c13b43d9a9b9d2807a90e81a76ad957da5a0ab (HEAD -> master, origin/master)
Author: 无烬 <xueyuankui.good@163.com>
Date:   Thu Sep 23 08:44:05 2021 +0800

    add user frame functions

commit c872bae17d013c7c7939ae58ce816b0c6a978d68 (tag: V0.1)
Author: 无烬 <xueyuankui.good@163.com>
Date:   Thu Sep 9 09:56:33 2021 +0800

    init
```

撤销

```shell
$ git reset --soft c872bae17d01

alex@alex-xiaomi MINGW64 /e/无烬/教程/px4_onboard_app_dev_tutorial/offboardSDK/git_repo (master)
$ git status
On branch master
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   OffboardSDK-0.2-py3-none-any.whl
        modified:   README.md
        modified:   demo.py

```



### 恢复撤销

如果执行了`git reset --hard <commitID>`操作，如何恢复呢？我们可以通过再次reset至之前的commitID即可。

通过`git reflog`获取操作日志

```shell
$ git reflog
293cb7a (HEAD -> followme, master) HEAD@{0}: reset: moving to 293cb7aca
fd9c1c1 HEAD@{1}: merge master: Merge made by the 'recursive' strategy.
ffac921 HEAD@{2}: checkout: moving from master to followme
```

然后再次执行reset回到一个撤销之前的commit：

```shell
$ git reset --hard ffac921
```

## 储藏(stash)

### 应用场景

- 当正在dev分支上开发某个项目，这时项目中出现一个bug，需要紧急修复，但是正在开发的内容只是完成一半，还不想提交，这时可以用git  stash命令将修改的内容保存至堆栈区，然后顺利切换到hotfix分支进行bug修复，修复完成后，再次切回到dev分支，从堆栈中恢复刚刚保存的内容。
- 由于疏忽，本应该在dev分支开发的内容，却在master上进行了开发，需要重新切回到dev分支上进行开发，可以用git stash将内容保存至堆栈中，切回到dev分支后，再次恢复内容即可。

总的来说，git  stash命令的作用就是将目前还不想提交的但是已经修改的内容进行保存至堆栈中，后续可以在某个分支上恢复出堆栈中的内容。这也就是说，stash中的内容不仅仅可以恢复到原先开发的分支，也可以恢复到其他任意指定的分支上。git stash作用的范围包括工作区和暂存区中的内容，也就是说没有提交的内容都会保存至堆栈中。

```shell
$ git stash                         # 能够将所有未提交的修改（工作区和暂存区）保存至堆栈中，用于后续恢复
$ git stash save ["stash log"]      # 同`git stash`，只不过是可以添加备注
$ git stash pop                     # 当前stash中的内容弹出，并应用到当前分支对应的工作目录上
$ git stash list                    # 查看当前的储藏
```

## 标签(tag)

### 创建tag

```shell
$ git tag -a v0.1 -m 'release 0.1'
```

推送tag至远程仓库

```shell
$ git push origin <tagName>
#如
$ git push origin v0.1
```

### 检出标签

```shell
$ git checkout tags/v1.1.0
```



### 删除

本地 tag 的删除：

```shell
git tag -d <tagName>
```

远程 tag 的删除：

```shell
git push origin :refs/tags/<tagName>
```

## 变基(rebase)

在开发过程中，不要在main分支上进行rebase操作，使用merge对其余开发分支的代码进行合并；

在开发分支使用rebase操作，将main分支代码合并进来；



### VScode的git插件操作

current branch：当前的工作分支（打勾的分支），代码编辑只对当前分支有效。

![image-20230213104614150](imgs\image-20230213104614150.png)

#### Rebase Current Branch onto Branch

说明：将当前分支基于分支（鼠标右键所选择的分支）进行变基。完成该操作后，选中的分支的commit会添加到当前当前分支中，并且添加在当前分支的commit之前。

应用场景：

1）如果在dev1上正在进行开发（当前分支），这时候需要用到main的最新代码希望将main合并进来；







## 创建发行版( Release)

发行版具有"**超出 Git 架构本身**" 的意义与作用，由于git 本身只能记录项目修改，本质上不适合将编译好的项目二进制文件记录下来。通过 release ，开发者可以把发布版本时项目所编译好的二进制文件保存了下来，方便用户下载，也方便查找特定版本的二进制文件。

创建发行版的界面如下，可以选择一个版本、进行上传文件等。

![image-20210923094700726](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210923094700726.png)

例如我们创建了一个发行版并且上传了该版本相应的二进制文件、说明文件等，发现版也包含了版本对应的程序源码。

![image-20210923094200018](https://gitee.com/bpnotes/pic-museum/raw/master/pictures/image-20210923094200018.png)



## 命令详解

### git init

将文件夹创建为仓库，也就代表后续将使用git对该文件进行项目版本管理，例如进入文件夹alex_tutorials后，输入git init命令：

```
Administrator@8F5GS3NCZXCQ2IF MINGW64 /d/deeplearning/alex_tutorials
$ git init
Initialized empty Git repository in D:/deeplearning/alex_tutorials/.git/
```

这是仓库内会自动创建一个**.git**文件夹，每个仓库文件夹都会有**.git**文件夹，如果删除该文件夹那么这个仓库文件夹就变成了一个普通文件夹。

### git clone 

将仓库保持至本地，并且会自动新建一个与仓库名字一样的本地文件夹，并将仓库内容复制到该文件夹中。

```
$ git clone https://github.com/AlexHAHA/alex_tutorials.git
或者使用
$ git clone git@github.com:AlexHAHA/alex_tutorials.git
```

如果你希望clone下的仓库文件夹名字不使用GitHub上的仓库名，可以在使用clone命令的最后提供一个参数做为本地仓库文件夹的名字，例如：

```
$ git clone https://github.com/AlexHAHA/alex_tutorials.git  my_tutorials
```

### git submodule

```
git submodule update --init --recursive
```

### git status

 可以用 `git status` 命令查看哪些文件处于什么状态。 如果修改了文件之后使用该命令：

```
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   github/github.md

no changes added to commit (use "git add" and/or "git commit -a")

```

### git add

 这是个多功能命令：可以用它开始跟踪（track）新文件，或者把已跟踪的（tracked）文件放到暂存区（stage area），还能用于合并时把有冲突的文件标记为已解决状态等。将这个命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。 

 `git add` 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。 

```python
git add source #source是文件夹，将其中的所有文件添加
git add *.c    #只添加后缀.c的文件

```



### git remote add 

将本地仓库和远程仓库联系起来，例如下命令：

```
git remote add origin git@github.com:AlexHAHA/alex_tutorials.git
```

将远程仓库**git@github.com:AlexHAHA/alex_tutorials.git**与本地仓库联系起来，并且把远程仓库简称为origin，当然也可以取其他名字，不过github者们默认的是origin。在其他命令中，origin就代表了这个与本地仓库关联的远程仓库。

### git remote remove origin

删除已经添加的远程仓库。

### git diff

 你可能通常会用它来回答这两个问题：当前做的哪些更新还没有暂存？ 有哪些更新已经暂存起来准备好下次提交？ 虽然 `git status` 已经通过在相应栏下列出文件名的方式回答了这个问题，但 `git diff` 将通过文件补丁的格式更加具体地显示哪些行发生了改变。 

 1、要看尚未暂存的文件更新了哪些部分，不加参数直接输入 `git diff`： 

```
git diff
```

此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。 也就是修改之后还没有暂存起来的变化内容。 

2、若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --staged` 命令。 这条命令将比对已暂存文件与最后一次提交的文件差异： 

```
git diff --staged
```

3、查看已经暂存起来的变化使用命令 `git diff --cached`  ：

```
git diff --cached
```

这个命令将会显示使用`git add`之前，添加至stage area文件的修改内容。

### git rm

参照"git机制->移除文件"章节。

### git mv

参照"git机制->移动文件"章节。

### git commit

将暂存区（stage area）的文件添提交至新的版本中，注意这里commit的文件版本是使用`git add`命令添加至暂存区的文件版本，一旦修改了某个文件，你需要重新使用`git add`命令将其添加至暂存区作为新的更改后的版本，commit后才能在新的版本中看到这个修改。

1、直接使用命令`git commit` ，将会弹出一个类似vim的文本编辑入口，可以输入提交时的说明信息。

2、使用`-m`选项，该命令接收一个字符串作为版本提交说明。

```
git commit -m "add source folder"
```

3、使用`-a`选项，尽管使用暂存区域的方式可以精心准备要提交的细节，但有时候这么做略显繁琐。 Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 `git commit` 加上 `-a` 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤，例如：

```
$ git commit -a -m "temp changes"
```

### git pull

**git pull** 命令用于从远程获取代码并合并本地的版本。 

**git pull** 其实就是 git fetch 和 git merge FETCH_HEAD 的简写。 命令格式如下：

```shell
$ git pull <远程主机名> <远程分支名>:<本地分支名>
```



撤销pull操作

1、使用`git reflog`命令查看你的历史变更记录；

```shell
5f40fdc HEAD@{7}: checkout: moving from master to newpbft
40a9a83 HEAD@{8}: clone: from https://github.com/yeongchingtarn/geth-pbft.git
```

2、使用`git reset --hard HEAD@{n}`命令回退；

```shell
git reset --hard 40a9a83
```



### git push

第一次推送master分支时，加上了-u参数，如下：

```
git push -u origin master
```

Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令，只需要使用`git push origin master`即可。

### git log

查看版本提交记录，输入`q`后退出信息显示入口，按方向键上和下能够上下浏览。

关于命令 `git log` 的常用选项 如下：

| 选项              | 说明                                                         |
| :---------------- | :----------------------------------------------------------- |
| `-p`              | 按补丁格式显示每个提交引入的差异。                           |
| `--stat`          | 显示每次提交的文件修改统计信息。                             |
| `--shortstat`     | 只显示 --stat 中最后的行数修改添加移除统计。                 |
| `--name-only`     | 仅在提交信息后显示已修改的文件清单。                         |
| `--name-status`   | 显示新增、修改、删除的文件清单。                             |
| `--abbrev-commit` | 仅显示 SHA-1 校验和所有 40 个字符中的前几个字符。            |
| `--relative-date` | 使用较短的相对时间而不是完整格式显示日期（比如，“2 weeks ago”）。 |
| `--graph`         | 在日志旁以 ASCII 图形显示分支与合并历史。                    |
| `--pretty`        | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（用来定义自己的格式）。 |

 很多时候信息太多不好查看，那么可以通过一些条件限制 `git log` 输出的选项，常用命令如下： 

| 选项                  | 说明                                       |
| :-------------------- | :----------------------------------------- |
| `-(n)`                | 仅显示最近的 n 条提交。                    |
| `--since`, `--after`  | 仅显示指定时间之后的提交。                 |
| `--until`, `--before` | 仅显示指定时间之前的提交。                 |
| `--author`            | 仅显示作者匹配指定字符串的提交。           |
| `--committer`         | 仅显示提交者匹配指定字符串的提交。         |
| `--grep`              | 仅显示提交说明中包含指定字符串的提交。     |
| `-S`                  | 仅显示添加或删除内容匹配指定字符串的提交。 |







