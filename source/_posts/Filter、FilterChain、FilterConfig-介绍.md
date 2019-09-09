---
title: Filter、FilterChain、FilterConfig 介绍
date: 2019-09-06 09:52:26
tags: [java,filter]
categories: [java]
---

# 一、Filter 的基本工作原理

* 1、Filter 程序是一个实现了特殊接口的 Java 类，与 Servlet 类似，也是由 Servlet 容器进行调用和执行的。

* 2、当在 web.xml 注册了一个 Filter 来对某个 Servlet 程序进行拦截处理时，它可以决定是否将请求继续传递给 Servlet 程序，以及对请求和响应消息是否进行修改。

* 3、当 Servlet 容器开始调用某个 Servlet 程序时，如果发现已经注册了一个 Filter 程序来对该 Servlet 进行拦截，那么容器不再直接调用 Servlet 的 service 方法，而是调用 Filter 的 doFilter 方法，再由 doFilter 方法决定是否去激活 service 方法。

* 4、但在 Filter.doFilter 方法中不能直接调用 Servlet 的 service 方法，而是调用 FilterChain.doFilter 方法来激活目标 Servlet 的 service 方法，FilterChain 对象是通过 Filter.doFilter 方法的参数传递进来的。

* 5、只要在 Filter.doFilter 方法中调用 FilterChain.doFilter 方法的语句前后增加某些程序代码，这样就可以在 Servlet 进行响应前后实现某些特殊功能。

* 6、如果在 Filter.doFilter 方法中没有调用 FilterChain.doFilter 方法，则目标 Servlet 的 service 方法不会被执行，这样通过 Filter 就可以阻止某些非法的访问请求。
![](/images/Filter的基本工作原理.png)
# 二、Filter 链

* 1、在一个 Web 应用程序中可以注册多个 Filter 程序，每个 Filter 程序都可以对一个或一组 Servlet 程序进行拦截。如果有多个 Filter 程序都可以对某个 Servlet 程序的访问过程进行拦截，当针对该 Servlet 的访问请求到达时，Web 容器将把这多个 Filter 程序组合成一个 Filter 链（也叫过滤器链）。

* 2、Filter 链中的各个 Filter 的拦截顺序与它们在 web.xml 文件中的映射顺序一致，上一个 Filter.doFilter 方法中调用 FilterChain.doFilter 方法将激活下一个 Filter的doFilter 方法，最后一个 Filter.doFilter 方法中调用的 FilterChain.doFilter 方法将激活目标 Servlet的service 方法。

* 3、只要 Filter 链中任意一个 Filter 没有调用 FilterChain.doFilter 方法，则目标 Servlet 的 service 方法都不会被执行。

# 三、Filter 接口
一个 Filter 程序就是一个 Java 类，这个类必须实现 Filter 接口。javax.servlet.Filter 接口中定义了三个方法：init、doFilter、destory。

## 1、init 方法
* 在 Web 应用程序启动时，Web 服务器（Web 容器）将根据其 web.xml 文件的配置信息来创建每个注册的 Filter 的实例对象，并将其保存在内存中。
   
* Web 容器创建 Filter 的实例对象后，将立即调用该 Filter 对象的 init 方法。init 方法在 Filter 生命周期中仅被执行一次，Web 容器在调用 init 方法时，会传递一个包含 Filter 的配置和运行环境信息的 FilterConfig 对象。


    public voic init(FilterConfig filterConfig) throws ServletException

*   开发人员可以在 init 方法中完成与构造方法类似的初始化功能，要注意的是：如果初始化代码要使用到 FilterConfig 对象，这些代码只能在 init 方法中编写，而不能在构造方法中编写（尚未调用 init 方法，即并没有创建 FilterConfig 对象，要使用它则必然出错）。

## 2、doFilter 方法
当一个 Filter 对象能够拦截访问请求时，Servlet 容器将调用 Filter 对象的 doFilter 方法。

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws java.io.IOException.ServletException
其中，参数 request 和 response 为 Web 容器或 Filter 链中上一个 Filter 传递过来的请求和响应对象；参数 chain 为代表当前 Filter 链的对象。

## 3、destroy 方法
该方法在 Web 容器卸载 Filter 对象之前被调用，也仅执行一次。可以完成与 init 方法相反的功能，释放被该 Filter 对象打开的资源，例如：关闭数据库连接和 IO 流。


# 四、FilterChain 接口
该接口用于定义一个 Filter 链的对象应该对外提供的方法，这个接口只定义了一个 doFilter 方法。

public void doFilter(ServletRequest request, ServletResponse response) throws java.io.IOException.ServletException

FilterChain 接口的 doFilter 方法用于通知 Web 容器把请求交给 Filter 链中的下一个 Filter 去处理，如果当前调用此方法的 Filter 对象是Filter 链中的最后一个 Filter，那么将把请求交给目标 Servlet 程序去处理。

# 五、FilterConfig 接口
## 1、与普通的 Servlet 程序一样，Filter 程序也很可能需要访问 Servlet 容器。Servlet 规范将代表 ServletContext 对象和 Filter 的配置参数信息都封装到一个称为 FilterConfig 的对象中。

