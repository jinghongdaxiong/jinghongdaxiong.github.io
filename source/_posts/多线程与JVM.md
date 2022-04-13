---
title: 多线程与JVM
date: 2020-06-01 13:16:35
tags: [多线程,JVM]
categories: java
---

# 多线程

## 1、如何在Java中实现线程？
Tips：继承Thread、实现Runnable、实现Callable接口通过FutureTask包装器来创建Thread线程、线
程池。建议看源码消化；

## 2、在具体多线程编程实践中，如何选用Runnable还是Thread？
Tips：接口、类的区别。讲解服务熔断多线程ExceptionRatioDegradeDemo的代码示例。

## 3、Thread类中的start()和run()方法有什么区别？
Tips：start就绪，run运行，掌握Thread的内部类State

## 4、Java中Runnable和Callable有什么不同？
Tips：Runnable和Callable基于接口实现多线程，前者不带返回参数，后者带返回参数

## 5、Java多线程中调用wait() 和 sleep()方法有什么不同？
Tips：Wait、notify一起使用，sleep当前线程睡眠

## 6、什么是Executor框架？
Tips：线程池框架，管理线程的生命周期。 Executor框架包含Executors，ExecutorService，CompletionService，
Future，Callable等 
## 7、在Java中Executor和Executors的区别？
Tips：Executor是多线程自带的框架， Executors是Executor框架的工厂类，通过Executors创建不同类型的线程池
## 8、Java中用到的线程调度算法是什么
Tips：对高优先级，使用优先调度的抢占式策略；同优先级线程组成先进先出队列（先到先服务），使用时间片策
略。
## 9、什么是多线程中的上下文切换？
Tips：CPU通过时间片分配算法来循环执行任务，当前任务执行一个时间片后会切换到下一个任务。但是，在切换
前会保存上一个任务的状态，以便下次切换回这个任务时，可以再加载这个任务的状态。所以任务从保存到再加
载的过程就是一次上下文切换。
## 10、什么是线程安全？
Tips：多线程访问，有锁保护，不会出现数据不一致、数据污染等情况

## 11、请说说有哪些线程不安全的java类？
Tips： ArrayList(非)、Vector；HashMap(非)、HashTable
## 12、Java中如何获取到线程dump文件？
Tips：jvm常见命令。 jmap -dump:format=b,file=文件名 [pid]。参考：
https://www.cnblogs.com/0616--ataozhijia/p/4136312.html

## 13、池技术有什么作用，常见的池技术有哪些？
tips：复用对象，节省创建、销毁资源的时间，提升性能
## 14、请谈谈线程池的使用场景
tips：大量线程使用的场景，且线程执行的时间较短，特别耗时的操作会导致大量线程阻塞，甚
至导致系统宕机
## 15、用线程池有什么好处？
tips：线程的复用，节省线程的创建、销毁时间，提升性能。参考jason java多线程.ppt

## 16、线程池的技术原理是什么？
Tips：读ThreadPoolExecutor源码，了解execute 方法，掌握线程池的原理示意图。
可参考： https://www.cnblogs.com/cm4j/p/thread-pool.html
## 17、线程池有哪些种类，各自的使用场景是什么？
tips：掌握excutors，读源码。参考jason java多线程.ppt
## 18、线程池有哪些重要的参数？
Tips：掌握ThreadPoolExecutor， corePoolSize、maximumPoolSize、keepAliveTime
## 19、你们在具体的设计开发过程中是如何设置这些重要参数的？
Tips： corePoolSize、maximumPoolSize和cpu、内存有关系，考虑到一定的冗余，宁可小不可大。
## 20、单例的使用场景是什么，如何实现单例？
Tips：高性能、节省重量级操作的资源、唯一实例，手写

## 21、如何在Java中创建线程安全的Singleton？
Tips：注意线程安全条件下的单例写法
## 22、synchronzied关键词的使用
Tips：悲观锁，保证线程安全。参考： https://blog.csdn.net/aa792978017/article/details/88835512
## 23、ReentrantLock和synchronized使用的场景是什么，机制有何不同？
Tips：二者都是锁， ReentrantLock基于CAS的乐观锁， synchronized是悲观锁。建议读
ReentrantLock源码，理解记忆。演示Counter7。
## 24、什么是ThreadLocal变量？
Tips： ThreadLocal用于保存某个线程共享变量。不同线程只能从中get，set，remove自己的变量，
而不会影响其他线程的变量。讲解ThreadLocal的set、get、remove方法。
## 25、ThreadLocal技术原理是什么，它在架构中常常用来做什么？
tips： 读ThreadLocal ，理解ThreadLocalMap。 参考jason 16、ThreadLocal原理及在多层架构中的应
                                                 用

