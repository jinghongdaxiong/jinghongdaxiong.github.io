---
title: Linux简介
date: 2018-04-24 16:19:03
tags: Linux
---

	Linux内核最初是由芬兰人李纳斯.托瓦兹(Linux Torvalds)在赫尔辛基上大学时出于个人爱好而编写的.
	Linux是一套免费使用和自由传播的类Unix操作系统.是一个基于POSIX和UNIX的多用户 多任务 支持多线
	程和多CPU的操作系统.

	Linux能运行主要的UNIX工具软件 应用程序和网络协议.它支持32位和64位硬件.Linux继承了Unix以网络
	为核心的设计思想,是一个性能稳定的多用户网络操作系统.

## Linux的发行版本

	Linux的发行版本简单说就是讲Linux内核与应用软件做一个打包.
	目前市面上比较知名的发行版有:
	
###	Ubuntu
###	ReaHat
###	CentOS
###	Debian
###	Fefora
###	SuSE
###	OpenSUSE
###	Arch Linux
###	SolusOS

## Linux 系统启动过程
    
Linux系统的启动的启动过程分为5个阶段

* 内核的引导

* 运行init

* 系统初始化

* 建立终端

* 用户登录系统

    init 程序的类型
    
    * SysV:init,CentOS5之前,配置文件: /etc/initab.
    
    * Upstart:init,CentOS6,配置文件: /etc/inittab,/etc/init/*.conf.
    
    * Systemd: systemd,CenOS 7配置文件: /user/lib/systemd/system  /etc/systemd/system
    
    
## 内核引导

当计算机打开电源后,首先是BIOS开机自检,按照BIOS中设置的启动设备(通常是硬盘)来启动.
操作系统接管硬件以后,首先读入/boot目录下的内核文件

操作系统 -----> boot --->

## 运行init    

init进程是系统所有进程的起点,你可以把它比拟成系统所有进程的老祖宗,没有这个进程,系统
中任何进程都不会启动.
init程序首先需要读取配置文件 /etc/inittab.

操作系统 -----> boot ---> init进程 --->运行级别

Linux系统有七个运行级别
    
   * 0 : 系统停机状态,系统默认运行级别不能设为0,否则不能正常启动
   
   * 1 : 单用户工作状态,root权限,用于系统维护,禁止远程登录
   
   * 2 : 多用户状态(没有NFS)
   
   * 3 : 完全的多用户状态,登录后进入控制台命令行模式
   
   * 4 : 系统未使用,保留
   
   * 5 : X11控制台,登录后进入图形GUI模式
   
   * 6 : 系统正常关闭并重启,默认运行级别不能设为6,否则不能正常启动
   
   
## 系统初始化   

在init的配置文件中有这么一行: si:sysinit:/etc/rc.d/rc.sysint 它调用执行了

/etc/rc.d/rc.sysinit 它调用执行了/etc/rc.d/rc.sysinit,而rc.sysinit是一个bash

shell 的脚本 ,它主要是完成一些系统初始化的工作,rc.sysinit是每一个运行级别都

要首先运行的重要脚本.

它主要完成的工作有:激活交换区,检查磁盘,加载硬件模块以及其他一些需要优先执行

任务.

15:5:wait:/etc/rc.d/rc 5

这一行表示以5为参数运行/etc/rc.d/rc, /etc/rc.d/rc是一个shell脚本,它接收5作为参数

,去执行/etc/rc.d/rc5.d/目录下的所有的rc启用脚本,/etc/rc.d/rc5.d/目录中的这些启动

脚本实际上都是一些连接文件,而不是真正的rc启动脚本,真正的rc启动脚本实际上都是放在

/etc/rc.d/init.d/目录下

而这些rc启动脚本有着类似的用法,它们一般能接受start stop restart status等参数.

/etc/rc.d/rc5.d/中的rc启动脚本通常是K或S开头的连接文件,用于以S开头的启动脚本

,将以start参数来运行. 而如果发现存在相应的脚本也存在K打头的连接,而且已经处于运

行状态了(以/var/lock/subsys/下的文件为标志),则将首先以stop为参数停止这些已经启动

了的守护进程,然后再重新运行.

这样做是为了保证当init改变运行级别时,所有相关的守护进程将重启.

至于在每个运行级别中将运行哪些守护进程,用户可以通过chkconfig或setup中的

"System Services"来自行设定.

操作系统 ---> /boot ---> init进程 ---> 运行级别 ---> /etc/init.d


## 建立终端

rc执行完毕后,返回init.这时基本系统环境已经设置好了,各种守护进程也已经启动了.

init接下来会打开6个终端,以便用户登录系统.在inittab中的以下6行就是定义了6个终

端:

1:2345:respawn:/sbin/mingetty tty1

2:2345:respawn:/sbin/mingetty tty2

3:2345:respawn:/sbin/mingetty tty3

4:2345:respawn:/sbin/mingetty tty4

5:2345:respawn:/sbin/mingetty tty5

6:2345:respawn:/sbin/mingetty tty6

从上面可以看出在2 3 4 5的运行级别中都将以 respawn 方式运行mingetty程序能打开终端

设置模式.同时它会显示一个文本登录界面,这个界面就是我们经常看到的登录界面,在这个等

录界面中会提示用户输入用户名,而用户输入的用户名将作为参数传给login程序来验证用户的

身份

## 用户登录系统

一般来说,用户的登录方式有三种:

* 1 命令行登录

* 2 ssh登录

* 3 图形界面登录

操作系统 --> /boot --> init进程 --> 运行级别 --> /etc/init.d --> 用户登录

对于运行级别为5的图形方式用户来说,他们的登录是通过一个图形化的登录界面.登录

成功后可以直接进入 KDE 或 Gnome 等窗口管理器 

而本文主要讲的是文本登录:当我们看到mingetty的登录界面时,我们就可以输入用户名

和密码来登录系统了.

Linux的账号验证程序是login,login会接收mingetty传来的用户名作为用户名参数.

然后login会对用户名进行分析:如果用户名不是root,且存在/etc/nologin文件,login

将输出nologin文件的内容,然后退出.

这通常用来系统维护时防止非root用户登录.只有/etc/securetty中登记了的终端才允许

root用户登录,如果不存在这个文件,则root用户可以在任何终端上登录.

/etc/usetty文件用于对用户作出附加访问限制,如果不存在这个文件,则没有其他限制

## 图形模式与文字模式的切换方式

Linux预设提供了六个命令 窗口终端机让我们来登录.

默认我们登录的就是第一个窗口,也就是tty1,这六个窗口分别为 tt1,tt2,tt3...tt6,
你可以按下

Ctrl + Alt + F1 ~ F6

来切换它们

如果你安装了图形界面,默认情况下是进入图形界面的,此时你就可以按Ctrl+Alt+F1~F6来
进入其中一个命令窗口界面.当你进入命令窗口界面后再返回图形界面只要按下

Ctrl + Alt + F7就回来了

如果你用的vmware虚拟机,命令窗口切换的快捷键位Alt+Space+F1~F6.如果你在图形界面下

请按Alt+Shift+Ctrl+F1~F6切换至命令窗口.

操作系统 --> /boot --> init进程 --> 运行级别 --> /etc/init.d 
                                                        |
                                                        |
                                       login shell <--用户登录

## Linux 关机

在Linux领域内大多用再服务器上,很少遇到关机的操作,毕竟服务器上跑一个服务时永无止境的,
除非特殊情况下,不得已才会关机.

正确的关机流程为 : sync > shutdown > reboot > halt

关机指令为 : shutdown,你可以man shutdown来看一下邦之文档.

* sync 将数据由内存同步到硬盘中

* shutdown 关机指令,你可以man shutdown来看一文档

* shutdown -h 10  10分钟后关机

* shutdown -h now 立马关机

* shutdown -h 20:25 系统会在今天20:25关机

* shutdown -h +10 十分钟后关机

* shutdown -r now 立马重启

* shutdown -r +10 十分钟后重启

* reboot 重启,等同于 shutdown -r now

* halt 关闭系统,等同于 shutdown -h now 和 poweroff

最后总结一下，不管是重启系统还是关闭系统，首先要运行 sync 命令，把内存中的数据写到磁盘中。
关机的命令有 shutdown –h now halt poweroff 和 init 0 , 重启系统的命令有 shutdown –r now reboot init 6。










