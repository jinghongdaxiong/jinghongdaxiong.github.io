---
title: 'gpg是什么  '
date: 2019-09-28 20:41:03
tags: [gpg]
categories: [gpg]
---

# GPG简介

　　GPG是GNU Privacy Guard的缩写，它是一种基于密钥的加密方式，使用了一对密钥对消息进行加密和解密，来保证消息的安全传输。
　　GPG有许多用途，主要用于文件加密。yum安装软件包的时候会使用gpg来验证。

## 1、大多数的linux发行版都默认包含了gpg
   
    # gpg --version
    
## 2、gpg常用命令
   
    创建密钥 $ gpg --gen-key
    
    查看公钥 $ gpg --list-key
    
    查看私钥 $ gpg --list-secret-key
    
    公钥删除 $ gpg --delete-keys 标识名
    
    私钥删除 $ gpg --delete-secret-keys 标识名
    
    公钥导出 $ gpg --export 标识名 > 导出文件名（多以gpg,asc为文件后缀）
    
    私钥导出 $ gpg --export-secret-key 标识名 > 导出文件名（多以asc为文件后缀）
    
    密钥导入 $ gpg --import 密钥文件
    
    加密文件 $ gpg --recipient 标识名 --encrypt 文件名
    
    解密文件 $ gpg --output 新文件名 --decrypt 加密文件名
    
    修改密钥 $ gpg --edit-key 标识名
         
##     3、gpg加密和ssh加密的区别
       
ssh加密是专们为远程登录和其他网络服务，如ftp 提供安全的一个软件
gpg是用来加密文件的


