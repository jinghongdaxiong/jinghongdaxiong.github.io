---
title: tomcat9因为在清除过期缓存条目后可用空间仍不足 - 请考虑增加缓存的最大空间。
date: 2019-08-29 09:55:24
tags: [java tomcat]
---
## 问题
因为在清除过期缓存条目后可用空间仍不足 - 请考虑增加缓存的最大空间。

    # 进入Tomcat安装位置
    $CATALINA_HOME/
    $ vim ../libexec/conf/context.xml
    
    将下面代码添加到 <Context> </Context>中
    <Resources
             cachingAllowed="true"
             cacheMaxSize="100000"
     	/>
