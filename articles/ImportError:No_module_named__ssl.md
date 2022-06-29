# ImportError: No module named _ssl 安装python时遇到的，如何解决？

​
不是我爱diss
参考了网上几篇，有很多疏漏粗心之处，没法解决问题

只能多篇综合参考，最终搞出了我实际可行的解决方案！

请往下看：

1、修改Setup文件（这里的...是你下载的python安装包目录。通过源码安装，就是容易遇到ssl问题）

```bash
vi Python-3.../Modules/Setup
```

2、然后参照我的改法：

```
# Socket module helper for socket(2)
_socket socketmodule.c    #去除该行注释（备注）

# Socket module helper for SSL support; you must comment out the other
# socket line above, and possibly edit the SSL variable:
SSL=/usr/local/openssl
_ssl _ssl.c \      #去除该行注释（备注）
-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \  #去除该行注释（备注）
-L$(SSL)/lib -lssl -lcrypto         #去除该行注释（备注）
```

3、然后再进行编译

```bash
make -j4
make install
```

4、然后，下载1.0.1的包

```bash
wget https://www.openssl.org/source/old/1.0.1/openssl-1.0.1e.tar.gz
```
 
5、下载完毕后解压，进入解压后的目录

6、通过执行以下命令，来生成Makefile文件。

```bash
./config shared zlib-dynamic
```

7、然后执行这个make命令：

```bash
make
```

将会生成：

```bash
libssl.so.1.0.0
libcrypto.so.1.0.0
```

8、将两个文件拷贝到这个目录

```bash
/usr/lib64 
```

9、创建软链接（ln源就是上面查出的对应版本的库文件）：

```bash
cd /usr/lib64/
ln -s libssl.so.1.0.0          libssl.so.10
ln -s libcrypto.so.1.0.0    libcrypto.so.10
```

10、然后在/usr/lib64/ 目录下查看效果
```bash
ll libssl.so*
---

lrwxrwxrwx 1 root root     15 Jan  5 14:06 libssl.so.10 -> libssl.so.1.0.0
-rwxr-xr-x 1 root root 487784 Jan  5 14:00 libssl.so.1.0.0
-rwxr-xr-x 1 root root 520912 Jan  4 22:32 libssl.so.1.1
```

11、这时再返回到你之前报错的地方重新执行，就可以了
