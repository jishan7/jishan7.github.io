# Git代码冲突的解决办法

## 1. 在本地仓库中, 更新并合并代码

```bash
git fetch origin
git rebase origin/master
```

## 2. 依据提示分别打开冲突的文件, 逐一修改冲突代码

## 3. 所有冲突都修改完毕后, 提交修改的代码

```bash
git add -u
git rebase --continue
```

## 4. 更新patch

```bash
git push origin HEAD:refs/for/master
```
