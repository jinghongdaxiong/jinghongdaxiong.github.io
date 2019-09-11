---
title: "Mac\_下locate命令使用问题WARNING: The locate database (/var/db/locate.database) does not exist"
date: 2019-09-11 09:44:23
tags: [Mac,exception]
categories: [Mac]
---
# 问题
![](locate.png)

根据提示使用 

sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.locate.plist
 并没有生效。

需要执行

sudo /usr/libexec/locate.updatedb 进行库更新。

[参考](https://www.cnblogs.com/b-ruce/p/5911048.html)
