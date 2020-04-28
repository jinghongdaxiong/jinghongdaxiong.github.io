---
title: MySQL · 特性分析 · 到底是谁执行了FTWRL
date: 2020-04-28 11:35:31
tags: [mysql]
categories: mysql
---

# what
    FTWRL是flush table with read lock的简称。
    该命令主要用于保证备份一致性备份 
    全局读锁(lock_global_read_lock) 会导致所有的更新操作被堵塞
    全局COMMIT锁(make_global_read_lock_block_commit) 会导致所有的活跃事务无法提交
    FLUSH TABLES WITH READ LOCK执行后整个系统会一直处于只读状态，直到显示执行UNLOCK TABLES。这点请切记。
    


# how
![](tmp.png)