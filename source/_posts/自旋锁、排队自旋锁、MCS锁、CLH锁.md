---
title: 自旋锁、排队自旋锁、MCS锁、CLH锁
date: 2020-05-28 20:50:24
tags: [java,锁]
categories: 
---

# 自旋锁
# what
当锁被其他线程占用时，自己不断的循环检测锁是否释放，不改变线程状态
（即不挂起线程或睡眠状态），需占用CPU资源，适用于临界区很小的情况。
若线程竞争激烈，则会消耗大量CPU资源，不适用于自旋锁。
# why
线程的状态改变是很消耗CPU资源的，所以有了不改变线程状态的自旋锁。
适用于锁竞争不是很激烈的情况
# how
talk is cheap show me the code
    
    import java.util.concurrent.atomic.AtomicReference;
    
    public class SpinLock {
       private AtomicReference<Thread> owner = new AtomicReference<Thread>();
    
       public void lock() {
           Thread currentThread = Thread.currentThread();
    
                  // 如果锁未被占用，则设置当前线程为锁的拥有者
           while (!owner.compareAndSet(null, currentThread)) {
           }
       }
    
       public void unlock() {
           Thread currentThread = Thread.currentThread();
    
                  // 只有锁的拥有者才能释放锁
           owner.compareAndSet(currentThread, null);
       }
       
SimpleSpinLock里有一个owner属性持有锁当前拥有者的线程的引用，如果该引用为null，则表示锁未被占用，不为null则被占用。

这里用AtomicReference是为了使用它的原子性的compareAndSet方法（CAS操作），解决了多线程并发操作导致数据不一致的问题，确保其他线程可以看到锁的真实状态。

缺点
CAS操作需要硬件的配合；
保证各个CPU的缓存（L1、L2、L3、跨CPU Socket、主存）的数据一致性，通讯开销很大，在多处理器系统上更严重；
没法保证公平性，不保证等待进程/线程按照FIFO顺序获得锁。       
    

# 排队自旋锁（Ticket Lock）
# what

# why
# how

# MCS锁
# what
# why
# how

# CLH锁
# what
# why
# how