## 26、volatile适用于高并发的什么场景？
Tips：轻量级锁，状态位。讲解服务熔断多线程ExceptionCountDegradeDemo的代码示例。
## 27、CountDownLatch用于多线程的什么场景？
Tips:线程计数、线程等待。建议阅读CountDownLatch，讲解Counter1示例.
## 28、java多线程有哪些常见的锁，各自用法是什么？
tips：至少有五种， volatile 、synchronized 、 ReentrantLock 、 Condition、 ReadWriteLock
## 29、多线程join方法用于什么场景？
Tips:控制线程顺序，讲解Thread的join源码。讲解JoinTest示例
## 30、java多线程中让所有子线程执行完毕的方法有哪几种？
tips：至少有两种。线程的sleep、join。

## 31、高并发环境下的计数器如何实现？
Tips：至少有6种实现方式。讲解counter包下的代码示例。复习 CountDownLatch、AtomicInteger、
synchronized、ReentrantLock、线程状态。
## 32、HashTable、HashMap、ConcurrentHashMap各自的技术原理和使用场景是什么？
Tips： 建议看源码。HashTable线程安全、悲观锁，锁整个hash表的数据，效率低；ConcurrentHashMap线程
安全、乐观锁，分段锁； HashMap非线程安全； 可参考：https://www.cnblogs.com/zq-boke/p/8654539.html
## 33、LinkedBlockingQueue、ConcurrentLinkedQueue各自技术原理和使用场景是什么？
Tips： 前者：阻塞队列，用于生产消费者模式； 后者：并发队列，用于高并发场景。
## 34、 Java中如何停止一个线程？
Tips：stop（不推荐）、状态位、interrupt。讲解ThreadStop1、 ThreadStop2。
## 35、Java中Semaphore是什么？
Tips：信号量，用于访问限制可以访问某些资源（物理或逻辑的）线程数目。讲解SemaphoreTest。

## 36、java多线程中有哪些并发流量控制工具类？
tips：至少有三种。 CountDownLatch、Semaphore 、CyclicBarrier
## 37、高并发场景下，如何理解每一个线程执行的逻辑耗时不能过长？
Tips：如果每一个线程执行的逻辑耗时过长，会导致大量线程阻塞，性能急剧下降，系统可用性
存在风险，存在宕机的可能性。
## 38、什么是线程非安全？
Tips：不提供数据访问保护，多个线程写数据造成所得到的数据是脏数据。
## 39、在微服务的分布式架构中，设置服务的超时时间有什么好处？
Tips：防止大量线程阻塞导致系统宕机。
## 40、常见的多线程数据结构有哪些，你用过其中的哪些多线程数据结构？
tips：建议多读concurrent包下的源码。 ConcurrentHashMap、LinkedBlockingQueue、
ConcurrentLinkedQueue、Semaphore等

## 41、多线程的常见设计模式，你用过其中的哪些设计模式
tips：掌握三种最常见的多线程设计模式。
## 42、什么是Master-Worker模式？如何实现Master-Worker模式？
Tips：大任务的并发分解、结果合并。掌握Master-Worker 设计模式原理图。讲解PlusWorker、
Master相关代码。
## 43、什么是Producer-Consumer模式？如何实现Producer-Consumer模式？
Tips：利用缓冲区对生产者、消费者解耦。掌握生产消费者原理图。讲解Producer、 Consumer
## 44、什么是Future模式？如何实现Future模式？
Tips：适合异步耗时的场景。阅读并掌握Excutors、ExecutorService、FutureTask。
## 45、多线程使用场景是什么？
Tips：并发场景、”小逻辑”、性能提升。

## 46、多线程有什么优缺点？
Tips：优点：提升性能；缺点：门槛高，特别是锁，滥用线程会产生死锁、影响性能甚至宕机，
线程切换耗性能；
## 47、假设某系统的某个接口的峰值TPS为2w/s(其它接口的并发峰值至多为200每秒)，且该接口会
保存数据至数据库，如何提升该接口的性能？
Tips：线程池、多线程、分页、批处理。讲解thunder中间件的ThunderEngine源码。
## 48、是否熟悉java concurrent包的内容，请讲讲concurrent包有哪些重要的内容？
Tips：建议多阅读java concurrent包的内容。
## 49、请讲讲并发编程的CAS理论
tips： Compare And Swap、乐观锁机制、jdk的Unsafe类执行这些操作、Doug Lea、 concurrent 包
的重要理论基石。
## 50、请讲讲并发队列和阻塞队列
tips： ConcurrentLinkedQueue、 LinkedBlockingQueue。掌握原理。

