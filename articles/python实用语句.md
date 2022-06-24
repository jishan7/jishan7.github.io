# python实用语句(不定时更新)

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
