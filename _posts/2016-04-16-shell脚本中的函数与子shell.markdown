---

layout: post
title: "shell脚本中的函数与子shell"
subtitle: ""
date: 2016-04-16
author: Fengya
category: shell
tags: shell linux mac 函数 subshell
finished: true
---



# shell脚本中的函数与子shell

## 函数

### 函数的使用

bash中也有函数。一个函数就是一个子程序，是用于实现一串操作的代码块。bash中的函数的形式如下：

```shell
function function_name(){
  
}

function_name(){
  
}

function_name()
{
  
}
```

以上三种形式的函数均可达到目的。

使用函数的时候只要简单的使用函数的名字即可。

例如：

```shell
#!/bin/bash 

function abc(){
  echo abc
}

abc
```

其输出结果为：

```
abc
```

bash中的函数必须在其第一次调用之前被完成。没有如c语言中那种函数声明的形式。即使提前定义了函数的变量也不可以。但是只要执行顺序中，函数的定义在函数的使用之前即可。

考虑如下情况：

```shell
function f1(){
  echo abc
  f2
}

function f2(){
  echo cde
}

f1
```

这种情况中，虽然看起来f2的使用在其定义之前已经有过一次了，但是实际运行当中我们不难发现f2先定义了之后f1才调用了f2。因此这种情况也是可以的。

函数是可以嵌套的。

考虑如下情况：

```shell
f1(){
  f2(){
    echo abc
	}
}

f2
f1
f2
```

其输出结果类似于：

```
bash: f2: command not found

abc
```

第一行表示没有找到f2，第二行执行了f1，第三行找到了f2。

函数声明也可能出现在任何看起来不可能的应该有一个命令出现的地方。

由以上内容可以看出，其实函数的定义其实只是一种命令。而bash脚本是一种线性执行的语言。因此只要保证函数使用的流程正确，就可以将函数应用在各种可能的地方。

### 函数的传参

bash的函数能够接受参数并返回状态退出码。

函数以位置来引用传递来的参数，类似脚本的参数传递。例如$1,$2等等。

例如：

```shell
function abc(){
  echo $1
  return 22
}

abc cde
echo $?
```

以上内容为一个最简单的函数的传参及返回值的情况。

输出为：

```
cde
22
```

此外，shift命令也可以应用于传递给函数的参数，像工作在脚本的参数中那样。

函数接受参数通常只是接受参数的值。因此我们也可以传递间接引用给函数。

例如：

```shell
function echoabc(){
  echo $1
}

a=b
b=c

echoabc ${a}
echoabc ${!a}

b=d

echoabc ${a}
echoabc ${!a} 
```

以上内容的输出为：

```
b
c
b
d
```

与此同理，我们可以在函数中修改间接引用中的值：

```shell
#!/bin/bash

function changeabc(){
  echo $1
  eval "echo \$$1"
  eval "$1=xyz"
  eval "echo \$$1"
}

var1=var2
var2=abc

changeabc $var1
echo $var2
eval "echo \$$var1"
```

以上内容的输出为：

```
var2
abc
xyz
xyz
xyz
```

### 函数的退出码

函数返回一个被称为退出状态的值. 退出状态可以由return来指定statement, 否则函数的退出状态是函数最后一个执行命令的退出状态(0表示成功，非0表示出错代码). 退出状态(exit status)可以在脚本中由$? 引用. 这个机制使脚本函数也可以像C函数一样有一个"返回值".

可以用来返回异常状态。

### 函数的重定向

函数的本质也是一个代码块，因此我们可以重定向其标准输入输出。

例如：

```shell
function echoabc(){
	read line
	while [ -n "$line" ]
	do
		echo $line
		read line
	done
} << strings
abcd
cdef
strings

echoabc
```

将一个heredocuments重定向到函数echoabc的输入。输出为：

```
abcd
cdef
```

## 子shell

运行一个shell脚本时会启动另一个命令解释器. 就好像你的命令是在命令行提示下被解释的一样, 类似于批处理文件里的一系列命令.每个shell脚本有效地运行在父shell(parent shell)的一个子进程里.这个父shell是指在一个控制终端或在一个xterm窗口中给你命令指示符的进程.

shell脚本也能启动他自已的子进程. 这些子shell(即子进程)使脚本并行地，有效率地地同时运行多个子任务.

### 圆括号运行子shell

嵌在圆括号里的一列命令在一个子shell里运行。

例如：

```shell
(
echo abcd
echo cdef
)
```

输出为：

```
abcd
cdef
```

在子shell里的变量不能被这段子shell代码块之外外面的脚本访问.这些变量是不能被产生这个子shell的父进程(parent process)存取的,实际上它们是局部变量(local variables).

例如：

```shell
(a=b)
echo a
```

输出为空

在子shell中的目录更改不会影响到父shell.

子shell可用于为一组命令设定临时的环境变量

进程在不同的子shell中可以并行地执行.这样就允许把一个复杂的任务分成几个小的子问题来同时地处理。

### 管道产生子shell

管道（|）也会产生子shell。在子shell中可以读取父shell中的变量，但是不能写这些变量。有时我们可以通过重定向输入输出的方式来传递这些变量。详细的可以参考[I/O重定向](http://fengyalv.github.io/Blogs/shell/shell%E8%84%9A%E6%9C%AC%E4%B8%AD%E7%9A%84%E5%87%BD%E6%95%B0%E4%B8%8E%E5%AD%90shell.html)

