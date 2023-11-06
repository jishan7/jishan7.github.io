# Git实用命令分享

```bash
# 添加当前分支上未跟踪的所有改动到commit中
git add .
git commit -m 'your comment'

# 将当前分支的commit推送到指定远程分支
git push origin your_remote_branch

# 推送改动到github评审
# 并不会直接合入到远程master分支，而是先进入到代码评审
git push origin master:refs/for/master

# 直接复用上次的commit，将上次commit中的改动更改重新提交
git add 改出问题.的代码文件
git commit --am
git push origin your_remote_branch

# 如果不需要修改上次commit中的message，仅仅是想变动一下需要推送的文件改动
git add 改出问题.的代码文件
git commit –-amend –-no-edit
git push origin your_remote_branch

# 使用 --depth=1 参数，避免下载历史版本
git clone --depth 1 ssh://your_code.git -b master 

# 把远程的分支合并到本地当前分支
git merge --no-ff origin/1.0

# 把远程新建的分支同步到本地
git fetch && git checkout your_remote_branch

# 撤销commit
git reset --soft HEAD^

# 撤销add
git reset HEAD 
git reset HEAD xx.py

# 查看最新的commit
git show

# 查看本地已同步的commit列表
git log

# 查看指定commit hashID的所有修改
git show your_commitId

# 查看当前未commit的改动细节
git diff

# 把当前未commit的改动先缓存起来
git stash

# 查看已缓存的改动列表
git stash list

# 把最近一个缓存取出来应用到当前分支
git stash pop
```
