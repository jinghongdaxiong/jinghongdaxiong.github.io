---
title: Learn Linux
date: 2018-04-20 11:40:30
tags:
---

# 学习Linux问题与总结

## 1 如何打开Ubuntu命令行工具

   * 按快捷键,Ctrl+Alt+F2/F3/F4/F5/F6,后面的F2到6是或者的意思

   * 然后会进入命令行的登录界面,这时候需要输入用户名和密码.

   * 密码是不可见的,输入后直接回车即可

## 2 如何关闭Ubuntu命令行工具(即切换到桌面)

   * 按快捷键,Ctrl+Alt+F7

## 3 Ubuntu下查看IP

   * ifconfig -a

## 4 使用locale查看系统当前编码

## 5 Ubuntu设置root用户初始密码

### 安装ubuntu成功后,都是普通用户权限,并没有最高root权限
    如果需要root权限的时候,通常都会在命令前面加上sudo.有时
    候感觉很麻烦...
    
    我们一般使用su命令来直接切换到root用户的,但是如果没有设
    置root初始密码,就会抛出su : Authentication failure这样的
    问题.所以我们只要给root用户设置一个初始密码就好了.
    
    输入sudo passwd命令,输入一般用户密码并设定root用户密码.
    设定root密码成功后,输入su命令,并输入刚才设定的root密码,
    就可以切换成root 了.提示符$代表一般用户,提示符#代表root用户
    
   * 总结 sudo passwd 设置初始root用户密码

## 6 查看所有用户组

    $cat /etc/group
    ssl-cert:x:110:postgres
    最前面一个字段ssl-cert是用户组名,最后一个字段postgres是用户名
    
## 7 查看所有用户

    $sudo cat /etc/shadow
    postgres:$6$m8anDHdE$FDY4j0CdAbgeLOM90EH1xCW/IMqHEZwM87sepyHHjUYccdmFOCVaFealGTd2zGBVfDV.AR9CWTlGz0Sw/JivL1:15910:0:99999:7:::  
    postgres是用户名
    
## 8 远程连接Linux(Ubuntu配置SSH服务)端口22
    安装OpenSSH
    Ubuntu缺省没有安装SSH Server,使用一下命令安装:
    
    sudo apt-get install openssh-server openssh-client
    
    不过Ubuntu缺省已经安装了ssh client.
    
    配置完成后重启:
    
    sudo /etc/init.d/ssh restart
    
    windows客户端用putty连接命令shell模式
    
## 9 如何查看Linux系统版本信息

### 查看Linux内核版本命令(两种方式)

   * cat /proc/version 
   
   * uname -a
   
### 查看Linux系统版本命令(3种方式)

   * lsb_release -a 
   
    这个命令适用于所有的Linux发行版,包括ReHat SUSE Debian...等发行版
    
   * cat /etc/redhat-release,
   
    这种方式只适合Redhat系的Linux
    
   * cat /etc/issue
   
    这种方式适用于所有的Linux发行版
    
## 10 ubuntu 安装 上传下载工具lrzsz

    apt-get install lrzsz y
    
## 11 Linux中运行.sh(Shell脚本)文件

    有两种方法:
    
    1 直接./加文件名.sh,如运行hello.sh为./hello.sh[hello.sh必须有x权限]
    
    2 直接sh加上文件名.sh,如运行hello.sh为sh hello.sh[hello.sh]可以没有x权限]
    
    步骤
    
    1 cd到.sh文件所在目录
    
    2 给.sh文件添加x执行权限,已hello.sh文件为例
    chmod u+x hello.sh
    
    3 执行 ./hello.sh 或者 sh hello.sh
    
    备注 绝对路径执行*.sh以hello.sh 为例
    
    ./home/test/shell/hello.sh ,可以这样运行时因为当前登录用户是root,当前路径
    是在/下,.代表当前路径.
    
    /home/test/shll/hello.sh,此路径为真实绝对路径,但此方法运行的条件是该用户对
    hello.sh拥有执行权限,即已执行chmod u+x hello.sh
    
    sh/home/test/shell/hello.sh,用sh命令执行shell脚本不需要该用户拥有hello.sh的执行
    权限即可执行.
    
## 12 zip 或unzip的安装和使用

    Linux系统没有自带的压缩解压缩工具;需要我们自己安装;当用到zip或者unzip如果没有安装
    就会出现unzip:Command Not Found 或 zip:Command Not Found;
    
    1 apt-get安装:
    apt-get install zip
    
    2 yum安装
    yum install -y unzip zip
    
## 13 虚拟机中CentOS无法上网(connect:network is unreachable)

    表现:ping时提示connet network is unreachable
    
    原因: ifconfig发现网卡没有分配IP地址,考虑是DHCP的问题.
    
    临时解决方案: sudo dhclient,发现可以上网,重启又没有IP了,
    
    一劳永逸解决方案: 修改etc目录下网卡配置信息
    vim /etc/sysconfig/network-scripts/ifcfg-[网络设备名]
    发现最后一行的ONBOOT选项竟然是no,将其改为yes,然后就正常了.
    
## 14 重启系统 reboot init 6

## 15 关机
    
    halt 立刻关机
    
    poweroff 立刻关机
    
    shutdown -h now 立刻关机(root用户使用)
    
    shutdown -h 10 10分钟后自动关机
    
    如果是通过shutdown命令设置关机的话,可以用shutdown -c命令取消重启
    
    推荐使用shutdown命令
    
## 16 centOS安装lrzsz
    
    yum install lrzsz
    
    
    
    
    
    
    
    


