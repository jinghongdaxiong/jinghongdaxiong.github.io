---
title: jdk8更换
date: 2021-09-27 10:09:58
tags: [java]
categories: [java]
---


## 最新免费版jdk
[最新免费版下载地址](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)

## Linux系统更换jdk
* 查看Linux内核版本
````
第一种方式
uname -srm
输出结果：

Linux 3.10.0-957.12.2.el7.x86_64 x86_64
3 - 内核版本.
10 - 主修订版本.
0-957 - 次要修订版本.
12 - 补丁版本.

第二种方式
使用`hostnamectl`命令查看内核版本
hostnamectl实用程序是systemd的一部分，用于查询和更改系统主机名。 它还显示Linux发行版和内核版本：

root@pre-k8s-node1 ~]# hostnamectl
   Static hostname: pre-k8s-node1
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 8f5b018f6eb344909f6cfec5ad0839ef
           Boot ID: 89c4766582c24bb8b9bfa1300c74d151
    Virtualization: kvm
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-1062.12.1.el7.x86_64
      Architecture: x86-64
      
第三种方式      
使用以下命令只显示当前内核版本：

[root@pre-k8s-node1 ~]# hostnamectl | grep -i kernel
            Kernel: Linux 3.10.0-1062.12.1.el7.x86_64
            
第四种方式      
通过查看/proc/version文件确认内核版本
/proc目录包含虚拟文件，其中包含有关系统内存，CPU内核，已安装文件系统等的信息。有关正在运行的内核的信息存储在/proc/version虚拟文件中。
结合cat查看文件内容：

cat /proc/version
输出结果如下：

Linux version 3.10.0-957.12.2.el7.x86_64 (mockbuild@kbuilder.bsys.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-36) (GCC) ) 1 SMP Tue May 14 21:24:32 UTC 2019            
      
这些命令适用于所有流行的Linux发行版，包括Debian，Red Hat，Ubuntu，Arch Linux，Fedora，CentOS，Kali Linux，OpenSUSE，Linux Mint等。      

````
  
* 从[最新免费版下载地址](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)
获取最新版本。

* 依据Linux内核确定 版本为jdk-8u202-linux-x64.tar.gz

* 通过FinalShell将解压后的文件上传至/usr/local/java/

* 配置环境变量vim /etc/profile
````
export JAVA_HOME=/usr/local/java/jdk1.8.0_202
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
````

* 刷新配置
source /etc/profile
  
* 查看java的安装目录
which java

## docker环境更换jdk

## 公告文档
Hello开发小伙伴们：

   因Oracle JDK许可变更，在没有商业许可的情况下，在2019年1月之后发布的Oracle Java SE 8的公开更新将不可用于
商业或生产用途。

   经架构委员会、运维部会议讨论，目前Oracle JDK 8u202已经满足公司当前开发需求。如后期有更高版本的使用需求，须
经架构委员会评估后，另行通知方可使用。

   从即日起全公司使用的Oracle JDK版本不得高于8u202,请各业务线上技术经理知悉并安排自检，同时通知研发人员不得使用
更高版本JDK进行开发，以防新特性无法上线。

## 参考文档
 [Oracle JDK究竟从哪个版本开始收费？](https://www.cnblogs.com/xuruiming/p/12881503.html)

 [linux下解决安装jdk后‘环境变量’不生效的问题](https://www.shuzhiduo.com/A/kvJ34GYw5g/)

 [Linux安装JDK，并配置多个JDK切换](https://lenjor.github.io/2020/12/Linux-Java-JDK-install/)

 [Linux下安装JDK(多个版本) 切换](https://blog.csdn.net/jxpxlinkui/article/details/79649204)

 [linux 卸载jdk和安装](https://www.cnblogs.com/javabg/p/10332993.html)

 [JDK Linux修改jdk不生效](https://blog.csdn.net/ca1m0921/article/details/90322309)

 [Linux 给文件夹或者文件增加权限](https://blog.csdn.net/zhangvalue/article/details/84979635)

````
chmod -R 777 文件夹
参数-R是递归的意思
777表示开放所有权限

chmod 777 test.sh

chmod +x 某文件

如果给所有人添加可执行权限：chmod a+x 文件名；
如果给文件所有者添加可执行权限：chmod u+x 文件名；
如果给所在组添加可执行权限：chmod g+x 文件名；
如果给所在组以外的人添加可执行权限：chmod o+x 文件名；
````
 [查看Linux内核版本](https://www.cnblogs.com/linuxprobe/p/11664104.html)
