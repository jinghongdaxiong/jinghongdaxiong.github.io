---
title: vim教程
date: 2019-06-11 09:43:44
tags: vim
categories: 
- linux
---
## linux下 输入命令 vimtutor 基本练习

## vim使用的区域(块)选择
    
    ctrl+v 块选择
    
## 字符选择    

    v 小写的v字符选择
    
## 行选择    

    shift+v 大写V行选择
    
## vim的模式    
    
    esc  进入普通模式
    
    shift + : 进入命令模式
    
    普通模式下 a 在光标尾插入
    普通模式下 i 在光标首插入

    vim包括一般模式,插入模式,命令模式,区域选择在一般模式下,
    选择的区域包括固定黑色,闪动黑色,闪动黑色表示光标位置.
    在区域选择的情况下,d删除选择的区域,y复制选择的区域,p
    粘贴选择的区域,小写p在当前行的下一行粘贴.大写P在当前行
    的上一行粘贴.
    
## 在当前行首添加字符ddd    
    
    命令模式下
    .s/^/ddd/g   
    
## 在第三行到第六行添加字符ddd    

    3,6s/^/ddd/g
    
    
## 在当前行尾添加字符ddd    
    
    .s/$/ddd/g
    

