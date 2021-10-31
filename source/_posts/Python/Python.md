---
title: Linux
tags:
  - Python
categories:
  - Python
date: 2020-08-19
---
pip install pipreqs

生成安装文件

pipreqs ./

安装生成的requirment.txt

pip install -r requirment.txt

###### 闭包

内部函数使用了外部函数的局部变量，就是闭包。

特点，变量不被污染，局部变量常驻内存。

判断是不是闭包__ __closure____

```python
def wraper():
    name = 1
    def inner():
        return name
    print(inner.__closure__)
    return inner
wraper()()
#(<cell at 0x00000276A0019F48: int object at 0x00007FFF54927110>,)
#能打印出来对象就是闭包，
```

###### 迭代器

可迭代的不一定是迭代器，迭代器都是可迭代的。

###### 生成器

本质就是迭代器。

1，生成器函数，关键字yeiled

```python
def test():
    for i in range(1000):
        a = yield i
        print(a)
print(test().__next__())
print(test().__next__())
#拿到所有的数据
for i in test():
    print(i)
#给上次的yeild赋值
ts =test()
print(ts.__next__())
ts.__next__()
ts.send(2)
```

<!--<u>生成器函数执行后返回的是生成器，不是执行函数。</u>-->

###### subprocess

subprocess模块的作用，即允许你去创建一个新的进程让其执行另外的程序，并与它进行通信，获取标准的输入，标准输出，标准错误以及返回码等等。

subprocess模块中定义了一个Popen类，通过它可以来创建进程，并与其进行复杂的交互。

参数说明

```python
def __init__(self, args, bufsize=-1, executable=None,
                 stdin=None, stdout=None, stderr=None,
                 preexec_fn=None, close_fds=True,
                 shell=False, cwd=None, env=None, universal_newlines=None,
                 startupinfo=None, creationflags=0,
                 restore_signals=True, start_new_session=False,
                 pass_fds=(), *, encoding=None, errors=None, text=None):
```

- agrs

  必须是一个字符串或者序列类型（列表，元组），哦用于指定进程的执行文件及其参数。如果是一个序列类型参数，则序列的第一个元素通常是可执行文件路径。

- stdin stdout stderr

  标准输入，输出，错误。

- shell

  程序设定为shell执行

- env

  描述环境变量，如果为None，继承父进程的环境。

```python
import subprocess
def test():
    res = subprocess.Popen("dir",cwd="d:",
                           stdout=subprocess.PIPE,#输出从管道获取
                           stderr=subprocess.STDOUT,#错误输入由输
                           )
    sout,serr =res.communicate()
    return res.returncode,sout,serr,res.pid
code,sout,serr,pid = test()
print(code,serr,pid,sout)
```

Pipe封装

```python
# -*- coding:utf-8 -*-

import subprocess
import sys
import threading

class LoopException(Exception):
    """循环异常自定义异常，此异常并不代表循环每一次都是非正常退出的"""
    def __init__(self,msg="LoopException"):
        self._msg=msg

    def __str__(self):
        return self._msg

class SwPipe():
    """
    与任意子进程通信管道类，可以进行管道交互通信
    """
    def __init__(self,commande,func,exitfunc,readyfunc=None,
        shell=True,stdin=subprocess.PIPE,stdout=subprocess.PIPE,stderr=subprocess.PIPE,code="GBK"):
        """
        commande 命令
        func 正确输出反馈函数
        exitfunc 异常反馈函数
        readyfunc 当管道创建完毕时调用
        """
        self._thread = threading.Thread(target=self.__run,args=(commande,shell,stdin,stdout,stderr,readyfunc))
        self._code = code
        self._func = func
        self._exitfunc = exitfunc
        self._flag = False
        self._CRFL = "\r\n"

    def __run(self,commande,shell,stdin,stdout,stderr,readyfunc):
        """ 私有函数 """
        try:
            self._process = subprocess.Popen(
                commande,
                shell=shell,
                stdin=stdin,
                stdout=stdout,
                stderr=stderr
                )
        except OSError as e:
            self._exitfunc(e)
        fun = self._process.stdout.readline
        self._flag = True
        if readyfunc != None:
            threading.Thread(target=readyfunc).start() #准备就绪
        while True:
            line = fun()
            if not line:
                break
            try:
                tmp = line.decode(self._code)
            except UnicodeDecodeError:
                tmp =  \
                self._CRFL + "[PIPE_CODE_ERROR] <Code ERROR: UnicodeDecodeError>\n"
                + "[PIPE_CODE_ERROR] Now code is: " + self._code + self._CRFL
            self._func(self,tmp)

        self._flag = False
        self._exitfunc(LoopException("While Loop break"))   #正常退出


    def write(self,msg):
        if self._flag:
            #请注意一下这里的换行
            self._process.stdin.write((msg + self._CRFL).encode(self._code))
            self._process.stdin.flush()
            #sys.stdin.write(msg)#怎么说呢，无法直接用代码发送指令，只能默认的stdin
        else:
            raise LoopException("Shell pipe error from '_flag' not True!")  #还未准备好就退出


    def start(self):
        """ 开始线程 """
        self._thread.start()

    def destroy(self):
        """ 停止并销毁自身 """
        process.stdout.close()
        self._thread.stop()
        del self

if __name__ == '__main__':   #那么我们来开始使用它吧
    e = None

    #反馈函数
    def event(cls,line):#输出反馈函数
        sys.stdout.write(line)

    def exit(msg):#退出反馈函数
        print(msg)

    def ready():#线程就绪反馈函数
        e.write("dir")  #执行
        e.write("ping www.baidu.com")
        e.write("echo Hello!World 你好中国！你好世界！")
        e.write("exit")

    e = SwPipe("cmd.exe",event,exit,ready)
    e.start()
```



