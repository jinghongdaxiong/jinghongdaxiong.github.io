---
title: flyway
date: 2022-03-30 10:44:42
tags: [SQL，版本管理]
categories: [SQL]
---

## 是什么

数据库版本管理工具

[官网](https://flywaydb.org/)

## 为什么

* 自己写的SQL忘了在所有环境执行
* 别人写的SQL我们不能确定是否都在所有环境执行了
* 有人修改了已经执行过的SQL，期望再次执行
* 需要新增环境做数据迁移
* 每次发版需要手动控制先发DB版本，再发布应用版本

## 怎么做

集成依赖
````
        <dependency>
            <groupId>org.flywaydb</groupId>
            <artifactId>flyway-core</artifactId>
        </dependency>

````

在项目resources中添加目录

````
cd 你的目录/resources
$ mkdir -p db/migration

````

在项目配置中添加flyway配置
````
spring:
  flyway:
    enabled: true
    # 禁止清理数据库表
    clean-disabled: true
    # 如果数据库不是空表，需要设置成 true，否则启动报错
    baseline-on-migrate: true
    # 与 baseline-on-migrate: true 搭配使用
    baseline-version: 0

````



### 命名规范

示例：

````
V1.1.__description.sql
  
R__description.sql
````

prefix：可配置，前缀标识，默认值 V 表示 Versioned，R 表示 Repeatable；

Version：标识版本号，由一个或多个数字构成，数字之间的分隔符可用点.或单下划线_；

separator：分隔符，默认是双下划线；

description：描述信息，文字之间可以用单下划线或空格分隔

suffix：可配置，后续标识，后续标识，默认为 .sql；

Versioned migration 用于版本升级，每个版本都有唯一的版本号并只能 apply 依次。（也就是V开头的）

Repeatable migration 是指可重复加载的 migration，可以重复修改内容使用一旦脚本的 checkksum 有变动，flyway 就会重新应用该脚本，它并不用于版本更新，这类的 migration 总是在 versioned migration 执行之后才被执行（R开头的）

### spring配置

````
flyway:
  #开启
  enabled: true
  #当迁移时发现目标schema非空，而且带有没有元数据的表时，是否自动执行基准迁移，默认false.
  baseline-on-migrate: true
  # 检测迁移脚本的路径是否存在，如不存在，则抛出异常
  check-location: true
  #sql脚本位置
  locations: classpath:db/migration
  #是否允许无序的迁移，默认false
  out-of-order: false
  #编码
  encoding: UTF-8
  
  
  
# flyway.baseline-description对执行迁移时基准版本的描述.
# flyway.baseline-on-migrate当迁移时发现目标schema非空，而且带有没有元数据的表时，是否自动执行基准迁移，默认false.
# flyway.baseline-version开始执行基准迁移时对现有的schema的版本打标签，默认值为1.
# flyway.check-location检查迁移脚本的位置是否存在，默认false.
# flyway.clean-on-validation-error当发现校验错误时是否自动调用clean，默认false.
# flyway.enabled是否开启flywary，默认true.
# flyway.encoding设置迁移时的编码，默认UTF-8.
# flyway.ignore-failed-future-migration当读取元数据表时是否忽略错误的迁移，默认false.
# flyway.init-sqls当初始化好连接时要执行的SQL.
# flyway.locations迁移脚本的位置，默认db/migration.
# flyway.out-of-order是否允许无序的迁移，默认false.
# flyway.password目标数据库的密码.
# flyway.placeholder-prefix设置每个placeholder的前缀，默认${.
# flyway.placeholder-replacementplaceholders是否要被替换，默认true.
# flyway.placeholder-suffix设置每个placeholder的后缀，默认}.
# flyway.placeholders.[placeholder name]设置placeholder的value
# flyway.schemas设定需要flywary迁移的schema，大小写敏感，默认为连接默认的schema.
# flyway.sql-migration-prefix迁移文件的前缀，默认为V.
# flyway.sql-migration-separator迁移脚本的文件名分隔符，默认__
# flyway.sql-migration-suffix迁移脚本的后缀，默认为.sql
# flyway.tableflyway使用的元数据表名，默认为schema_version
# flyway.target迁移时使用的目标版本，默认为latest version
# flyway.url迁移时使用的JDBC URL，如果没有指定的话，将使用配置的主数据源
# flyway.user迁移数据库的用户名
# flyway.validate-on-migrate迁移时是否校验，默认为true.

````

## 坑

* 坑一：命名SQL文件问题

  没有按标准命名，导致执行不成功
* 坑二：执行失败的文件，再次启动时执行不了，可以删除数据库中`flyway_schema_history`表最后一条数据，然后再重启

* 坑三：云家园装配后不生效，但maven可以生效，目前还在解决中。


## 竞品对比


nextep(开源)
http://www.nextep-softwares.com/

dbdeploy(开源)
http://dbdeploy.com/

**Liquibase**(开源)
http://www.liquibase.org/

Post Facto(开源)
http://www.post-facto.org/


下面是red gate公司的商业软件

SQL Source Control (商业软件)
http://www.red-gate.com/products/SQL_Source_Control/index.htm


## 参考文档
* [官网](https://flywaydb.org)
* "https://www.jianshu.com/p/567a8a161641"
* "https://blog.csdn.net/chenleiking/article/details/80691750"
* "https://segmentfault.com/a/1190000038960756"
* "https://juejin.cn/post/6844903802215071758"
* "https://blog.csdn.net/qq_33430083/article/details/105508025"
* **"https://blog.51cto.com/u_15076233/4273411"**
* **"https://www.saoniuhuo.com/article/detail-30226.html"**
* [**这个用的比较新的flyway**](https://blog.51cto.com/javastack/2967527)
* [知乎的这篇原理介绍还可以](https://zhuanlan.zhihu.com/p/63513168)
* [知乎的这篇讨论也有点意思，尤其是其中的其他链接](https://www.zhihu.com/question/41782437/answer/1242972612)
* [这是要删库跑路的节奏啊](https://zhuanlan.zhihu.com/p/51170727)
* [这篇讲的实战中k8s结合flyway很高大上](https://zhuanlan.zhihu.com/p/334650852)
* [这篇中idea新建项目及sql文件命名讲的不错](https://blog.csdn.net/zxd1435513775/article/details/104392489)
* [这就是我遇到的一个坑，自己创建映射目录时没注意](https://stackoverflow.com/questions/55328817/flyway-cant-find-classpathdb-migrations)
* [这篇值得深入研究**比较数据版本控制工具-2020**](https://www.diglog.com/story/1032583.html)
* [几个数据库的版本控制工具](https://www.cnblogs.com/harrychinese/archive/2011/03/20/some_database_version_control_tool.html)
* [数据库版本控制工具介绍](https://www.shangmayuan.com/a/54e27b8f31f740a9a95de09c.html)
