---
title: 'springmvc在controller方法里面跳转外网的方法  '
date: 2019-09-27 15:17:15
tags: [java,spring]
categories: [spring]
---

1.return new ModelAndView(new RedirectView("https://www.baidu.com"));

2.return  "redirect:https://www.baidu.com/";


