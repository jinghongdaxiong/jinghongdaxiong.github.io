---
title: ' java.io.EOFException'
date: 2019-09-06 10:45:51
tags: [java,异常,spring]
categories: [exception]
---

Caused by: java.io.EOFException: Can not read response from server. Expected to read 4 bytes, read 0 bytes before connection was unexpectedly lost.

启动ssm项目报上述错误,查了一堆答案

有说改 jdbc配置的,有说改数据库的
顺便还改了下数据库
    
    show variables like  'wait_timeout';
    
    show global variables like  '%wait%';        
    
    set wait_timeout=86400;
    
问题都没有解决

最终解决方案:

可能是防火墙,翻墙软件等造成的

    
