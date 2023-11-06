# 【已解决】redis-cli后打字就出现无数个127.0.0.1:6379>

症状：
```
[root@iZ25ngimygcZ redis-2.8.17]# redis-cli
127.0.0.1:6379> 127.0.0.1:6379> p127.0.0.1:6379> pu127.0.0.1:6379> put127.0.0.1:6379> put 127.0.0.1:6379> put
(error) ERR unknown command 'put'
127.0.0.1:6379>
```

解决方法：

如果你用的是SecureCRT，换个终端

或者，设置当前的Session Options-->Terminal-->Emulation-->Terminal为Linux

注意：这个方法同样可以解决键盘home、end、delete键不好使的问题。

笔者原先设置是VT100，而不是linux，所以没法很开心的使用这几个键，现在好了~