## 51、多线程yield方法使用于什么场景？
Tips： yield：让步，线程等待。
## 52、请讲讲线程异步处理的原理及关键组件？
Tips：异步耗时的操作。读源码并掌握Excutors、ExecutorService、FutureTask。
## 53、在实际项目（产品）研发过程中，你是否有使用过多线程，和线程池，如果有，请举例说明
Tips：通用常见的技术场景：批量数据的处理。建议用STAR模型回答。
## 54、什么是多线程的原子操作？
Tips：基于cas的最基本的操作。阅读AtomicInteger的代码头说明并讲解AtomicInteger的CAS机制。
## 55、Java 中有哪些原子操作？
Tips： AtomicInteger、 AtomicLong、AtomicBoolean、 AtomicIntegerArray等等

## 56、多线程原子操作类使用场景是什么，你在项目的实际研发过程中是否有使用
过原子操作类？
Tips：并发计数，比如：微服务场景下的服务监控的统计。
## 57、如何在多个线程间共享数据？
Tips：多个线程之间传参，共享变量；或者内部类；运行MultyThreadShareDateTest。
## 58、线程的状态有哪些，线程状态的使用场景是什么？
Tips：建议阅读Thread类及其内部类State。讲解woker-master的Master类
## 59、有多个线程T1，T2，T3，怎么确保它们按顺序执行？
tips：参考JoinTest
## 60、wait/notify/notifyAll一般使用于什么场景？
Tips：悲观锁，和synchronize关键字联合使用，不建议使用，编程复杂。

# JVM复习题

## 1、请说说jvm的基本结构
Tips：建议掌握jvm的基本结构图。java类加载器、方法区、堆、直接内存、java栈、本地方法栈、
PC寄存器、执行引擎。参考jason的JVM基础知识及性能调优.ppt。 
## 2、什么是JVM ？
Tips：Java Virtual Machine。Java应用和操作系统之间的桥梁。

## 3、堆空间的结构
Tips:Eden、S0、S1、tenured(Old Gerneration)。记忆技巧：按照时间来分。参考jason的JVM基础
知识及性能调优.ppt。 

## 4、Java中堆和栈有什么区别？
Tips：堆主要用于管理对象，栈主要用来管理方法区相关的局部变量。参考：
https://www.cnblogs.com/ibelieve618/p/6380328.html
## 5、为何新生代要设置两个survivor区，jvm的设计上有何目的？
Tips:复制算法

## 6、垃圾回收中的复制算法适用于在什么场景下使用？
Tips:年轻代的垃圾回收。参考jason的JVM基础知识及性能调优.ppt。 
## 7、老年代的垃圾回收一般用什么算法？
Tips:标记清除算法、标记压缩算法。参考jason的JVM基础知识及性能调优.ppt。 
## 8、 java方法栈和本地方法栈的区别？
Tips: 前者是java方法的调用，后者是java调用native方法，比如操作系统、dll等相关的c语
言的方法。
## 9、GC回收机制？
Tips:垃圾回收算法、垃圾收集器。
## 10、top、jmap、jstat、jstack命令各自有什么用途？
Tips:top系统整体资源使用情况，jmap导出堆到文件，jstat查看jvm运行情况，jstack导出 线程堆栈到文件。


## 11、有哪些常见的jvm命令，说说各自的用途是什么？
Tips:掌握jstat、jmap、jstack、jinfo等jvm命令。
## 12、GC有哪些算法。
Tips:复制算法、标记算法、标记压缩算法、分代算法。
## 13、什么是线程中断。
Tips: stop the world，简称STW，垃圾回收的停顿，参考 billy 1.GC算法与种类
## 14、MGC、FGC分别是什么意思，它们在什么情况下会发生？
Tips：年轻代垃圾回收、老年代垃圾回收。参考jason JVM基础知识及性能调优
## 15、请讲讲jvm的分代，为什么要分代，jvm分代有什么好处？
Tips：根据对象的实际情况采用不同的分代。掌握分代的临界值变量。参考jason 
JVM基础知识及性能调优.PPT

