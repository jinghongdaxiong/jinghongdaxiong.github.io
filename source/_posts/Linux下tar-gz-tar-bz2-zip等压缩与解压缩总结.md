---
title: 'Linux下tar.gz,tar,bz2,zip等压缩与解压缩总结'
date: 2018-04-28 09:42:39
tags:
---

## tar命令基本用法:

tar命令的选项有很多(用man tar可以查看到),常用的就下面几个

* tar -cf all.tar *.jpg

    这条命令是将所有.jpg的文件打成一个名为all.tar的包.-c是表示

    生成新的包,-f指定包的文件名.

* tar -rf all.tar *.gif
    
    这条命令是将所有.gif的文件增加到all.tar的包里面去.-r是表示增
    
    加文件的意思.
    
* tar -uf all.tar logo.gif

    更新原来all.tar中logo.gif文件,-u是表示更新文件的意思
    
* tar -tf all.tar    
    
    列出all.tar中的所有文件,-u表示更新文件的意思
    
* tar -xf all.tar    

    解出all.tar中的所有文件,-x是解开文件的意思
    
以上就是tar的最基本的用法,为了方便用户在打包解包的同时可以压缩或解压
文件,tar提供了一种特殊的功能.就是tar可以在打包或解包的同时调用其他的
压缩程序,比如调用gzip bzip2等.

## 1)tar调用gzip

gzip是GNU组织开发的一个压缩程序,.gz结尾的文件是gzip压缩的结果.与gzip相对

的解压程序是gunzip.tar中使用-z这个参数来调用gzip

* # tar -czf all.tar.gz *.jpg  

将所有.jpg的文件打成一个tar包,并将其用gzip压缩,生成一个gzip压缩过的包,
包名为all.tar.gz

* # tar -xzf all.tar.gz

解压包


## 2) tar调用bzip2

bzip2是一个压缩能力更强的压缩程序,.bz2结尾的文件是bzip压缩的结果.

与bzip相对的解压程序是bunzip2.tar中使用-j这个参数来调用gzip.

* # tar -cjf all.tar.bz2 *.jpg

这条命令是将所有.jpg的文件打成一个tar包,并且将其用bzip2压缩,生成一个
bzip2相对的压缩


    
    
    
    

    
    
