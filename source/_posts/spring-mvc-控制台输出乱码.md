---
title: spring mvc 控制台输出乱码
date: 2019-09-03 18:18:19
tags: [Spring,Tomcat]
categories: [Tomcat]
---
1、运行环境：
操作系统系统：Mac OS X10.12.6，语言：英文
开发工具：IntelliJ IDEA 2017.2.2，默认编码：UTF-8
Tomcat:9.0.0.M26
2、问题：运行Web项目时，控制台输出乱码。
3、解决方法：
设置Servlet的VM options(虚拟机选项)为：-Dfile.encoding=UTF-8
![](/images/vmset.png)

[来源](https://www.cnblogs.com/gdwkong/p/7457181.html)