## 16、你知道哪些jvm调优工具么？
Tips：VisualVM、Jconsole、MAT。参考性能监控工具.ppt
## 17、在jvm中，年轻代如何向老年代转变的，转换的重要参数是什么?
Tips：年龄累加、 MaxTenuringThreshold。参考jason JVM基础知识及性能调优.PPT
## 18、直接内存使用场景是什么，使用直接内存可能会存在什么问题？
Tips：java原生、netty、mina等相关的NIO操作。参考jason JVM基础知识及性能调优.PPT
## 19、堆内存有哪些重要参数？
Tips: -Xms(初始堆大小)、 -Xmx(最大堆大小)
## 20、如何设置堆大小，是否有一些经验值？
Tips:堆内存至少可以设置为整个内存的一半大小，甚至2/3大小。

## 21、如何打印JVM日志？
Tips: -XX:+PrintGC、-XX:+PrintGCDetails、-XX:+PrintGCTimeStamps、-XX:+PrintGCDateStamps。演示
DirectBufferOOM
## 22、请介绍常见的jvm参数
Tips：堆内存参数、年轻代参数、日志参数、直接内存参数等等。参考jason JVM基础知识及性能
调优.PPT 
## 23、CMS收集器有什么特点？
Tips:老年代、并发收集、低停顿。
## 24、G1收集器有什么特点？
Tips:年轻代和老年代，最新的垃圾回收算法，较综合。
## 25、垃圾回收器有哪些？
Tips: Serial、Parallel 、CMS、G1。参考jason JVM基础知识及性能调优.PPT

## 26、java内存模型
Tips:多个线程通信、同步、happens-before原则(volatile、join)。参考：
https://www.cnblogs.com/yuanfy008/p/9252555.html
## 27、什么是类加载器，类加载器有哪些，类加载器的加载顺序是什么？
Tips：将类的.class文件中的二进制数据读入到内存。三种类加载器：BootstrapClassLoader->ExtClassLoader-> 
AppClassLoader ，演示ClassLoaderTest，熟悉rt.jar的Launcher、Classloader。参考：
https://www.cnblogs.com/heyanan/p/6123279.html
## 28、简述java内存分配与回收策略
Tips：年轻代，老年代，小对象，大对象。 参考：https://segmentfault.com/a/1190000014944731
## 29、Perm Space中保存什么数据？会引起OutOfMemory吗？
Tips:类加载数据。永久代内存过小，会导致OOM。
## 30、是否有做过jvm参数方面的调优，如果有，请举例说明。
Tips：最好用star面试模型。设置堆内存参数、直接内存参数、eclipse、tomcat的相关的jvm参数等等。

## 31、内存溢出的根本原因是什么，该如何解决？
Tips:jvm的相关资源不够用，导致内存溢出。解决程序错误、调整参数、分布式架构(大数据、微服务架构，
本质都是多台机器分布式计算或者处理相关程序逻辑)。
## 32、简述java类加载机制
Tips：熟悉Launcher 、ClassLoader源码理解记忆。
## 33、GC收集器有哪些？CMS收集器与G1收集器的特点
Tips:从内存年代、回收算法、线程数等角度整体理解记忆。参考：
https://blog.csdn.net/qq_35503221/article/details/80313129
## 34、类加载器双亲委派模型机制是什么？
Tips：熟悉ClassLoader的loadClass() 。 参考：https://blog.csdn.net/weixin_38118016/article/details/79579657
## 35、什么情况下会出现永久代内存溢出，如何解决此类问题？
Tips: jar包过多、加载大量class文件，永久代内存参数过小。增加JVM的PermSize和MaxPermSize参数大小。
演示PermTest、PermTest2、 PermTest3。

## 36、什么情况下会出现堆内存溢出，如何解决此类问题？
Tips:参考jason JVM基础知识及性能调优.PPT。演示 HeapOOMTest。
## 37、什么情况下会出现直接内存溢出，如何解决此类问题？
Tips:参考jason JVM基础知识及性能调优.PPT。演示 DirectBufferOOM 。
## 38、什么情况下会出现过多线程导致内存溢出的问题，如何解决此类问题？
Tips:参考jason JVM基础知识及性能调优.PPT。
## 39、什么情况下会出现CPU使用率过高的问题，如何解决此类问题？
Tips:参考jason JVM基础知识及性能调优.PPT。
## 40、OOM有哪些可能，应该如何处理。
Tips：该问题比较综合，参考Jason JVM基础知识及性能调优.PPT