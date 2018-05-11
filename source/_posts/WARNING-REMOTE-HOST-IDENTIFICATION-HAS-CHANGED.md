---
title: ' WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! '
date: 2018-05-11 11:36:59
tags:
---

## 参考: https://blog.csdn.net/nahancy/article/details/51052127

## 问题

On branch master
nothing to commit, working tree clean
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the DSA key sent by the remote host is
SHA256:br9IjFspm1vxR3iA35FWE+4VTyz1hYVLIE2t1/CeyWQ.
Please contact your system administrator.
Add correct host key in /Users/xiongzixu/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/xiongzixu/.ssh/known_hosts:1
DSA host key for github.com has changed and you have requested strict checking.
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.


## 原因:
找了好久发现有篇文章里面提到.ssh/known_hosts文件,原来known_hosts是记录远程主机的公钥的文件，
之前更新了系统，而保存的公钥还是未重装系统的系统公钥，在ssh链接的时候首先会验证公钥，如果公钥不对，
那么就会报错




## 解决方案(3种):

* 1: 使用shh-keygen 命令（强烈建议使用此方法）
       
     比如我们要将172.16.152.209的公钥信息清除，使用命令（请自己将172.16.152.209替换成自己的IP或域名）：
     
* 2: 将known_hosts文件中的与登录错误的IP的公钥删除即可，下图就是我的218机子的公钥（实则是之前系统的公钥），然后将其删除，再ssh 登录 great 登录成功了。     
       

![](https://img-blog.csdn.net/20160403214246747?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


* 3: 将known_hosts文件中的内容清空即可，但不建议使用此方法，里面还保存有其他机子的公钥。 
     


