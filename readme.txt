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

