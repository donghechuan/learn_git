查看git当前用户名
git config user.name

查看git当前用户邮箱
git config user.email

修改用户名
git config --global user.name "your name"

修改用户邮箱
git config --global user.email "your email"

创建一个版本库
mkdir learn_git
cd learn_git

通过git init命令把这个目录变成git可以管理的仓库
git init

将文件readme.txt添加到仓库
git add readme.txt

使用git commit 把文件提交到仓库
git commit -m "文件描述"

查看仓库当前状态
git status

比较未暂存的修改版本与分支中最后的版本之间的不同
git diff 

比较暂存区的修改版本与分支中最后的版本之间的不同
git diff --staged

提交修改与提交新文件是一样的两步
git add filename
git commit -m “文件描述”

在windows系统中下使用git时 报如下警告：
$ git add readme.txt
warning: LF will be replaced by CRLF in readme.txt.
The file will have its original line endings in your working directory.

意思大概是：LF（换行，Line Feed）将会被CRLF（回车换行，CarriageReturn）替代。

                     该文件将在工作目录中具有其原始行尾。

                     报这个警告时是由于文件夹远程不存在，但是不影响提交
解决方法：
rm -rf .git  //删除.git
git config --global core.autocrlf false //禁用自动转换

然后重新执行：
git init
git add filename
git commit -m "文件描述"


查看最近到最远的提交日志
git log 

简化信息量使用
git log --pretty=oneline

回退到上一个版本
git reset --hard HEAD^

回退到上上个版本
git reset --hard HEAD^^

回退到往上100个版本
git reset --hard HEAD~100

回到未来的某个版本
git reset --hard 1b33a(这是未来版本的那个commit id号)

查看命令执行记录
git reflog

工作区就是工作目录，如learn_git文件夹是一个工作区

工作区中有一个隐藏的.git 这个是git版本库

版本库中有stage(暂存区)和一个master分支

工作区-> stage -> master

git add 将文件从工作区添加到暂存区
git commit 再将文件从暂存区提交到当前分支


撤销修改

1.文件修改后还没有git add，目前还是在工作区,使用下列的命令可以丢弃工作区的修改
git checkout -- readme.txt

2.文件修改后执行了 git add 已到达stage（暂存区）使用下面命令可以把暂存区的修改撤销掉,然后在执行1的命令就可以把工作区的修改撤销
git reset HEAD readme.txt

删除文件test.txt

第一种情况(将工作区和版本库分支中的文件都删除干净，不能恢复): 
rm test.txt /用rm直接删除或在文件管理器中删除
git rm test.txt
git commit -m "rm test.txt"

第二种情况：（误删除，可以从版本库中恢复到工作区中）
rm test.txt /用rm直接删除或在文件管理器中删除
git checkout -- test.txt

添加远程仓库github并将本地仓库内容推送到远程仓库上

本地git仓库和github仓库之间的传输是通过ssh加密的
第一步 创建ssh key

ssh-keygen -t rsa -C "your email"
第二步 登陆github 打开“Account settings”,"SSH Keys"页面，点击“Add SSH Key”
填上任意title，在Key文本框里粘贴id_rsa.pub文件内容 点击“Add Key”就可以看到添加的key了

第三步 在github创建一个新仓库learn_git

第四步 把已经有的本地仓库与github上的新仓库learn_git关联
git remote add origin git@github.com:donghechuan/learn_git.git  其中origin是git默认的远程库名字

第五步 把本地库所有内容推送到远程库上
git push -u origin master (由于远程库是空的，第一次推送到远程仓库时加参数 -u 以后就不用加-u了)

从远程仓库中克隆到本地仓库
git clone git@github.com:donghechuan/gitskills.git

创建分支dev，并切换到dev分支：
git checkout -b dev

git checkout命令加上-b参数表示创建并切换，相当于下面两条命令
git branch dev   //创建分支dev 
git checkout dev //切换分支到dev上

列出所有分支，当前分支前面有个*
git branch

将dev分支合并到master分支上：
git merge dev

合并后可以删除dev分支
git branch -d dev

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>


解决分支间的冲突
git status 查看合并分支时的不同，手动修改后在提交

查看分支合并情况
git log --graph --pretty=oneline --abbrev-commit

git log退出按Q


分支管理策略
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
git merge --no-ff -m "merge with no-ff" dev


bug 分支

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

git stash list 查看工作现场列表

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除

查看远程库的信息
git remote

查看远程库的详细信息
git remote -v

推送分支到远程库
git push origin 分支名

要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是用这个命令创建本地dev分支：

git checkout -b dev origin/dev


多人协作的工作模式通常是这样：

首先，可以试图用git push origin <branch-name>推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

这就是多人协作的工作模式，一旦熟悉了，就非常简单


标签
git tag <tagname> 用于创建一个新标签，默认为HEAD，也可以指定一个commit id

git tag -a <tagname> -m "指定标签信息"

git tag 可以查看所有标签

命令git push origin <tagname>可以推送一个本地标签；

命令git push origin --tags可以推送全部未推送过的本地标签；

命令git tag -d <tagname>可以删除一个本地标签；

命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

配置别名

只需要敲一行命令，告诉Git，以后st就表示status：

git config --global alias.st status
