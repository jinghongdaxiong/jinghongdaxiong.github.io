---
title: php调用shell的方法
date: 2018-04-20 10:07:17
tags:
---

## 引用:http://www.jb51.net/article/57118.htm

## 这篇文章主要介绍了PHP调用shell的方法,包括相关的原理 注意事项及函数的用法,需要的朋友可以参考下

### 一 配置
#### 查看php.ini中配置是否打开安全模式,主要是以下三个地方 

* safe_mode = (这个如果是off下面两个就不用管了)

* disable_functions = 

* safe_mode_exec_dir = 

### 二使用

#### 由于PHP是基本是用于WEB程序开发的,所以安全性成了人们考虑的一个重要方面.于是PHP的设计者们给  PHP加了一个门:安全模式.如果运行在安全模式下,那么PHP脚本中将受到如下四个方面的限制:

* 执行外部命令

* 在打开文件时有些限制

* 连接MYSQL数据库

* 基于HTTP的认证

在安全模式下,只有在特定目录中的外部程序才可以被执行,对其他程序的调用将被拒绝.这个目录可以在  PHP.ini文件中用safe_model_exex_dir指令,或在编译PHP时加上--with-exec-dir选项来指定,默认是  /usr/local/php/bin.

如果你调用一个应该可以输出结果的外部命令(意思是PHP脚本没有错误),得到的却是一片空白,那么很可能   你的网管已经把PHP运行在安全模式下了.

### 三如何做

#### 在PHP中调用外部命令,可以用如下三种方法来实现:

* 1 ) 用PHP提供的专门函数

### php提供了3个专门的执行外部命令的函数: system(),exec(),passthru().


### system()
原型: string system(string command[,int return_var])

system()函数和其他语言中的差不多,它执行给定的命令,输出和返回结果.第二个参数是可选的,用来得到命令执行后的状态码.

例子:

* system("/usr/local/bin/webalizer/webalizer");

### exec()

原型: string exex(string command[,string array[,int return_var]])

exec()函数与system()类似,也执行给定的命令,但不输出结果,而是返回结果的最后一行.虽然它只返回命令  结果的最后一行,但用第二个参数array可以得到完整的结果,方法是吧结果逐行追加到  array的结尾处.所 以如果array不是空的,在调用之前最好用unset()把它清掉.只有指定了第二个参数时,才可以用第三个参数,用来取得命令执行的状态码.

例子:
* exec("/bin/ls -|");
* exec("/bin/ls -|",$res);'
* $res是一个数据,每个元素代表结果的一行
* exec("/bin/ls -|",$res,$rc);'
* $rc的值是命令/bin/ls -|的状态码.成功的情况下通常是0


### passthru()

  原型: void passthru(string command[,int return_var])
  
  passthru()只调用命令,不返回任何结果,但把命令的运行结果原样
  地直接输出到标准输出设备上.所以passthru()函数经常用来调用像
  pbmplus(Unix下的一个处理图片的工具,输出二进制的原始图片的流)
  这样的程序.同样它也可以得到命令执行的状态码.
  
  例子
  
  header("Content-type:image/gif");
  passthru("./ppmtogif hunte.ppm");


