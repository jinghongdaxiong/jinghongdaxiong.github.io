---
title: >-
  mac上解决java.rmi.server.ExportException- Port already in use- 1099; nested
  exception is- java.net.Bi
date: 2019-09-09 18:39:14
tags: [mac,exception]
categories: exception
---

tomcat启动报如下的错误：

java.rmi.server.ExportException: Port already in use: 1099; nested exception is: java.net.BindException: Address already in use (Bind failed)

解决方法有两种

# 第一种：换其他的端口再次启动，例如：换1098端口，如果你不想换其他端口，则见第二种方法

# 第二种

第一步：使用lsof -i tcp:1099 查看时那个应用占用了此端口

第二部：使用kill pid 即可，这里的pid是第一步所查询到结果


[原文链接](https://blog.csdn.net/u010412719/article/details/76724125)
