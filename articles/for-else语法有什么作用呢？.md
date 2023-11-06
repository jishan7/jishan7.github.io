
# for else语法有什么作用呢？

for else后面的else内容，仅在循环正常结束后，才会执行。

## 示例1

```python
# 循环被break了，属于非正常结束循环，于是else语句不执行
for i in range(3):
    if i == 2:
        break
    print(i)
else:
    print('end')
```

打印内容：

```
0
1
```

## 示例2
```python
# 循环被continue了，但循环还是正常结束的，于是else语句会执行
for i in range(3):
    if i == 2:
        continue
    print(i)
else:
    print('end')
```

打印内容：

```
0
1
end
```

## 示例3

```python
# 这里的break不会触发，循环正常结束，于是else语句会执行
for i in range(3):
    if i == 10:
        break
    print(i)
else:
    print('end')
```

打印内容：

```
0
1
2
end
```
