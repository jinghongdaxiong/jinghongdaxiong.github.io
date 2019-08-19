---
title: Kotlin
date: 2018-04-20 17:34:49
tags:[kotlin]
---

## 引用: https://baike.baidu.com/item/Kotlin/1133714?fr=aladdin

    Kotlin 是一个用于现代多平台应用的静态编程语言,有JetBrains开
    发.
    
    Kotlin可以编译称为Java字节码,也可以编译成JavaScript,方便在没有
    JVM的设备上运行.
    
    Kotlin已正式成为Android官方支持开发语言.
    
## 简介

    JetBrains,作为广受欢迎的java IDE intelliJ的提供商,在Apache许可
    下已经开源其Kotlin编程语言.
    
## 设计目标
    
    创建一种兼容java的语言
    
    让它比java更安全,能够静态检测常见的陷阱.如:引用空指针
    让它比java更简洁,通过支持variable type inference,higher-order
    function(closures),extension functions,mixins and first-class
    delegation等实现.
    
    让它比最成熟的竞争对手Scala语言更加简洁.
    
## 开发源代码

    其基础编译器(他们将其改为kompiler---开创了一系列以K字打头的用语
    ---甚至连 contributors这类词他们也用改成了kontributors)可以被独立
    出来并嵌入到Maven Ant或Gradle工具链中,这使得在IDE中开发的代码能够
    利用已有的机制来构建,从而尽可能的减少了在新环境中使用所受的干预,哪
    怕与那些没有安装Kotlin插件的开发人员一起合作项目也没有问题.
    
    The Intellij Kotlin插件扩展了Java编译器使得Kotlin代码能够得以编写 
    编译和调试.除此之外,关于基本的Java集合,已经有编写好的帮助函数,可以
    更顺畅地衔接在java8中出现的集合扩展.
    
    有两篇文章对Kotlin与Java以及Scala分别进行了比较,对各自特性和异同进
    行了对比.即便Scala可能还是更为强大些,Kotlin还是尝试提供比java更好
    的函数 模式匹配 空指针预防和泛型.该语言同时支持特征(traits)和模式匹配
    
    Kotlin插件在当前版本的IntelliJ和Eclipse中均已能使用.
    
    Kotlin,类似Xtend一样,旨在提供一种更好的java而非重建整个新平台.这两种
    语言都向下编译为字节码(虽然Xtend是首先转换成相应的java代码,再让java
    编译器完成繁重的工作),而且两者都引入了函数和扩展函数(在某个有限范围
    内静态地增加一个新方法到某个已有类型的能力).Xtend是基于Eclipse的,而
    Kotlin是基于IntelliJ的,两者都提供无界面构建.能够首先演变到其他IDE的语
    言有可能成为最后的赢家.
    
    
    
    
    
    

