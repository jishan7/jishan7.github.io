# python3实用语句(不定时更新)

```python
>>> filter(None, [3,23,23,56,None,3])
[3, 23, 23, 56, 3]


>>> reduce(lambda x, y: x+y, [1,2,3,4,5])
15
```

```python
# 文件名 /Users/linyc/tmp/test.py
# coding=utf-8
import os
read_file_path = os.path.abspath(os.path.dirname(__file__))
print(read_file_path)

# 打印结果：
# /Users/linyc/tmp
```

```python
# 打开文件
fo = open("test.txt", "w")
print "文件名为: ", fo.name
seq = ["菜鸟教程 1\n", "菜鸟教程 2"]
fo.writelines( seq )

# 关闭文件
fo.close()
```

```python
# 使用ch中存储的运算符，直接进行运算
>>> first_num, second_num = 2, 5
>>> ch = '/'
>>> int(eval(f'{second_num} {ch} {first_num}'))
2
```

```python
# 反转列表
>>> li = [3, 2, 1]
>>> li[::-1]
[1, 2, 3]
```

```python
# append时的浅拷贝
>>> li=[]
>>> per=[1,2,3]
>>> li.append(per)  # 注意此处写法
>>> li
[[1, 2, 3]]
>>> per.append(4)
>>> li
[[1, 2, 3, 4]]  # 只对per操作，结果影响了li

# append时的深拷贝
>>> li2=[]
>>> per=[1,2,3]
>>> li2.append(per[:])  # 注意此处写法
>>> per.append(4)
>>> li2
[[1, 2, 3]]  # per里增加了4，不影响li2里的拷贝内容
>>> per
[1, 2, 3, 4]
```

# 在列表中找到某个val的位置index
>>> orders=[1,2,3]
>>> idx = orders.index(2)
>>> idx
1

# 列出当前目录的所有文件名，包含子目录、隐藏文件等
>>> import os
>>> os.listdir('.')
['11.py', 'default-README.md', 'articles', 'pics', 'README.md', '.gitignore', '_config.yml', '1.py', '.git']

```
