# Vim着色设置

参考：

https://zhuanlan.zhihu.com/p/497315237

http://blog.chinaunix.net/uid-21288388-id-4847193.html

(1) 确认当前用户目录下存在~/.vim/colors目录，没有则新建，安装的Vim配色方案对应.vim文件需放在该目录下

(2) 下载或编辑某个配色方案的.vim文件，保存到~/.vim/colors目录下

(3) 然后编辑 vi ~/.vimrc 的配置文件，添加 “colorscheme 着色文件名”，这里请注意，如果下载的着色文件名为 example.vim ，那就填写 “colorscheme example”，后缀名不要加，否则会提示找不到文件。

(4) 下载着色文件：gruvbox.vim，下载地址：https://github.com/morhetz/gruvbox

(5) vim语法着色，参考：https://blog.csdn.net/SwellHuang/article/details/103782352

打开.vimrc，然后添加如下：

```
# vi .vimrc

set hlsearch"高亮度反白
set backspace=2 "可随时用退格键删除
set autoindent"自动缩进
set ruler "可显示最后一行的状态
set showmode"左下角那一行的状态
set nu"显示行号
set bg=dark "设置底色色调
syntax on "进行语法检验，并对语法着色
```

使用一下命令，使vi和vim都有颜色

```
alias vi='vim'
```
