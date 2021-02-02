# 代码审查标准及规范

## git 提交标准

* mvn test 通过

* 代码格式化通过 (ctrl + alt + l) 

* 阿里规约扫描无警告

## git 提交前要做的

    0、 git pull 
    
    1、 执行代码格式化快捷键 reformat code 
    
    2、 在项目根目录执行 mvn test 
    
    3、阿里规约扫描
    
## git提交参考脚本   

    #!bin/bash
    
    if [ -z "$1" ]
    then
    	echo "提交信息不可为空"
        exit;
    fi
    
    git add -A
    
    git commit -m "$1"
    
    git pull
    
    # 测试
    mvn clean test
    
    git push
    
    git checkout debugging
    
    git pull
    
    git merge V1.0.0 -m "$1"
    
    mvn test
    
    git push
    
    git checkout V1.0.0
    
## [代码规范](http://showdoc.huifanayb.cn:4999/web/#/p/9c0423d7da951ea5923f8f381c63a368)

## [前后端规约](http://showdoc.huifanayb.cn:4999/web/#/p/6a57dc10a462d2a8a5cbac0bf402f777)

## [命名风格](http://showdoc.huifanayb.cn:4999/web/#/p/5247ce95619e723c005451cf0bfcaf6d)

## [代码格式配置及使用方法]( http://showdoc.huifanayb.cn:4999/web/#/p/38fef09fc67bec02f52fdc107cde88d7)

## 规范特别提示
* idea中不可出现爆红    

* 代码格式需标准统一

* 代码中不可出现无用引用包

* 阿里规约不可扫描出现问题

## 审查方式

1、项目技术负责人负责自己所属项目代码审查

2、代码审查总负责人负责抽查项目

3、项目总负责人每周汇总每个人的代码不规范之处代码及数量
    
