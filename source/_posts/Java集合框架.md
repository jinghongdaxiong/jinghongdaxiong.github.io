---
title: Java集合框架
date: 2020-04-11 08:26:51
tags: [java,集合，面试]
categories: java
---
# Java集合框架

## Collection
  ### List
    ArrayList
        排列有序，可重复
        底层使用数组
        速度快，增删慢，getter()和setter()快
        线程不安全
        当容量不够，ArrayList是当前容量*1.5+1
    Vector
        排列有序，可重复
        底层使用数组
        速度快，增删慢
        线程安全，效率低
        当容量不够，默认扩张一倍容量
    LinkedList
        排列有序，可重复
        底层使用双向循环链表数据结构
        查询速度慢，增删快，add（）和remove（）方法快
        线程不安全
  ### Set
    HashSet
        排列无序，不可重复
        底层使用hash表实现
        存取速度快
        内部是HashMap
    TreeSet
        排列无序，不可重复
        底层使用二叉树实现
        排序存储
        内部是TreeMap的SortedSet
    LinkedHashSet
        采用Hash表存储，并用双向链表记录插入顺序
        内部是LinkedHashMap
  ### Queue
    在两端出入的List，所以也可以用数组或链表来实现
## Map
  ### HashMap
    键不可重复，值可重复
    底层hash表
    线程不安全
    允许key值为null，值为null
  ### HashTable
    键不可重复，值可重复
    底层Hash表
    线程安全
    键和值都不可为null
  ### TreeMap
    键不可重复，值可重复
    底层二叉树
# 思维导图
![](Java集合框架.png)