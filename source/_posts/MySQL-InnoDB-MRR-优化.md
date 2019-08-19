---
title: 'MySQL InnoDB MRR 优化  '
date: 2019-08-19 16:37:07
tags:
---

MRR 是 Multi-Range Read 的简写，目的是减少磁盘随机访问，将随机访问转化为较为顺序的访问。适用于 range/ref/eq_ref 类型的查询。

## 实现原理：

1. 在二级索引查找后，根据得到的主键到聚簇索引找出需要的数据。

2. 二级索引查找得到的主键的顺序是不确定的，因为二级索引的顺序与聚簇索引的顺序不一定一致；

3. 如果没有 MRR，那么在聚簇索引查找时就可能出现乱序读取数据页，这对于机械硬盘是及其不友好的。

4. MRR 的优化方式：
   
   * 将查找到的二级索引键值放在一个缓存中；
   * 将缓存中的键值按照 主键 进行排序；
   * 根据排序后的主键去聚簇索引访问实际的数据文件。

5. 当优化器使用了 MRR 时，执行计划的 Extra 列会出现 “Using MRR” 。

6. 如果查询使用的二级索引的顺序本身与结果集的顺序一致，那么使用 MRR 后需要对得到的结果集进行排序。


## 如何使用
使用 MRR 还可以减少缓冲池中页被替换的次数，批量处理对键值的查询操作。

可以使用命令 select @@optimizer_switch; 查看是否开启了 MRR：

    index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=off,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=off,skip_scan=on

mrr_cost_based=on 表示是否通过 cost based 的方式来选择使用 MRR 。

用 set @@optimizer_switch='mrr=on/off'; 命令开启或关闭 MRR 。

select @@read_rnd_buffer_size ; 参数用来控制键值的缓冲区大小，默认 256K，当大于该参数值时，执行器根据主键对已缓存的数据进行排序，然后再通过主键取得行数据。



