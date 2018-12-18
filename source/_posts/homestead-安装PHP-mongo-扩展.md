---
title: homestead 安装PHP mongo 扩展
date: 2018-12-17 19:51:52
tags:
---


# 解决痛点: homestead多个PHP版本,某个版本安装mongo扩展

## 1. 进入虚拟机

    vagrant ssh
    
## 2. 切换到管理员

    sudo su     
    
## 3. 查看PHP版本路径 

    // 选择你要安装的PHP版本(我是PHP5.6)
    php --ini 
    或
    find / -name php.ini
    返回
    /etc/php/7.1/cli/php.ini
    /etc/php/7.1/fpm/php.ini
    /etc/php/7.2/cli/php.ini
    /etc/php/7.2/fpm/php.ini
    /etc/php/7.0/cli/php.ini
    /etc/php/7.0/fpm/php.ini
    /etc/php/5.6/cli/php.ini
    /etc/php/5.6/fpm/php.ini
    
## 4. 进入工作目录(这个随意,我习惯进入/etc/php/5.6)    

    cd /etc/php/5.6
    
## 5.下载php mongo扩展  

    git clone https://github.com/mongodb/mongo-php-driver-legacy.git
    
## 6. 进入下载的目录

    cd mongo-php-driver-legacy/
    
## 7. 选择对应的扩展

    // 参考文档
    https://docs.mongodb.com/ecosystem/drivers/php/#drivers
    
    git branch -a
    
    git checkout v1.6
    
## 8. 编译PHP的mongo扩展

    //（不同php版本的情况下phpize版本不同）
    /usr/bin/phpize5.6 
     
    //（这里边也需要根据情况指定php-config的版本，且和phpize的版本保持一致。）
    ./configure --with-php-config=/usr/bin/php-config5.6 

    make && make install 

*  编译完成后，mongo的php扩展在module目录中，它的文件名是mongo.so*

## 9. 查看php的extension_dir

    /usr/bin/php5.6 -i|grep extension_dir
    
    返回 extension_dir => /usr/lib/php/20131226 => /usr/lib/php/20131226
    
* 这说明php的扩展目录是/usr/lib/php/20131226
* 或者你用phpinfo()输出一个页面，在里面找extension_dir也可以找到*
    
## 10. 把mongo.so扩展模块移入php扩展目录中 

    mv ./module/mongo.so /usr/lib/php/20131226  
    
* 注意，前提要求当前工作目录是在刚才编译的mongo-php-driver-legacy目录中

## 11. 添加php配置文件的ini文件

    sudo touch /etc/php/5.6/fpm/conf.d/20-mongo.ini
    
## 12. 使用vi编辑器写入如下内容

    vim /etc/php/5.6/fpm/conf.d/20-mongo.ini
    
    extension=mongo.so
    
* 记得使用vi编辑器时使用：wq命令保存

## 13. 重启PHP

    service php5.6-fpm restart
    
    


      

    
    