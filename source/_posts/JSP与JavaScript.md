---
title: JSP与JavaScript
date: 2019-09-04 17:50:42
tags: [JSP,JavaScript]
categories: [笑话]
---

某日在洗手间偶遇某大牛,聊起JSP,大牛曰:"JSP 哦知道,JavaScript"
我竟无言以对!在此记录一下JSP与JavaScript的区别

# 首先下结论 雷锋与雷锋塔的区别

## JSP是什么
SUN首先发展出SERVLET，其功能比较强劲，体系设计也很先进，只是，它输出HTML语句还是采用了老的CGI方式，是一句一句输出，所以，编写和修改HTML非常不方便。 后来SUN推出了类似于ASP的镶嵌型的JSP，把JSP TAG镶嵌到HTML语句中，这样，就大大简化和方便了网页的设计和修改。

JSP全名为Java Server Pages，其根本是一个简化的Servlet设计，他实现了Html语法中的java扩张（以 <%, %>形式）。JSP与Servlet一样，是在服务器端执行的。通常返回给客户端的就是一个HTML文本，因此客户端只要有浏览器就能浏览。Web服务器在遇到访问JSP网页的请求时，首先执行其中的程序段，然后将执行结果连同JSP文件中的HTML代码一起返回给客户端。插入的Java程序段可以操作数据库、重新定向网页等，以实现建立动态网页所需要的功能。

JSP页面由HTML代码和嵌入其中的Java代码所组成。服务器在页面被客户端请求以后对这些Java代码进行处理，然后将生成的HTML页面返回给客户端的浏览器。Java Servlet是JSP的技术基础，而且大型的Web应用程序的开发需要Java Servlet和JSP配合才能完成。JSP具备了Java技术的简单易用，完全的面向对象，具有平台无关性且安全可靠，主要面向因特网的所有特点。

jsp 要先翻译，注意是翻译成servlet才能执行：
比如 test.jsp 要变成 test_jsp.java 然后编译成 test_jsp.class
而 test_jsp.java 本身就是一个servlet.
所以 jsp只是servlet的一个变种，方便书写html内容才出现的。
    
    servlet的运行机制和Applet类似，只不过它运行在服务器端。一个servlet是javax.servlet包中HttpServlet类的子类，由支持servlet的服务器完成该子类的对象，即servlet的初始化。
    
    扩展阅读0：jsp转化为servlet的过程：
    
    http://www.w3cschool.cc/jsp/jsp-architecture.html
    
    扩展阅读1：servlet版的Helloworld（需要装tomcat,我通常使用XAMPP集成的tomcat）
    
    http://blog.163.com/adoom_2010/blog/static/1820326362011710102719527/
    
    扩展阅读2：servlet程序中的各部分的作用、调用顺序
    
    http://wenku.baidu.com/link?url=U2B6Gx_C1X702ppIFJdXR23MyY85lZzJeneIDZSFCuA3bZ-ynwDFx9oYm4pNcpa4ZjmlUPnkrtwkHg0skxdo3mqOY-IAvXzzYqaCOc7DVmW
    
## JavaScript是什么
Java Script 是一种基于对象的客户端脚本语言。主要目的是为了解决服务器端语言，比如Perl，遗留的速度问题，为客户提供更流畅的浏览效果。JS可以直接嵌入到html代码中进行解析执行，非常简单易学，可以产生很多动态的效果。

## 二者区别
简单地说——JS是在客户端执行的，需要浏览器支持Javascript。JSP是在服务器端执行的，需要服务器上部署支持Servlet的服务器程序。JS代码是能够直接从服务器上download得到，对外是可见的，jsp(和翻译后的servlet)代码是对外不可见的。

JS与JavaScript相比：虽然JavaScript可以在客户端动态生成HTML，但是很难与服务器交互，因此不能提供复杂的服务，比如访问数据库和图像处理等等。JSP在HTML中用<%%>里面实现。JS在html中用<Script></Script>实现


[参考1](https://zh.wikipedia.org/wiki/JavaScript)
[参考2](https://zh.wikipedia.org/wiki/JSP)
[参考3](https://blog.csdn.net/a2806005024/article/details/28265503)
