---
title: Spring Boot 核心注解
date: 2019-09-25 14:00:45
tags: [Spring,java]
categories:[Java]
---

#问题：

我看你上面写了熟悉 Spring Boot，那你能讲下为什么我们要用 Spring Boot 吗？

#下面我列几个最常见的三个回答：

## A：Spring Boot 最主要是不用 XML 配置，可以用 Java 来配置 bean，省去了许多配置文件。

我又问：Spring 本身就可以用 Java 配置代替 XML 配置，和 Spring Boot 有什么关系呢？

然后对方就吱吱唔唔了……

## B：Spring Boot 我们用来做 Spring Cloud 微服务。

我又问：微服务和 Spring Boot 有什么关系？不用 Spring Boot 行不行？

然后对方就吱吱唔唔了……

## C：Spring Boot 可以打 jar 包部署，内部集成了Tomcat。

这个确实是 Spring Boot 的特色，但是我还是觉得没有答到关键点上。

然后我继续问，如果不考虑打 jar 包部署呢，然后就没然后了……

# 为什么我们要用 Spring Boot，显然上面三个求职者没有答到关键点上，Spring Boot 最重要的功能是：自动配置。

## 为什么说是自动配置？

Spring Boot 的开启注解是：@SpringBootApplication，其实它就是由下面三个注解组成的：

@Configuration
@ComponentScan
@EnableAutoConfiguration
上面三个注解，前面两个都是 Spring 自带的，和 Spring Boot 无关，所以说上面的回答的不是在点上。

所以说 Spring Boot 最最核心的就是这个 **@EnableAutoConfiguration** 注解了，它能根据类路径下的 jar 包和配置动态加载配置和注入bean。

举个例子，比如我在 lib 下放一个 druid 连接池的 jar 包，然后在 application.yml 文件配置 druid 相关的参数，Spring Boot 就能够自动配置所有我们需要的东西，如果我把 jar 包拿掉或者把参数去掉，那 Spring Boot 就不会自动配置。

这样我们就能把许多功能做成公共的自动配置的启动器（starters），其实 druid 连接池就是这么做的，它提供了针对 Spring Boot 的启动器：druid-spring-boot-starter。

有了这个自动配置的启动器，我们就能非常简单的使用它，

先添加 jar 包依赖：

    <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid-spring-boot-starter</artifactId>
       <version>1.1.10</version>
    </dependency>    

再添加相关参数：

    spring.datasource.url= 
    spring.datasource.username=
    spring.datasource.password=
    ……
如果是传统的项目，我们要自己手动写一大堆的配置，而且还不灵活，有了这个启动器，我们就可以做到简单集成。具体大家可以看 druid-spring-boot-starter 是怎么实现的。

所以，这才是 Spring Boot 的核心，这才是我们为什么使用 Spring Boot 的原因。如果答不到这个关键点，那真没有掌握到 Spring Boot 的核心所在。
