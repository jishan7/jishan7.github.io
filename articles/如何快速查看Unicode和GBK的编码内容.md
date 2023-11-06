# 如何快速查看Unicode和GBK的编码内容

有时候会因为编码问题，有些中文编成了乱码或奇怪的编码。乱码不好说，但编码可以恢复为正常的中文。

如何快速查看编码对应的中文？

## Unicode

例如'\u597d'

\u开头证明是unicode编码

可以打开 [这个网站](http://tool.chinaz.com/tools/unicode.aspx) 在线转换

python3命令行也可（python2的不行），例如：

```bash
>>> print('\u597d')
好
```

## GBK

例如'30\xe7\xba\xa7\xe7\xba\xa2\xe5\x8c\x85'

这种是gbk编码

直接打开python2命令行（python3不行），按以下方法输入：

```bash
>>> print '30\xe7\xba\xa7\xe7\xba\xa2\xe5\x8c\x85'
30级红包
```
