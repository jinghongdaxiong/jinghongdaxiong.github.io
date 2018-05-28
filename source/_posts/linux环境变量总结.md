---
title: linux环境变量总结
date: 2018-05-28 13:42:14
tags:
---

## 引用https://www.jianshu.com/p/ac2bc0ad3d74

Linux是一个多用户多任务的操作系统,可以在Linux中为不同的用户设置不同的运行环境,
具体做法是设置不同用户的环境变量.

## Linux环境变量分类

一 按照生命周期来分,Linux环境变量可以分为两类:
1 永久的:需要用户修改相关的配置文件,变量永久生效.
2 临时的:用户利用export命令,在当前终端下声明环境变量,关闭shell终端失效.

二 按照作用域来分,Linux环境变量可以分为:
1 系统环境变量:系统环境变量对系统中的所有用户都有效.
2 用户环境变量:顾名思义,这种类型的环境变量只对特定的用户有效.

## Linux设置环境变量的方法

一 在/etc/profile文件中增加变量,改变量将会对Linux下所有用户有效,并且是永久的.

example:

    vim /etc/profile    
    export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
    
注意:修改文件后要想马上生效还要运行$ source ~/.bash_profile不然只能在下次
重进此用户是生效.

二 在用户目录下的.bash_profile文件中增加变量[对单一用户生效(永久的)]
用vim ~/.bash_profile文件中增加变量,改变仅会对当前用户有效,并且是"永久的".

    vim ~/.bash.profile
    export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
注意:修改文件后要想马上生效还要运行$ source ~/.bash_profile不然只能在下次
重进此用户时生效.

三 直接运行export命令定义变量[只对当前shell(bash)有效(临时的),]
在shell的命令行下直接使用   export 变量名=变量值
定义变量，该变量只在当前的shell（BASH）或其子shell（BASH）下是有效的，
shell关闭了，变量也就失效了，再打开新shell时就没有这个变量，需要使用的
话还需要重新定义。

## Linux环境变量使用

一 Linux中常见的环境变量有:

* PATH :指定命令的搜索路径

    
    PATH声明用法:
    PATH=$PATH:<PATH 1>:<PATH 2>:<PATH 3>:------:<PATH n>
    export PATH
    你可以自己加上指定的路径,中间用冒号隔开.环境变量更改后,在用户下次登录时生效.
    echo $path 查看当前系统path路径
    
* HOME: 指定用户的主工作目录(即用户登陆到Linux系统中时,默认的用户目录)    
* HISTSIZE: 指保存历史命令记录的条数.
* LOGNAME: 指当前用户的登陆名.
* HOSTNAME: 指定主机的名称,许多应用程序如果用到主机名的话,通常是从这个环境
变量中来取得的
* SHELL: 指当前用户用的是哪种shell.
* LANG/LANGUGE: 和语言相关的环境变量,使用多种语言的用户可以修改此环境变量.
* MAIL: 指当前用户的邮件存放目录.

注意:上述变量的名字并不固定,如:HOSTNAME在某些Linux系统中可能设置成HOST

二 Linux也提供了修改和查看环境变量的命令,下面通过几个实例来说明:

* echo 显示某个环境变量值 echo $PATH
* export 设置一个新的环境变量 export HELLO="hello"(可以无引号)
* env 显示所有环境变量
* set 显示本地定义的shell变量
* unset 清除环境变量 unset HELLO
* readonly 设置只读环境变量 readonly HELLO

三 C程序调用环境变量函数

* getenv() 返回一个环境变量.
* setenv() 设置一个环境变量.
* unsetenv() 清除一个环境变量.

    
    
