# python元类是什么玩意儿

道生一，一生二，二生三，三生万物。

道 即是 type;

一 即是 metaclass(元类，或者叫类生成器);

二 即是 class(类，或者叫实例生成器);

三 即是 instance(实例);

万物 即是 实例的各种属性与方法，我们平常使用python时，调用的就是它们。

元类就是metaclass，也就是类生成器

文章里的前两个例子的写法应该是有问题，直接拿来，会运行报错

我能力范围内只能改例子2……

例子2：

```python
# 道生一
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        # 天赋：通过add方法将值绑定
        attrs['add'] = lambda self, value: self.append(value)
        return type.__new__(cls, name, bases, attrs)

# 一生二
class MyList(list):
    __metaclass__ = ListMetaclass
    
# 二生三
L = MyList()

# 三生万物
L.add(1)

print L
```
