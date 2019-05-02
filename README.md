

[Git 学习笔记](#Git 学习笔记)

[在 Linux 上搭建 Git 服务器](#在 Linux 上搭建 Git 服务器)

## Git 学习笔记

Git command：

1.配置用户名和邮箱地址

```
git config --global user.name "caocb.sdc"
git config --global user.email "1571616014@qq.com"
```
2.显示所有配置

```
git config --list
```
3.创建文件：

```
cd /d
mkdir learngit
cd learngit
pwd		//用于显示当前目录
```
4.创建Git仓库（初始化目录）
```
git init	//此时多了一个隐藏的.git目录
```
5.把文件添加到仓库（从工作区——>暂存区）
```
git add readme.txt
```
6.把文件提交到仓库（从暂存区——>最终版本库）
```
git commit -m "wrote a readme.txt file"		//-m后面输入的是本次提交的说明
```
7.查看当前仓库文件的状态（与上次提交版本比较是否有所改动）
```
git status
```
8.查看具体修改了什么内容
```
git diff
```
9.查看历史提交日志
```
git log
```
显示如下：commitID+用户信息+日期+版本说明
```
commit e956221304f2a64dbb4593fdfb031ba907641703 (HEAD -> master)
Author: caocb.sdc <1571616014@qq.com>
Date:   Sat Feb 3 22:22:39 2018 +0800

    readme ver.1

commit 49b8ef963cf38b568d39b7bfd4426a1c8cb96f6f
Author: caocb.sdc <1571616014@qq.com>
Date:   Sat Feb 3 22:11:49 2018 +0800

    wrote a readme.txt file

commit 27c60d6a253cb8204613b0542deeb1bb07668f76
Author: caocb.sdc <1571616014@qq.com>
Date:   Sat Feb 3 22:02:56 2018 +0800

    wrote a readme.txt file
```

10.回退版本
```
git reset --hard HEAD		//取回当前最新提交版本
git reset --hard HEAD^		//回退到上个版本
git reset --hard HEAD^^		//回退到上上个版本
git reset --hard HEAD~100	//往上回退100个版本
```
11.如果已回退上一版本，但想取回最新版本，需指定版本号（commitID）
```
git reset --hard e956221	//版本号前几位就行
```
如果当前窗口已经找不到版本号，可以使用命令 `git reflog`

显示每次提交的版本号+提交命令

12.查看工作区和版本库里面最新版本的区别：

```
git diff HEAD -- readme.txt
```
13.撤销修改

如果修改后还未add到暂存区，使用：

```
git checkout -- readme.txt
```
撤销工作区的全部修改（其实就是拿版本库里的版本替换工作区的版本）

如果已使用add提交到暂存区，未commit，需要把暂存区的修改回退到工作区（清除暂存区文件），使用：

`git reset HEAD file`

14.删除文件

删除工作区的：`rm test.txt`

删除版本库的：`git rm readme.txt  ——> git commit -m "remove readme.txt"`

15.远程仓库：GitHub

第1步：创建SSH key，用于关联远程仓库：

`ssh-keygen -t rsa -C "youremail@example.com"`

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可。

由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里（C:\Users\Administrator\.ssh）找到`.ssh`目录，

里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，

id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：

16.提交到远程库命令：（需要在GitHub上新建一个空目录，不包含README.md）

```
git remote add origin https://github.com/turbobin/learngit.git
git push -u origin master	(-u表示把本地master分支和远程关联起来)
```
此后，只要本地作了提交，就可以通过命令：

`$ git push origin master`

把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

17.克隆一个远程库到本地

使用https协议：

`git clone https://github.com/turbobin/learning_git_from_liao`

使用ssh协议：

`git clone git@github.com/turbobin/learning_git_from_liao`

18.操作远程主机命令：

```
git remote -v						——查看远程主机名与网址
git remote show <主机名> 			——查看该主机的详细信息
git remote add <主机名> <网址>		——添加远程库
git remote rm <主机名>				——删除远程主机
git remote rename <原主机名> <新主机名>	——重命名主机名
```

19.将远程版本库的更新取回本地：

`git fetch origin master` (这里指定master分支，也可以指定其他分支，不会做合并)

比较本地master分支和远程分支差别：

`git log -p master..origin/master`

合并分支：

`git merge origin/master`

以上步骤也可以一步到位：

`git pull origin master`

20.查看远程分支：

```
git branch -r
git branch -a (查看所有分支)
```
21.分支操作：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

禁用fast-forward 模式合并分支，能看出历史合并：

`git merge --no-ff -m "merge with no-ff" dev`

删除分支：`git branch -d <name>`

22.出现冲突时 ，可以使用下面命令查看分支合并图：
```
git log --graph
git log --graph --pretty=oneline --abbrev-commit
```
23.解决bug分支：

先保存工作现场：`git stash`

创建bug分支修复bug，如从主分支上修复bug

```
git checkout master
git checkout -b bug-101
```
之后修改-->提交-->切回master分支-->合并bug分支

切回dev分支，

查看保存的工作现场：

`git stash list`

恢复工作现场:

`git stash apply` (stash内容不删除) --> `git stash drop` (删除stash)

或者使用:`git stash pop`

24.如果要丢弃一个没有被合并过的分支，

可以通过`git branch -D <name>`强行删除

25.从本地推送分支，使用`git push origin branch-name`，

如果推送失败，先用`git pull`抓取远程的新提交；

在本地创建和远程分支对应的分支，

使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch --set-upstream-to branch-name origin/branch-name`；

从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

git在pull或者合并分支的时候有时会遇到提示：Please enter a commit message...

解决方式：

① 按键盘字母i 进入insert模式

② 修改信息

③ 按键盘Esc键，输入":wq",回车即可

26.给分支版本打标签：

切换到需要打标签的分支，输入：

```
git tag v1.0
```
给历史版本打标签：

使用`git log --pretty=oneline --abbrev-commit` 查看历史提交 commit id

再输入：`git tag <标签> <commit id>`

带说明的标签：`git tag -a v0.1 -m "version 0.1 released" 3628164`

查看标签：`git show <标签名>`

`git tag -s <tagname> -m "blablabla..."` 可以用PGP签名标签

27.

命令`git push origin <tagname>`可以推送一个本地标签；

命令`git push origin --tags`可以推送全部未推送过的本地标签；

命令`git tag -d <tagname>`可以删除一个本地标签；

命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

28.设置别名：

```
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
```
等等...

配置文件：

`.git/config`

`user/.gitconfig`

## 在 Linux 上搭建 Git 服务器

1.安装Linux系统（Windows下用虚拟机安装）

以在Windows下为例，使用secureCRT登录到Linux(此处用root用户登录)

检查是否安装git(一般默认都有安装)：

```
[root@localhost srv]# git --version
git version 1.7.1
```
2.创建一个git用户，用来运行git服务

`# sudo adduser git`  (root用户是#开头,其他用户是$开头)

3.选定一个目录作为 Git 仓库，假定是`/srv/sample.git`，在`/srv`目录下输入命令：

`# sudo git init --bare sample.git`

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，

所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。

4.把所属权限变更为git用户:

`# sudo chown -R git:git sample.git`

5.创建证书登录：

在`/home/git`目录下创建`.ssh/authorized_keys`文件

编辑文件：

`vi /home/git/.ssh/authorized_keys`

把Windows下`C:\Users\Administrator\.ssh\id_rsa.pub`里内容复制到`authorized_keys`文件中,

保存退出。

6.现在可以在Windows中操作远程仓库了：

如添加远程仓库：

`$ git remote add linuxgit git@192.168.223.2:/srv/sample.git`

推送远程仓库：

`$ git push -u linuxgit master`

克隆：

`git clone git@192.168.223.2:/srv/sample.git`

7.如果为了安全，要禁用shell登录，可以编辑`/etc/passwd`

`# vi /etc/passwd`

把类似 `git​:x:​501:501::/home/git:/bin/bash `内容改成

`git​:x:​501:501::/home/git:/bin/git-shell`

[wode]: 
[www.baidu.com]: 
[zhihu]: 