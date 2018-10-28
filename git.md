# git安装及配置

![](assets/59c31e4400013bc911720340.png)

Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库

## 1. git安装

Linux环境下：

```shell
zhangtao@odoo:~$ sudo apt-get install -y git
zhangtao@odoo:~$ git config --global user.name bingshan2018
zhangtao@odoo:~$ git config --global user.email 708935977@qq.com
```

Windows环境下：

下载地址：https://www.git-scm.com/download/

按默认设置安装完成后，会产生git bash、git CMD、git GUI，前两个是命令行模式

进入git bash

```bash
$ git config --global user.name "bingshan2018"
$ git config --global user.email "708935977@qq.com"
```

> 注意：git config --global 参数，有了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然你也可以对某个仓库指定的不同的用户名和邮箱。

## 2. git仓库

### 2.1 本地工作区、本地仓库

1. 打开git bash，在目标路径新建文件夹

   ```bash
   e30714050@e30714050-PC MINGW64 ~
   $ cd f:
   
   e30714050@e30714050-PC MINGW64 /f
   $ mkdir test
   
   e30714050@e30714050-PC MINGW64 /f
   $ cd test
   
   e30714050@e30714050-PC MINGW64 /f/test
   $
   ```

2. 通过命令 git init 把这个目录变成git可以管理的仓库。

   ```bash
   e30714050@e30714050-PC MINGW64 /f/test
   $ git init
   Initialized empty Git repository in F:/test/.git/
   ```

   这时候你当前testgit目录下会多了一个.git的目录，这个目录是Git来跟踪管理版本的，没事千万不要手动乱改这个目录里面的文件，否则，会把git仓库给破坏了。

3. 在目标文件夹新建一个文件readme.md

4. 使用命令 git add readme.md添加到暂存区里面去，没有任何提示，说明已经添加成功了

   ```bash
   e30714050@e30714050-PC MINGW64 /f/test (master)
   $ ls
   readme.md
   
   e30714050@e30714050-PC MINGW64 /f/test (master)
   $ git add readme.md
   ```



5. 用命令 git commit告诉Git，把文件提交到本地仓库

   ```bash
   e30714050@e30714050-PC MINGW64 /f/test (master)
   $ git commit -m "首次提交readme.md"
   [master (root-commit) 3f62b8f] 首次提交readme.md
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 readme.md
   ```


6. 通过命令git status来查看是否还有文件未提交说明没有任何文件未提交，但是我现在继续来改下readme.md里面的文件内容，继续使用git status来查看下结果，如下

   ```bash
   e30714050@e30714050-PC MINGW64 /f/test (master)
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)
   
           modified:   readme.md
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ```

7. 其他命令

   修改日志查询：

   ```bash
   git log
   ```

   回退至上一版本：

   ```bash
   git reset --hard HEAD^
   ```

   回退至n版本

   ```bash
   git reset --hard HEAD~n
   ```

   撤销修改：

   - 若上次修改提交至暂存区，会撤销本次修改，与暂存区一致
   - 若上次修改已commit至仓库，会撤销本次修改，与仓库一致

   ```bash
   git checkout -- readme.md
   ```

   删除文件（彻底删除需要commit）：

   ```bash
   rm readme.md
   git commit -m "删除readme.md"
   ```

### 2.2 远程仓库

1. 注册github账号

2. 创建SSH Key

   在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果有的话，直接跳过此如下命令，如果没有的话，打开命令行，输入如下命令：

   ```bash
   ssh-keygen -t rsa –C “youremail@example.com”
   ```

   id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

3. 设置github，导入公钥

   登录github,打开” settings”中的SSH Keys页面，然后点击“Add SSH Key”,填上任意title，在Key文本框里黏贴id_rsa.pub文件的内容，点击 Add Key

4. 创建远程仓库

