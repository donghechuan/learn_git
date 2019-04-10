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
查看文件修改内容信息
git diff filename

提交修改与提交新文件是一样的两步
git add filename
git commit -m “文件描述”

