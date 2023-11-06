# 【已解决】安装pyleus遇到的'bdist_wheel'错误​

今天用pip命令安装pyleus时遇到以下错误：
```
  Failed building wheel for PyYAML
  Running setup.py bdist_wheel for msgpack-python
  Complete output from command /usr/bin/python -c "import setuptools;__file__='/tmp/pip-build-685OEx/msgpack-python/setup.py';exec(compile(open(__file__).read().replace('\r\n', '\n'), __file__, 'exec'))" bdist_wheel -d /tmp/tmpUJDHTSpip-wheel-:
  usage: -c [global_opts] cmd1 [cmd1_opts] [cmd2 [cmd2_opts] ...]
     or: -c --help [cmd1 cmd2 ...]
     or: -c --help-commands
     or: -c cmd --help
 
  error: invalid command 'bdist_wheel'
```

查了下bdist_wheel，发现似乎是pip的一个子命令，wheel本质上是一种zip包，用于py模块的安装，它的出现是为了替代Eggs。

尝试运行pip wheel，果然有告错：
```
ERROR: 'pip wheel' requires setuptools >= 0.8 for dist-info support. To fix this, run: pip install --upgrade setuptools
```

于是根据它的建议，升级pip的安装工具setuptools ，命令是：

```bash
sudo pip install --upgrade setuptools
```


再试，出现如下信息：


```bash
[linyc@localhost etc]$ pip wheel
You are using pip version 7.1.0, however version 7.1.2 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
You must give at least one requirement to wheel (see "pip help wheel")
```

我不确定pip本身升级是不是必要，读者可以试一试不升级而直接安装pyleus。

升级pip完毕，尝试pip安装pyleus，啊，提示已存在。那么先uninstall，再安装。

一阵等待……后，ok搞定。