5. 关联本地仓库和远程github仓库

   我们已经在本地创建了一个Git仓库后，又想在github创建一个Git仓库，并且希望这两个仓库进行远程同步，这样github的仓库可以作为备份，又可以其他人通过该仓库来协作。

   在GitHub上的这个testgit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

   根据GitHub的提示，在本地的test仓库下运行命令：

   ```bash
   git remote add origin https://github.com/bingshan/test.git
   ```

   把本地库的内容推送到远程，使用 git push命令，实际上是把当前分支master推送到远程。

   由于远程库是空的，我们第一次推送master分支时，加上了 –u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。推送成功后，可以立刻在github页面中看到远程库的内容已经和本地一模一样了

   ```bash
   git push -u origin master
   ```



   之后，本地修改推送远程仓库的一般步骤为

   ```
   git add aaa.txt
   git commit -m "本次提交简要描述"
   git push origin master
   ```

6. 克隆远程仓库

   bash中cd到目标路径执行git clone

   ```bash
   git clone https://aaa.git
   ```

## 3. 创建与合并分支

在 版本回退里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。

```bash
查看所有分支：git branch
创建分支：git branch branchname
切换分支：git checkout branchname
创建+切换分支：git checkout –b branchname
合并某分支到当前分支：git merge branchname
删除分支：git branch –d branchname
```

### 3.1 分支管理策略

通常合并分支时，git一般使用”Fast forward”模式，在这种模式下，删除分支后，会丢掉分支信息，现在我们来使用带参数 –no-ff来禁用”Fast forward”模式。

```
git merge –-no-ff -m “本次合并的注释说明” branchname
```

分支策略：首先master主分支应该是非常稳定的，也就是用来发布新版本，一般情况下不允许在上面干活，干活一般情况下在新建的dev分支上干活，干完后，比如上要发布，或者说dev分支代码稳定后可以合并到主分支master上来。

1. bug分支：

   在开发中，会经常碰到bug问题，那么有了bug就需要修复，在Git中，分支是很强大的，每个bug都可以通过一个临时分支来修复，修复完成后，合并分支，然后将临时的分支删除掉。

   比如我在开发中接到一个404 bug时候，我们可以创建一个404分支来修复它，但是，当前的dev分支上的工作还没有提交。

   并不是我不想提交，而是工作进行到一半时候，我们还无法提交，比如我这个分支bug要2天完成，但是我issue-404 bug需要5个小时内完成。

   怎么办呢？还好，Git还提供了一个stash功能，可以把当前工作现场 ”隐藏起来”，等以后恢复现场后继续工作。

   ```bash
   /f/git/test(dev)
   隐藏当前工作现场：git stash
   切换到master分支：git checkout master
   
   /f/git/test(master)
   在master上创建临时分支：git checkout -b issue-solve
   在issue-solve分支上修改文件并测试通过，问题解决
   加载到issue-solve分支的暂存区：git add aaa.txt
   提交到工作区：git commit -m "修复临时问题"
   
   切换到master分支：git checkout master
   合并临时分支到master上：git merge --no-ff -m "修复临时问题" issue-solve
   删除临时分支：git branch -d issue-solve
   
   回到之前的工作现场，继续干活：git checkout dev
   ```

2. 工作现场的管理

   ```bash
   git stash list：查看所有工作现场
   git stash apply：工作现场恢复
   git stash drop：删除工作现场
   git stash pop：恢复工作现场并删除
   ```

## 4. 多人协作

当你从远程库克隆时候，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且远程库的默认名称是origin。

要查看远程库的信息 使用 git remote
要查看远程库的详细信息 使用 git remote –v

### 4.1 推送分支

推送分支就是把该分支上所有本地提交到远程库中，推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```bash
git push origin branchname
```

> master分支是主分支，因此要时刻与远程同步。
> 一些修复bug分支不需要推送到远程去，可以先合并到主分支上，然后把主分支master推送到远程去。

### 4.2 抓取分支

多人协作时，大家都会往master分支上推送各自的修改。现在我们可以模拟另外一个同事，可以在另一台电脑上（注意要把SSH key添加到github上）或者同一台电脑上另外一个目录克隆，新建一个目录名字叫testgit2

首先，可以试图用git push origin branchname推送自己的修改.
如果推送失败，则因为远程分支比你的本地更新早，需要先用git pull试图合并。
如果合并有冲突，则需要解决冲突，并在本地提交。再用git push origin branchname推送。



## 5. git命令清单

## 6. github desktop的使用



