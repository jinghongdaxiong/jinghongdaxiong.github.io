---
title: 深入浅出Java多线程
date: 2020-04-03 19:46:41
tags: [java,多线程]
categories: [java]
---

# 基础篇

##  进程与线程的基本概念
### 进程产生的背景
指令---》指令集（批处理） 

缺点：串行，效率低

**进程：** 应用程序在内存中分配的空间，正在运行的程序。

 进程优点： 操作系统并发成为可能。
 
**线程:** 一个线程执行一个子任务，一个进程包含多个线程。

优点：让进程的内部并发成为了可能。 

 **多进程的方式也可以实现并发，为什么我们要使用多线程？**

* 进程通信复杂，线程间可共享资源，通信较线程容易;
* 进程重量级，线程轻量级，系统开销小。

**进程线程的区别？**
* 本质区别：能否单独占有内存空间及其他系统资源（如：I/O);
* 进程是操作系统进行资源分配的基本单位，而线程是操作系统进行系统调度的基本单位，即CPU时间的分配单位。

### 上下文切换 
**上下文：** 某一时间点CPU寄存器和程序计数器的内容。

**上下文切换:** CPU从一个进程/线程切换到另一个进程/线程。

**缺点：** 上先文切换是计算密集型的，消耗大量的CPU时间，故线程也不是越多越好。

##  java多线程入门类和接口

### Thread类和Runnable接口 
**Java如何实现多线程？**

* 继承Thread类，并重写run()方法。
* 实现Runnable接口的run()方法。

**继承Thread类需注意什么？**

thread.start()方法后，该线程才算启动，调用start()后，虚拟机会先为我们创建一个线程，然后等到这个线程第一次得到时间片时，再调用run()方法。start()不可多次调用，否则抛异常。

**Java8函数式编程**

    new Thread(()->{System.out.println("java8");}).start();
    
**Thread类的构造方法**

    Thread(Runnable target)
    Thread(Runnable target, String name)
    
**Thread类的几个常用方法**

    currentThread(); // 静态方法，返回正在执行线程的引用。
    start(); // 启动线程。
    yield(); // 让出当前处理器的占用。
    sleep(); // 静态方法，休眠。
    join(); // 当前线程等待另一个线程执行完毕后再继续执行。内部调用的Object.wait()。
    
**Thread类与Runnable比较？**
* Java单继承多实现，Runnable比Thread灵活；
* Runnable更符合面向对象，将线程单独进行对象的封装;
* Runnable降低了线程对象和线程任务的耦合性；
* 若不用Thread类诸多方法，Runnable更轻量级，适合实现多线程。

### Callable、Future与FutureTask

**为啥用Callable Future FutureTask?**
因为Runnable和Thread创建的线程没有返回值。当我们希望开启一个线程执行完一个任务后有返回值则用以上方式（异步模型）。

**Callable特点？**
* 有返回值，支持泛型

**Callable咋用？**

    伪代码 
    ExecutorService.submit(Callable) return一个Future，
    通过Future的get方法获取结果。
    
**Future接口注意项**
* cancel() 试图取消，并不一定取消成功。

**FutureTask类总结**
* Future类的实现类。
* FutureTask实现了RunnableFuture接口，RunnableFuture同时继承了Runnable接口和Future接口。

**为什么要用FutureTask?**
高并发下，Callable和FutureTask会创建多次。FutureTask能确保任务只执行一次。

**FutureTask有几个状态?分别是？及其转换关系？**
六个状态分别是：
* NEW = 0 // 新建
* COMPLETING = 1 // 完成 
* NORMAL = 2 // 正常
* EXCEPTIONAL = 3 // 异常
* CANCELLED = 4  // 取消
* INTERRUPTING = 5 // 打断中
* INTERRUPTED = 6 // 打断了的


    转变路径
    0 -> 1 -> 2
    0 -> 1 -> 3
    0 -> 4
    0 -> 5 -> 6
    
    
##  线程组和线程优先级

### 线程组（ThreadGroup）
**线程组作用？**
****
****
****
****

##  java线程的状态和主要转化方法
##  Java线程间的通信

# Java内存模型基础知识

##  Java内存模型基础知识
##  重排序与happens-before
##  volatile
##  synchronized与锁
##  CAS与原子操作
##  AQS

# JDK工具 

## 线程池原理 
## 阻塞队列
## 锁接口和类
## 并发集合容器简介
## CopyOnWrite
## 通信工具类
## Fork/Join框架
## Java8 Stream并行计算原理
## 计划任务
