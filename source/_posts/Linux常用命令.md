---
title: Linux常用命令
date: 2018-07-27 11:41:49
tags: [Linux,命令]
categories: 
- linux
---
# 日常操作命令

## 查看当前所在的工作目录的全路径pwd

    [root@localhost ~]# pwd
    /root
    
## 查看当前系统的时间date

    [root@localhost ~]# date +%Y-%m-%d
    
    date +%Y-%m-%d --date="-1 day" #加减也可以 month | year
    
    date -s "2016-07-28 16:12:00" ## 修改时间
    
## 查看有谁在线(哪些人登陆了服务器)    

    who 查看当前在线
    
    last 查看最近的登陆历史记录
    
## 关机/重启

    关机(必须用root用户)
    shutdown -h now ## 立刻关机
    shutdown -h +10 ## 10分钟以后关机
    shutdown -h 12:00:00 ## 12点整的时候关机
    halt #  等于立刻关机
    
    重启
    shutdown -r now
    reboot # 等于立刻重启
    
## 清屏

    clear ## 或者用快捷键 Ctrl + 1
    
## 退出当前进程
    Ctrl + c  ##有些程序也可以用q键退出
    
## 挂起当前进程
    Ctrl + z ## 进程会挂起到后台
    bg jobid ## 进程在后台继续执行
    fg jobid ## 让进程回到前台
            
## echo
    相当于Java中System.out.println(userName)
    a="test"
    echo a ## a
    echo $a ## test
                
# 目录操作
                
## 查看目录信息
    ls / ## 查看根目录下的子节点(文件夹和文件)信息
    ls -al ## -a是显示隐藏文件 -l是以更详细的列表形式显示
    ls -l ## 有一个别名: ll 可以直接使用ll<是两个L>
                    
## 切换工作目录
    cd ~ ##切换都用户主目录
    cd - ##切换上次所在的目录
    cd 什么都不带,则回到用户的主目录
    
## 创建文件夹
    mkdir aaa ## 这是相对路径的写法
    mkdir /data ## 这是绝对路径的写法
    mkdir -p aaa/bbb/ccc ## 级联创建目录
## 删除文件夹
    rmdir aaa ##可以删除空目录
    rm -r aaa ## 可以把aaa整个文件夹及其中的所有子节点全部删除
    rm -rf aaa ## 强制删除aaa
            
## 修改文件夹名称
    mv aaa boy
    mv本质上是移动
    mv install.log aaa/ 将当前目录下的install.log移动到aaa文件夹中去
    
    rename 可以用来批量更改文件名
    [root@localhost aaa]# ll
    total 0
    -rw-r--r--. 1 root root 0 Jul 28 17:33 1.txt
    -rw-r--r--. 1 root root 0 Jul 28 17:33 2.txt
    -rw-r--r--. 1 root root 0 Jul 28 17:33 3.txt
    [root@localhost aaa]# rename .txt .txt.bak *
    [root@localhost aaa]# ll
    total 0
    -rw-r--r--. 1 root root 0 Jul 28 17:33 1.txt.bak
    -rw-r--r--. 1 root root 0 Jul 28 17:33 2.txt.bak
    -rw-r--r--. 1 root root 0 Jul 28 17:33 3.txt.bak
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                
                    
    