---
title: 关于 getWriter() has already been called for this response 的错误解决办法
date: 2019-09-09 13:46:37
tags: [java,exception]
categories: [exception]
---

上篇Filter、FilterChain、FilterConfig 介绍
文中的代码若在多个filter中执行则会报错" getWriter() has already been called for this response "

解决方案为在doFilter() 之前将流关闭.

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
            response.setContentType("text/html; charset=utf-8");
            PrintWriter out = response.getWriter();
            out.println("IP地址为：" + request.getRemoteHost() + "<br>");
    
    
            //在目标返回后写入响应内容
            out.println("<br>名称为encoding的初始化参数的值为：" + paramValue);
            out.println("<br>当前Web程序的真实路径为：" + filterConfig.getServletContext().getRealPath("/"));
            out.close(); // 不关闭则会报错 getWriter() has already been called for this response
            chain.doFilter(request, response);
        }


