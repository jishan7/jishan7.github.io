# Patch文件的生成和使用

参考：[patch文件的生成和使用](https://www.cnblogs.com/hanrp/p/11088589.html)

## 1. 下载原版whl

此处以ipykernel为例

```bash
pip download --index-url=https://pypi.tuna.tsinghua.edu.cn/simple --trusted-host=https://pypi.tuna.tsinghua.edu.cn --no-deps ipykernel==6.9.1
```

## 2. 解压

```bash
unzip ipykernel-6.9.1-py3-none-any.whl
```

## 3. 复制出新的文件夹ipykernel.new

```bash
cp -r ipykernel ipykernel.new
```

## 4. 将想要的改动加到ipykernel.new

略

## 5. 执行diff命令生成.patch文件

```bash
diff -uprN ipykernel/ ipykernel.new/ > ipykernel.patch
```

## 6. 应用改动

把ipykernel.patch移动到有ipykernel文件夹的地方
例如这样：

```bash
[linyc@linyc tmp]$ ls
ipykernel  ipykernel.patch
```

然后执行命令：

```
patch -re -p0 < ipykernel.patch
```

这样ipykernel文件夹里就有.new的改动了
