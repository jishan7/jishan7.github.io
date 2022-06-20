# Python class中的模式方法 __call__()是什么鬼？！

```python
class Animal(object):
    def __call__(self, words):
        print "Hello: ", words

if __name__ == '__main__':
    cat = Animal()
    cat("I am cat!")

```

可以得知，cat这个时候因为__call__的缘故，既可以作为示例变量名，又可以作为函数名使用。
