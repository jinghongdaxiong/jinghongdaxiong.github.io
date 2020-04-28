---
title: mysql之锁
date: 2020-04-28 09:03:29
tags: [mysql]
categories: mysql
---
[摘自](https://xzh20121116.github.io/post/mysql-zhi-suo/)

# 表级锁
## 表锁 
    加锁 lock tablas ...read/write
    主动解锁 unlock tables ... 
    被动解锁 客户端断开连接时被动解锁
    限制其他线程读写，也会对本线程接下来的操作对象。
    一般不用，消耗大
## MDL（元数据锁 meta data lock）
    自动加锁
    读锁 对表数据的增删改查
    写锁 对表结构的修改
    多个事务读锁不互斥
    多个事务写锁互斥
    mdl作用保证读写的正确性

# 行级锁
根据数据引擎来实现的，MyIsam就不支持行锁
## 两阶段锁
    innodb中有需要时加此锁，但并非不需要时解锁，而是在事务
    提交之后解锁。
    
    若一个事务锁多行，尽可能把并发多的行往后写。
## 死锁
    事务a等待事务b释放某行的锁，事务b等待事务a释放某行的锁就造成死锁。
    例如：
    事务a update table a ... where line = 1;
    事务b update table a ... where line = 2;
    事务a update table a ... where line = 2;
    事务b update table a ... where line = 1;
    
    事务a等待事务b释放line=2的锁，事务b等待事务a释放line=1的锁，由于
    两阶段锁的存在（即事务只有在commit之后才释放锁）顾造成锁都释放
    不了，造成死锁。
    
## mvcc(多版本并发控制)
    mysql在修改一行数据时，都会记录一行此数据相应的回滚操作，
    若某行数据被改了n次，则此事务相应的回滚记录也被记录了n次。
    这种一条数据在系统中有多个版本就是多版本并发控制mvcc.
    
# 脑图
 ![](mysql之锁.png) 
