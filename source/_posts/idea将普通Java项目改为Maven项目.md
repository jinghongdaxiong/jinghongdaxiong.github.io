---
title: idea将普通Java项目改为Maven项目
date: 2019-09-04 10:05:05
tags: [idea,java,maven]
categories: [java]
---

# 方式 1

1 在项目右键 Add Framework Support

2 选择maven

# 方式2

1.右键工程文件，新建文件pom.xml，并填写好内容。

2.在pom.xml 文件上右键 Add as Maven Project。

3.idea自己导入maven。


# 怎么在pom.xml中添加项目中libs下的jar呢，而不是从本地仓库中添加？

1、首先将要添加的jar包复制到项目中的libs文件夹下

2、然后在pom.xml中添加如下代码：

		<dependency>
			<groupId>htmlunit</groupId>
			<artifactId>htmlunit</artifactId>
			<version>2.21-OSGi</version>
			<scope>system</scope>
			<systemPath>${project.basedir}/libs/htmlunit-2.21-OSGi.jar</systemPath>
		</dependency>
注意scope元素和systemPath元素，其中systemPath元素指定的就是jar包在项目中的路径。
注意libs文件夹下的这个jar包不需要Add to Build Path

[参考1](https://blog.csdn.net/zhengxiangwen/article/details/50734565)
[参考2](https://blog.csdn.net/lcgoing/article/details/86928738)
