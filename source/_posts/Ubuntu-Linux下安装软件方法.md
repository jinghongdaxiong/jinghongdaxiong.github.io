---
title: Ubuntu Linux下安装软件方法
date: 2018-04-20 15:02:03
tags: Linux
categories: 
- linux
---

## 引用: https://www.linuxidc.com/Linux/2015-01/111216.htm

    Linux系统中,软件通常以源代码或者编译包的形式提供.
    
    (1)源代码需要编译为二进制的机器码才能够使用,安装比较
    耗时,不过您可以自行调节编译选项,决定需要的功能或组
    件,或者针对硬件平台做一些优化.
    
    (2)预编译的软件包,通常是由软件的发布者进行编译,您只要
    将软件拷贝到系统中就可以了.考虑到预编译软件包的适用性
    ,预编译软件通常不会针对某种硬件平台优化.它所包含的功能
    和组件也是通用的组合.
    
    
## 1 deb包的安装方式

    deb是debian系Linux的包管理方式,Ubuntu是属于debian系的
    Linux发行版,所以默认支持这种软件安装方式,当下载到一个
    deb格式的软件后,在终端输入这个命令就能安装:
    
    sudo dpkg -i *.deb
    
    或者直接双击安装.
    
## 2 编译安装方式

    (小贴士: 使用编译安装前,需要建立编译环境,使用一下命令建立
    基本的编译环境: sudo apt-get install bulid-essential)
    
    在Linux的世界,有很多软件只提供了源代码给你,需要你自己进行
    编译安装,一般开源的软件都会使用tar.gz压缩档来进行发布,当然
    还有其他的形式,拿到源代码的压缩文档,把它解压缩到/tmp目录下
    ,进入/tmp/软件目录,然后执行下三个命令:
    
   * 1 ./configure
   * 2 make 
   * 3 sudo make install
    
    在第一步./configure时可能会提示说某某软件找不到,例如提示"libgnome"
    这个开发包找不到,那就把libgnome这个关键词copy,然后打开新立得软件管
    理器,在里面搜索libgnome这个关键词,就会找到libgnome相关的项目,把前
    面有个ubuntu符号的libgnome包(注意:同样需要安装dev包,但可以不装doc
    包)全部安装,通过这个方法把./configure过程中缺失的开发包全部装上就
    ok了,第一步顺利通过,第二三步基本问题不大.
    
## 3 apt-get安装方法
    
    ubuntu世界有许多软件源,在系统安装篇已经介绍过如何添加源,apt-get的基本
    软件安装命令是:
     
    sudo apt-get install 软件名
    
## 4 新立得软件包管理 
    打开: 系统 --系统管理--新立得软件包管理,这个工具其实跟apt一样,可以搜索
    ,下载,安装ubuntu源里的软件,具体安装方式很简单,看看界面应该会懂,就不详细
    介绍了.
    
## 5 二进制包的安装方式
    
    有不少开源的商业软件都会采用这种方式发布Linux软件,例如google earth,拿到
    二进制软件后,把它放到/tmp目录,在终端下进入安装目录,在安装目录下执行:
    
    ./软件名
    
    然后按照一步步提示,就能安装该软件.例如安装realplayer播放器:你直接到官网
    http://www.real.com/linux 下载RealPayer的安装包,安装包是.bin格式,用如下
    命令安装:
    
    chomd +x RealPlayer11GOLD.bin
    
    ./RealPlayer11GOLD.bin
    
## 6 rpm包的安装方式
    
    rpm 包是deb包最常见的一种管理方式,但ubuntu同样可以使用rpm的软件资源.首先
    我们安装一个rpm转deb的软件
    
    sudo apt-get install alien
    
    然后就可以对rpm格式的软件转换成deb格式了:
    
    alien -d *.rpm
    
    然后就可以用deb的安装方式进行软件安装.也可以不需转换而直接对rpm包进行安装:
    
    alien -i *.rpm
    
    更多的alien使用方法可以用-h参数查看相应说明文档.
    
## 7 其他安装方式

    其他安装方式一般还有脚本安装方式,这类软件,你会在安装目录下发现类似后缀名的文件
    ,如: .sh .py .run等的,有的甚至连后缀名都没有,直接一个INSTALL文件,对于这种软件,
    可以尝试以下几种方式安装:
    最简单的就是直接在软件目录下输入: ./软件名*(注意有一个*号,那是一般可以通配所有
    后缀名)
    或者: sh 软件名.sh
    或者: python软件名.py
    
    TIP:如以上方法均无法安装软件,可以参考软件源代码下面的README文档.
    
    