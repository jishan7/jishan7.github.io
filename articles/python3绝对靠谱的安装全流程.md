# python3怎么安装？绝对靠谱的安装全流程来了！
​
## 第一步：小心操作系统版本，这很重要，能避开很多莫名其妙的坑！

先保证自己的操作系统版本不要低于centos6

我的是：

```bash
[linyc@linyc plugin-server]$ lsb_release -a
LSB Version:	:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID:	CentOS
Description:	CentOS release 6.3 (Final)
Release:	6.3
Codename:	Final
```

## 第二步：补上openssl-devel

参考：[Python3中无法导入ssl模块的解决办法](https://blog.csdn.net/worldchinalee/article/details/79642926)

查看openssl安装包

```bash
rpm -aq|grep openssl
```

如果发现缺少openssl-devel包就执行以下安装

```bash
yum install openssl-devel -y     #  只能root权限执行
```

查看安装结果，应该是四个结果

```bash
rpm -aq|grep openssl
--
openssl098e-0.9.8e-17.el6.centos.2.x86_64
openssl-devel-1.0.1e-58.el6_10.x86_64
openssl-static-1.0.1e-58.el6_10.x86_64
openssl-1.1.1g-15.el8_3.x86_64
```

## 第三步：避坑ctypes错误
为了防止出现：ModuleNotFoundError: No module named '_ctypes'

需要先执行

```bash
yum install libffi-devel  -y
```

## 第四步：下载解压python3

没有特殊要求的话，强烈建议安装3.8及以上的版本，省心

```bash
wget https://www.python.org/ftp/python/3.x.x/Python-3.x.x.tgz  # 这里xx要填自己想要的版本号
tar zxf Python-....tgz
cd Python-3...
./configure --prefix=/usr/local/python3
```

## 第五步：编译安装python3

接下来为了避免出现import ssl错误（ImportError: No module named _ssl）

参考：

[Python3中无法导入ssl模块的解决办法](https://blog.csdn.net/worldchinalee/article/details/79642926)

[openssl升级后libssl.so.10缺失及版本问题](https://blog.csdn.net/worldchinalee/article/details/79642926)

修改Setup文件

```bash
vi Python-3.../Modules/Setup
```

找到如下内容，改为：

```
# Socket module helper for socket(2)
_socket socketmodule.c    #去除该行注释（备注）

# Socket module helper for SSL support; you must comment out the other
# socket line above, and possibly edit the SSL variable:
SSL=/usr/local/openssl
_ssl _ssl.c \      #去除该行注释（备注）
-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \  #去除该行注释（备注）
-L$(SSL)/lib -lssl -lcrypto         #去除该行注释（备注）
```

然后再进行python3的编译安装

```bash
make -j4
make install
```

然后，下载1.0.1的包

```bash
wget https://www.openssl.org/source/old/1.0.1/openssl-1.0.1e.tar.gz
```

下载完毕后解压。

进入解压后的目录，执行如下命令生成Makefile文件。

```bash
./config shared zlib-dynamic 
```

然后执行：

```bash
make
```

生成如下文件：

```
libssl.so.1.0.0
libcrypto.so.1.0.0
```

将两个文件拷贝到：

```
/usr/lib64 
```

创建软链接（ln源就是上面查出的对应版本的库文件）：

```bash
cd /usr/lib64/
ln -s libssl.so.1.0.0          libssl.so.10
ln -s libcrypto.so.1.0.0    libcrypto.so.10
```

然后在/usr/lib64/ 下查看效果

```bash
ll libssl.so*
---
lrwxrwxrwx 1 root root     15 Jan  5 14:06 libssl.so.10 -> libssl.so.1.0.0
-rwxr-xr-x 1 root root 487784 Jan  5 14:00 libssl.so.1.0.0
-rwxr-xr-x 1 root root 520912 Jan  4 22:32 libssl.so.1.1
```

到这一步，python3就装好了，可以敲python或者python3命令测一下

## 第六步：配置pip源

```bash
vi ~/.pip/pip.conf
```

加入如下内容：

```
​# 阿里巴巴的源（建议，目前的使用经验来看这个源更新的及时一些）
[global]
index-url = Simple Index
[install]
trusted-host = mirrors.aliyun.com
```

到这一步，基本上所需的就装好了，pip或者pip3命令测试一下
