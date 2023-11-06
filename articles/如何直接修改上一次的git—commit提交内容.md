# 如何直接修改上一次的git commit提交内容？

review过后要改代码，但就是不想再额外加一个commit，这样搞的话很多commit很杂乱，也要重新写commit message很烦

其实，通过如下方法，可以直接复用上次的提交

1、改完代码后，add：

```bash
git add 你的代码文件
```

2、然后

```bash
git commit --am
```

3、此时应该会弹出vim编辑器界面，里面内容是你上次commit的message内容

直接:q退出就好，因为我们就是要原封不动的使用上次的message

4、最后git push

会发现commit没有增加，上一次的commit的diff已经应用上了你刚才的代码修改

