---
title: .zip和.tar.gz的文件有什么区别
date: 2018-04-27 20:22:56
tags:
---

## 转自:https://blog.csdn.net/suyu_yuan/article/details/52733117

.tar.gz 压缩格式用于unix的操作系统,但在windows系统中用WinRar工具

同样可以解压缩tar.gz格式的

zip流行于windows系统上的压缩文件(其他系统也可以打开).zip格式开发且

免费.zip支持分卷压缩,128/256-bitAES加密算法等功能.zip的含义是速度,其

目标是为顶替ARC而诞生的"职业"压缩软件.


tar是"table archive"(磁带存档)的简称,它出现在还没有软盘驱动器 硬盘和

光盘驱动器的计算机早期阶段,随着时间的推移,tar命令逐渐变为一个将很多文件

进行存档的工具,目前许多用于Linux操作系统的程序就是打包为tar档案文件的形式

.在Linux里面,tar一般和其他没有文件管理的压缩算法文件结合使用,用tar打包整

个文件目录结构成一个文件,再用gz,bzip等压缩算法压缩成一次,也是Linux常见的

压缩归档的处理方法.

