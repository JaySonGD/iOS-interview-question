git init 初始化一个仓库

git add 添加一个文件到仓库里面

git commit -m "commit a file"   提交文件

git status 查看文件的状态

git diff  查看文件修改的不同

 git log 查看提交的日志  //只能查看3次的日志
 
##版本回退

用`HEAD`表示当前版本，上一个版本是`HEAD^` ，上上一个版本是`HEAD^^`

git reset --hard HEAD^   //回退到上一个版本

git reflog  //记录使用命令

##工作区和版本库

git add是把工作区的文件添加到版本库的暂存区

git commit再把暂存区的文件提交到分支

创建版本库的时候，git为我们自动创建了一个master分区

Untracked files:未提交的文件，新的

##工作区和版本库最新版本区别

##撤销修改
git checkout -- file      `--`很重要表示撤回,丢弃工作区的修改 没有的话则表示创建一个新的分支

git reset HEAD file  可以把暂存区的修改撤销掉（unstage），重新放回工作区：:

##删除
git rm		从版本库里面删除

##切换分支
git checkout命令加上-b参数表示创建并切换，

git branch命令会列出所有分支，当前分支前面会标一个*号

git merge命令用于合并指定分支到当前分支

git log --graph --pretty=oneline --abbrev-commit 查看分支合并情况

git branch -d 删除分支

git stash 存储当前现场

git stash list 查看当前的现场

git stash apply恢复现场但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了：