## 2、FilterConfig 接口则用于定义 FilterConfig 对象应该对外提供的方法，以便在 Filter 程序中可以调用这些方法来获取 ServletContext 对象，以及获取在 web.xml 文件中为 Filter 设置的友好名称和初始化参数。

## 3、FilterConfig接口定义的各个方法：
   
* getFilterName 方法，返回 <filter-name> 元素的设置值。
  
* getServletContext 方法，返回 FilterConfig 对象中所包装的 ServletContext 对象的引用。
  
* getInitParameter 方法，用于返回在 web.xml 文件中为 Filter 所设置的某个名称的初始化的参数值。
  
* getInitParameterNames 方法，返回一个 Enumeration 集合对象。
  
# 六、Filter 的注册与映射
## 1、注册 Filter
一个 <filter> 元素用于注册一个 Filter。其中，<filter-name> 元素是必需的，<filter-class> 元素也是必需的，<init-param> 元素是可选的，可以有多个 < init-param> 元素。
    
    <filter>
        <filter-name>FirstFilter</filter-name>
        <filter-class>FirstFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>GB2312</param-value>
        </init-param>
    </filter>
    
## 2、映射 Filter
    
    <filter-mapping> 元素用于设置一个 Filter 所负责拦截的资源。一个 Filter 拦截的资源可以通过两种方式来指定：资源的访问请求路径和 Servlet 名称。
### 第一种：指定资源的访问路径
    
    <filter-mapping>
        <filter-name>FirstFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
<url-pattern> 元素中的访问路径的设置方式遵循 Servlet 的 URL 映射规范。

* /*：表示拦截所有的访问请求。

* /filter/*：表示拦截 filter 目录下的所有访问请求，如：http://localhost:8888/testFilter_001/filter/xxxxxx 。
    
* /test.html：表示拦截根目录下以 test.html 为资源名的访问请求，访问链接只会是：http://localhost:8888/test.html。
    
### 第二种：指定 Servlet 的名称

    <filter-mapping>
        <filter-name>FirstFilter</filter-name>
        <servlet-name>default></servlet-name>
        <dispatcher>INCLUDE</dispatcher>
        <dispatcher>REQUEST</dispatcher>
    </filter-mapping>
    
（1）、<servlet-name> 元素与 <url-pattern> 元素是二选一的关系，其值是某个 Servlet 在 web.xml 文件中的注册名称。
（2）、<dispatcher> 元素的设置值有 4 种：REQUEST、INCLUDE、FORWARD、ERROR，分别对应 Servlet 容器调用资源的 4 种方式：

* 通过正常的访问请求调用；
* 通过 RequestDispatcher.include 方法调用；
* 通过 RequestDispatcher.forward 方法调用；
* 作为错误响应资源调用。
如果没有设置 <dispatcher> 子元素，则等效于 REQUEST 的情况。也可以设置多个 <dispatcher> 子元素，用于指定 Filter 对资源的多种调用方式都进行拦截。

# 七、Filter 程序示例

FitstFilter.java
    
    import java.io.IOException;
    import java.io.PrintWriter;
    import java.util.Enumeration;
    import javax.servlet.Filter;
    import javax.servlet.FilterChain;
    import javax.servlet.FilterConfig;
    import javax.servlet.ServletException;
    import javax.servlet.ServletRequest;
    import javax.servlet.ServletResponse;
    import javax.servlet.http.HttpServletRequest;
     
    public class FirstFilter implements Filter {
        private FilterConfig filterConfig = null;
        String paramValue = null;
     
        @Override
        public void init(FilterConfig filterConfig) throws ServletException {
            this.filterConfig = filterConfig;
            paramValue = filterConfig.getInitParameter("encoding");
        }
        
        @Override
        public void doFilter(ServletRequest request, ServletResponse response,
                FilterChain chain) throws IOException, ServletException {
            System.out.println("begin headers-------------------");
            Enumeration<?> headerNames = ((HttpServletRequest)request).getHeaderNames();
            
            while(headerNames.hasMoreElements()) {
                String headerName = (String)headerNames.nextElement();
                System.out.println(headerName + ": " + ((HttpServletRequest)request).getHeader(headerName));
            }
            System.out.println("end headers-------------------");
            
            //在调用目标前写入响应内容
            response.setContentType("text/html; charset=gb2312");
            PrintWriter out = response.getWriter();
            out.println("IP地址为：" + request.getRemoteHost() + "<br>");
     
            chain.doFilter(request, response);
            
            //在目标返回后写入响应内容
            out.println("<br>名称为encoding的初始化参数的值为：" + paramValue);
            out.println("<br>当前Web程序的真实路径为：" + filterConfig.getServletContext().getRealPath("/"));
            
            //out.println("<br>修改了test.html文件！");
        }
        
        @Override
        public void destroy() {
            this.filterConfig = null;
        }
    }

web.xml
    
    <filter>
        <filter-name>FirstFilter</filter-name>
        <filter-class>FirstFilter</filter-class>
        <init-param>
        <param-name>encoding</param-name>
        <param-value>GB2312</param-value>
        </init-param>
    </filter>
        
    <filter-mapping>
        <filter-name>FirstFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
test.html（位于WebContent路径的filter目录中）
    
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <body>
    这就是test.html页面的原始内容！
    </body>
    </html>
    

[参考](https://my.oschina.net/u/1171518/blog/265467)