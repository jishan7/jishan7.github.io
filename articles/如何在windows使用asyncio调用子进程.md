# 如何在windows使用asyncio调用子进程

尝试在win环境运行python3的asyncio.create_subprocess_shell，

也就是希望能够以协程异步的形式，运行一个子进程。

希望不仅能够获得进程id，而且主进程可以在等待子进程完成的时候，让出cpu时间。

但是，初步的代码，没法运行，会报错：

```python
# run.py
# coding=utf-8
import time
while 1:
    print(1)
    time.sleep(10)
```

```python
# start.py
# coding=utf-8
import asyncio
import time

async def start():
    proc = await asyncio.create_subprocess_shell("python .\run.py",
                                            stdin=asyncio.subprocess.PIPE,
                                            stdout=asyncio.subprocess.PIPE,
                                            stderr=asyncio.subprocess.PIPE)
    print(proc.pid)
    await proc.wait()
    return proc.pid

if __name__ == "__main__":
    asyncio.run(start())
```


```
报错信息：
Traceback (most recent call last):
  File ".\start.py", line 35, in <module>
    asyncio.run(start())
  File "C:\Users\linyc\AppData\Local\Programs\Python\Python37\lib\asyncio\runners.py", line 43, in run
    return loop.run_until_complete(main)
  File "C:\Users\linyc\AppData\Local\Programs\Python\Python37\lib\asyncio\base_events.py", line 587, in run_until_complete
    return future.result()
  File ".\start.py", line 14, in start
    stderr=asyncio.subprocess.PIPE)
  File "C:\Users\linyc\AppData\Local\Programs\Python\Python37\lib\asyncio\subprocess.py", line 202, in create_subprocess_shell
    stderr=stderr, **kwds)
  File "C:\Users\linyc\AppData\Local\Programs\Python\Python37\lib\asyncio\base_events.py", line 1514, in subprocess_shell
    protocol, cmd, True, stdin, stdout, stderr, bufsize, **kwargs)
  File "C:\Users\linyc\AppData\Local\Programs\Python\Python37\lib\asyncio\base_events.py", line 462, in _make_subprocess_transport
    raise NotImplementedError
NotImplementedError
```

于是去查了下官网文档

https://docs.python.org/3.7/library/asyncio-subprocess.html

尝试搜关键词“win”，还真的有描述：

```
Note The default asyncio event loop implementation on Windows does not support subprocesses. Subprocesses are available for Windows if a ProactorEventLoop is used. See Subprocess Support on Windows for details.
```

这个文档，翻到最后面其实可以看到官方示例：

```python
import asyncio
import sys

async def get_date():
    code = 'import datetime; print(datetime.datetime.now())'

    # Create the subprocess; redirect the standard output
    # into a pipe.
    proc = await asyncio.create_subprocess_exec(
        sys.executable, '-c', code,
        stdout=asyncio.subprocess.PIPE)

    # Read one line of output.
    data = await proc.stdout.readline()
    line = data.decode('ascii').rstrip()

    # Wait for the subprocess exit.
    await proc.wait()
    return line

if sys.platform == "win32":
    asyncio.set_event_loop_policy(
        asyncio.WindowsProactorEventLoopPolicy())

date = asyncio.run(get_date())
print(f"Current date: {date}")
```

继续顺着原文档里的链接摸过去：
https://docs.python.org/3.7/library/asyncio-platforms.html#asyncio-windows-subprocess

也可以看到这个 WindowsProactorEventLoopPolicy 的具体描述

```
SelectorEventLoop on Windows does not support subproceses. On Windows, ProactorEventLoop should be used instead
```

懂了，那就替换掉event_loop_policy呗，改成这样：


```python
# coding=utf-8
import asyncio
import time
import sys

if sys.platform == "win32":
    asyncio.set_event_loop_policy(
        asyncio.WindowsProactorEventLoopPolicy())

async def start():
    proc = await asyncio.create_subprocess_shell("python .\run.py",
                                            stdin=asyncio.subprocess.PIPE,
                                            stdout=asyncio.subprocess.PIPE,
                                            stderr=asyncio.subprocess.PIPE)
    print(proc.pid)
    await proc.wait()
    return proc.pid

if __name__ == "__main__":
    asyncio.run(start())
```

奇怪，还是不对！

执行了下，立即就退出了，没有报错。

我写的run.py明明是一个死循环来着，期待的状态应该是一直处在等待中才对。

猜测是子进程执行报错了。

仔细看了下，才发现，“.\run.py” 不能这么写！

虽然windows powershell里确实是“python .\run.py”这样执行的。

但是python里，反斜杆会转义掉的，

其实，只需要直接“python run.py”就可以了。

于是改成这样：

```python
# coding=utf-8
import asyncio
import time
import sys

if sys.platform == "win32":
    asyncio.set_event_loop_policy(
        asyncio.WindowsProactorEventLoopPolicy())

async def start():
    proc = await asyncio.create_subprocess_shell("python run.py",
                                            stdin=asyncio.subprocess.PIPE,
                                            stdout=asyncio.subprocess.PIPE,
                                            stderr=asyncio.subprocess.PIPE)
    print(proc.pid)
    await proc.wait()
    return proc.pid

if __name__ == "__main__":
    asyncio.run(start())
```

会得到一个进程号的打印。拿到这个进程号就可以点对点杀掉这个子进程，子进程没了后这个主进程也会退出。

注意!

在windows powershell那里，尽量使用这个命令来查找某个进程号对应的进程

```powershell
tasklist|findstr 一个进程号
```

而不是使用这个

```powershell
Get-process|findstr 一个进程号
```

因为proc.pid这个进程号是tasklist里的进程号，Get-process里的进程号完全对不上号的！

包括如果要终止这个子进程，也必须使用配套的命令才行：

```powershell
taskkill /t /f /pid 一个进程号
```

附1：tasklist的返回示例

```powershell
PS C:\Users\linyc\Desktop> tasklist
映像名称                       PID 会话名              会话#       内存使用
========================= ======== ================ =========== ============
System Idle Process              0 Services                   0          8 K
System                           4 Services                   0         28 K
Registry                        96 Services                   0     23,860 K
```

附2：Get-process的返回示例

```powershell
PS C:\Users\linyc\Desktop> get-process
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    207      17     3984       5620       0.66   8020   1 AeXAgentUIHost
    568      34    16704       7908              3048   0 AeXNSAgent
    412      26    20648       3260       0.83   8876   1 ApplicationFrameHost
```

ps命令在powershell这里能用，结果和Get-process一样。

```powershell
PS C:\Users\linyc\Desktop> ps
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    211      17     4016       5636       0.66   8020   1 AeXAgentUIHost
    571      34    16704       8104              3048   0 AeXNSAgent
    412      26    20648       3260       0.83   8876   1 ApplicationFrameHost
```
