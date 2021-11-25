---
title: Mac配置多个Java版本
date: 2019-08-28 17:58:34
tags: [java jdk]
---

## 一. 安装最新版的Java

    # 如何没有安装 brew cask。请执行    $ brew tap caskroom/versions
    $ brew cask install java
    
## 二.安装其他版本的Java

    如果你需要安装其他的jdk(JDK 7 或者 JDK 6)，可以使用homebrew-cask-versions：
    $ brew search java
    $ brew cask install java6      # 使用cask安装其他的工具

## 三.查看本地安装的Java Home

    $ /usr/libexec/java_home -V #查看本地安装的java版本

## 四.切换java版本【手动修改环境变量】

    那问题来了，当你运行java或者 Java 程序时使用的是哪个 JDK 呢？在 OS X 下，java也就是/usr/bin/java在默认情况下指向的是已经安装的最新版本。但是你可以设置环境变量JAVA_HOME来更改其指向：
    
    # 查看当前的java版本
    $ java -version          
    java version "1.8.0_60"
    Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
    Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)
    
    # 切换版本，可切换为第三步的本地java home中的任意一个。
    $ export JAVA_HOME=/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home java -version  
    java version "1.6.0_65"
    Java(TM) SE Runtime Environment (build 1.6.0_65-b14-466.1-11M4716)
    Java HotSpot(TM) 64-Bit Server VM (build 20.65-b04-466.1, mixed mode)

## 五.配置命令自动切换

修改系统环境变量：

在~/.bash_profile（如果是Zsh，修改~/.zshrc）文件中添加如下内容：
    
    # JDK 6  
    export JAVA_6_HOME="/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home"
    # JDK 8
    export JAVA_8_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_101.jdk/Contents/Home"
    
    export JAVA_HOME=$JAVA_8_HOME #默认JDK 8
    
    #alias命令动态切换JDK版本  
    alias jdk6="export JAVA_HOME=$JAVA_6_HOME"    
    alias jdk8="export JAVA_HOME=$JAVA_8_HOME"  

更新配置：

    $ source ~/.bash_profile #Zsh应改为 source ~/.zshrc

切换java版本：

    $ jdk6    #使用jdk6
    $ java -version 
        java version "1.6.0_65"
        Java(TM) SE Runtime Environment (build 1.6.0_65-b14-468)
        Java HotSpot(TM) 64-Bit Server VM (build 20.65-b04-468, mixed mode)
    
    $ jdk8    #使用jdk8
    $ java -version 
        java version "1.8.0_101"
        Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
        Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)
        
    说明：Mac系统的环境变量，加载顺序为：
    /etc/profile /etc/paths ~/.bash_profile ~/.bash_login ~/.profile ~/.bashrc

[参考1](https://segmentfault.com/a/1190000013131276)            
[参考2](https://www.kancloud.cn/kancloud/ocds-guide-to-setting-up-mac)