---
title: java 面试
date: 2020-02-01 23:52:28
tags: [面试，Java]
categories: 面试
---
[摘自](https://www.javanav.com/interview/93b0069472fd479393006c0e73043fc4.html)
# 基础与语法

## 1 jdk jre 区别
JRE：Java Runtime Environment（ java 运行时环境）。即java程序的运行时环境，包含了 java 虚拟机，java基础类库。

JDK：Java Development Kit（ java 开发工具包）。即java语言编写的程序所需的开发工具包。JDK 包含了 JRE，同时还包括 java 源码的编译器 javac、监控工具 jconsole、分析工具 jvisualvm等。

## 2 ==和equals的区别是什么?
 == 的作用：
 * 基本类型：比较值是否相等
 * 引用类型：比较内存地址值是否相等
 
 equals() 的作用:
 引用类型：默认情况下比较的是内存地址是否相等。可以按照需求逻辑，重写对象的equals（）方法（重写 equals() 方法，一般须重写 hashCode() 方法）。

## 3 基本类型和包装类对象使用 == 和 equals进行比较的结果？
1、值不同，使用 ＝＝ 和 equals() 比较都返回 false

2、值相同

使用 ＝＝ 比较：

* 基本类型 － 基本类型、基本类型 － 包装对象返回 true
* 包装对象 － 包装对象返回 false
* 缓存中取的包装对象比较返回 true（原因是 JVM 缓存部分基本类型常用的包装类对象，如 Integer -128 ~ 127 是被缓存的）
 
 
      Integer i1 = 100;
      Integer i2 = 100;
      Integer i3 = 200;
      Integer i4 = 200;
              
      System.out.println(i1==i2); //打印true
      System.out.println(i3==i4); //打印false 
使用 equals() 比较

* 包装对象－基本类型返回 true
* 包装对象－包装对象返回 true

3、不同类型的对象对比，返回 false

JDK1.8，实验代码

    byte b1 = 127;
    Byte b2 = new Byte("127");
    Byte b3 = new Byte("127");
    System.out.println("Byte 基本类型和包装对象使用 == 比较 : " + (b1 == b2));
    System.out.println("Byte 基本类型和包装对象使用 equals 比较 : " + b2.equals(b1));
    System.out.println("Byte 包装对象和包装对象使用 == 比较 : " + (b2 == b3));
    System.out.println("Byte 包装对象和包装对象使用 equals 比较 : " + b2.equals(b3));
    System.out.println();
    
    short s1 = 12;
    Short s2 = new Short("12");
    Short s3 = new Short("12");
    System.out.println("Short 基本类型和包装对象使用 == 比较 : " + (s1 == s2));
    System.out.println("Short 基本类型和包装对象使用 equals 比较 : " + s2.equals(s1));
    System.out.println("Short 包装对象和包装对象使用 == 比较 : " + (s2 == s3));
    System.out.println("Short 包装对象和包装对象使用 equals 比较 : " + s2.equals(s3));
    System.out.println();
    
    char c1 = 'A';
    Character c2 = new Character('A');
    Character c3 = new Character('A');
    System.out.println("Character 基本类型和包装对象使用 == 比较 : " + (c1 == c2));
    System.out.println("Character 基本类型和包装对象使用 equals 比较 : " + c2.equals(c1));
    System.out.println("Character 包装对象和包装对象使用 == 比较 : " + (c2 == c3));
    System.out.println("Character 包装对象和包装对象使用 equals 比较 : " + c2.equals(c3));
    System.out.println();
    
    int i1 = 10000;
    Integer i2 = new Integer(10000);
    Integer i3 = new Integer(10000);
    System.out.println("Integer 基本类型和包装对象使用 == 比较 : " + (i1 == i2));
    System.out.println("Integer 基本类型和包装对象使用 equals 比较 : " + i2.equals(i1));
    System.out.println("Integer 包装对象和包装对象使用 == 比较 : " + (i2 == i3));
    System.out.println("Integer 包装对象和包装对象使用 equals 比较 : " + i2.equals(i3));
    System.out.println();
    
    long l1 = 1000000000000000L;
    Long l2 = new Long("1000000000000000");
    Long l3 = new Long("1000000000000000");
    System.out.println("Long 基本类型和包装对象使用 == 比较 : " + (l1 == l2));
    System.out.println("Long 基本类型和包装对象使用 equals 比较 : " + l2.equals(l1));
    System.out.println("Long 包装对象和包装对象使用 == 比较 : " + (l2 == l3));
    System.out.println("Long 包装对象和包装对象使用 equals 比较 : " + l2.equals(l3));
    System.out.println();
    
    float f1 = 10000.111F;
    Float f2 = new Float("10000.111");
    Float f3 = new Float("10000.111");
    System.out.println("Float 基本类型和包装对象使用 == 比较 : " + (f1 == f2));
    System.out.println("Float 基本类型和包装对象使用 equals 比较 : " + f2.equals(f1));
    System.out.println("Float 包装对象和包装对象使用 == 比较 : " + (f2 == f3));
    System.out.println("Float 包装对象和包装对象使用 equals 比较 : " + f2.equals(f3));
    System.out.println();
    
    double d1 = 10000.111;
    Double d2 = new Double("10000.111");
    Double d3 = new Double("10000.111");
    System.out.println("Double 基本类型和包装对象使用 == 比较 : " + (d1 == d2));
    System.out.println("Double 基本类型和包装对象使用 equals 比较 : " + d2.equals(d1));
    System.out.println("Double 包装对象和包装对象使用 == 比较 : " + (d2 == d3));
    System.out.println("Double 包装对象和包装对象使用 equals 比较 : " + d2.equals(d3));
    System.out.println();
    
    boolean bl1 = true;
    Boolean bl2 = new Boolean("true");
    Boolean bl3 = new Boolean("true");
    System.out.println("Boolean 基本类型和包装对象使用 == 比较 : " + (bl1 == bl2));
    System.out.println("Boolean 基本类型和包装对象使用 equals 比较 : " + bl2.equals(bl1));
    System.out.println("Boolean 包装对象和包装对象使用 == 比较 : " + (bl2 == bl3));
    System.out.println("Boolean 包装对象和包装对象使用 equals 比较 : " + bl2.equals(bl3));
    
运行结果

    Byte 基本类型和包装对象使用 == 比较 : true
    Byte 基本类型和包装对象使用 equals 比较 : true
    Byte 包装对象和包装对象使用 == 比较 : false
    Byte 包装对象和包装对象使用 equals 比较 : true
     
    Short 基本类型和包装对象使用 == 比较 : true
    Short 基本类型和包装对象使用 equals 比较 : true
    Short 包装对象和包装对象使用 == 比较 : false
    Short 包装对象和包装对象使用 equals 比较 : true
     
    Character 基本类型和包装对象使用 == 比较 : true
    Character 基本类型和包装对象使用 equals 比较 : true
    Character 包装对象和包装对象使用 == 比较 : false
    Character 包装对象和包装对象使用 equals 比较 : true
     
    Integer 基本类型和包装对象使用 == 比较 : true
    Integer 基本类型和包装对象使用 equals 比较 : true
    Integer 包装对象和包装对象使用 == 比较 : false
    Integer 包装对象和包装对象使用 equals 比较 : true
     
    Long 基本类型和包装对象使用 == 比较 : true
    Long 基本类型和包装对象使用 equals 比较 : true
    Long 包装对象和包装对象使用 == 比较 : false
    Long 包装对象和包装对象使用 equals 比较 : true
     
    Float 基本类型和包装对象使用 == 比较 : true
    Float 基本类型和包装对象使用 equals 比较 : true
    Float 包装对象和包装对象使用 == 比较 : false
    Float 包装对象和包装对象使用 equals 比较 : true
     
    Double 基本类型和包装对象使用 == 比较 : true
    Double 基本类型和包装对象使用 equals 比较 : true
    Double 包装对象和包装对象使用 == 比较 : false
    Double 包装对象和包装对象使用 equals 比较 : true
     
    Boolean 基本类型和包装对象使用 == 比较 : true
    Boolean 基本类型和包装对象使用 equals 比较 : true
    Boolean 包装对象和包装对象使用 == 比较 : false
    Boolean 包装对象和包装对象使用 equals 比较 : true
    
ps：可以延伸一个问题，基本类型与包装对象的拆/装箱的过程

## 4 什么是装箱？什么是拆箱？装箱和拆箱的执行过程？常见问题？ 
1、什么是装箱？什么是拆箱？
装箱：基本类型转变为包装器类型的过程。
拆箱：包装器类型转变为基本类型的过程。

    //JDK1.5之前是不支持自动装箱和自动拆箱的，定义Integer对象，必须
    Integer i = new Integer(8);
    
    //JDK1.5开始，提供了自动装箱的功能，定义Integer对象可以这样
    Integer i = 8;
    int n = i;//自动拆箱
2、装箱和拆箱的执行过程？
* 装箱是通过调用包装器类的 valueOf 方法实现的
* 拆箱是通过调用包装器类的 xxxValue 方法实现的，xxx代表对应的基本数据类型。
* 如int装箱的时候自动调用Integer的valueOf(int)方法；Integer拆箱的时候自动调用Integer的intValue方法。

3、常见问题？

* 整型的包装类 valueOf 方法返回对象时，在常用的取值范围内，会返回缓存对象。
* 浮点型的包装类 valueOf 方法返回新的对象。
* 布尔型的包装类 valueOf 方法 Boolean类的静态常量 TRUE | FALSE。

实验代码

    Integer i1 = 100;
    Integer i2 = 100;
    Integer i3 = 200;
    Integer i4 = 200;
    System.out.println(i1 == i2);//true
    System.out.println(i3 == i4);//false
            
    Double d1 = 100.0;
    Double d2 = 100.0;
    Double d3 = 200.0;
    Double d4 = 200.0;
    System.out.println(d1 == d2);//false
    System.out.println(d3 == d4);//false
            
    Boolean b1 = false;
    Boolean b2 = false;
    Boolean b3 = true;
    Boolean b4 = true;
    System.out.println(b1 == b2);//true
    System.out.println(b3 == b4);//true
* 包含算术运算会触发自动拆箱。
* 存在大量自动装箱的过程，如果装箱返回的包装对象不是从缓存中获取，会创建很多新的对象，比较消耗内存。


    Integer s1 = 0;
    long t1 = System.currentTimeMillis();
    for(int i = 0; i <1000 * 10000; i++){
        s1 += i;
    }
    long t2 = System.currentTimeMillis();
    System.out.println("使用Integer，递增相加耗时：" + (t2 - t1));//使用Integer，递增相加耗时：68
            
    int s2 = 0;
    long t3 = System.currentTimeMillis();
    for(int i = 0; i <1000 * 10000; i++){
        s2 += i;
    }
    long t4 = System.currentTimeMillis();
    System.out.println("使用int" + (t4 - t3));//使用int，递增相加耗时：6
ps：可深入研究一下 javap 命令，看下自动拆箱、装箱后的class文件组成。
       看一下 JDK 中 Byte、Short、Character、Integer、Long、Boolean、Float、Double的 valueOf 和 xxxValue 方法的源码（xxx代表基本类型如intValue）。     
## hashCode()相同，equals()也一定为true吗？
首先，答案肯定是不一定。同时反过来 equals() 为true，hashCode() 也不一定相同。

* 类的 hashCode() 方法和 equals() 方法都可以重写，返回的值完全在于自己定义。
* hashCode() 返回该对象的哈希码值；equals() 返回两个对象是否相等。

关于 hashCode() 和 equals() 是方法是有一些 常规协定 ：

* 1、两个对象用 equals() 比较返回true，那么两个对象的hashCode()方法必须返回相同的结果。
 
* 2、两个对象用 equals() 比较返回false，不要求hashCode()方法也一定返回不同的值，但是最好返回不同值，以提搞哈希表性能。
  
* 3、重写 equals() 方法，必须重写 hashCode() 方法，以保证 equals() 方法相等时两个对象 hashcode() 返回相同的值。
  
## final在java中的作用
final 语义是不可改变的。
* 被 final 修饰的类，不能够被继承。
* 被 final 修饰的成员变量必须要初始化，赋初值后不能再重新赋值(可以调用对象方法修改属性值)。对基本类型来说是其值不可变；对引用变量来说其引用不可变，即不能再指向其他的对象。
* 被 final 修饰的方法代表不能重写。

## final finally finalize()区别
* final 表示最终的、不可改变的。用于修饰类、方法和变量。final 变量必须在声明时给定初值，只能读取，不可修改。final 方法也同样只能使用，不能重写，但能够重载。final 修饰的对象，对象的引用地址不能变，但对象的属性值可以改变

* finally 异常处理的一部分，它只能用在 try/catch 语句中，表示希望 finally 语句块中的代码最后一定被执行（存在一些情况导致 finally 语句块不会被执行，如 jvm 结束）

* finalize() 是在 java.lang.Object 里定义的，Object 的 finalize() 方法什么都不做，对象被回收时 finalize() 方法会被调用。Java 技术允许使用 finalize() 方法在垃圾收集器将对象从内存中清除出去之前做必要清理工作，在垃圾收集器删除对象之前被调用的。一般情况下，此方法由JVM调用。特殊情况下，可重写 finalize() 方法，当对象被回收的时候释放一些资源，须调用 super.finalize() 。 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

# 集合

# 网络编程

# 并发

# web 

# 安全
# 设计模式
# 框架
# 数据结构与算法
# 异常
# 文件解析与生成
# linux
# mysql 

# Oracle

# Redis

# Dubbo
