# 使用pem快速进行ssh登录

如果要登录的服务器只允许pem认证

每次输入ssh -i xxxx.pem 用户@ip 地址 就很烦

这里有个一劳永逸的方法：

进入到自己的用户目录，例如/home/me

把pem文件放在/home/me/.ssh/你的pem文件名.pem

然后

```bash
vi .ssh/config
```

将文件内容修改为如下：

```plain
  Host *
      ServerAliveInterval 60
  Host denglu
      HostName 你的ip
      User linyc

IdentityFile ~/.ssh/你的pem文件名.pem
```

保存后，更改权限为：

```bash
-rw------- 1 me me 132 12月 18 10:55 .ssh/config
```

完成设置。可以立即输入以下命令，测试一下（前提是你已经登录了me这个账号）

```bash
ssh denglu
```

即可登录到目标服务器。再也不用每次输入长长的命令了。
