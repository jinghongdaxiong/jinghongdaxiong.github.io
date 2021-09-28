---
title: github登录不上解决办法
date: 2021-09-01 17:07:53
tags: [git]
categories: [git]
---

## 打开下面网站获取IP地址
https://github.com.ipaddress.com/
````
140.82.113.3 github.com
````

## 打开下面网站获取IP地址
https://fastly.net.ipaddress.com/github.global.ssl.fastly.net#ipinfo
````
199.232.69.194 github.global.ssl.fastly.net
````

## 打开下面网站获取IP地址
https://github.com.ipaddress.com/assets-cdn.github.com
````
185.199.108.153 assets-cdn.github.com
185.199.109.153 assets-cdn.github.com
185.199.110.153 assets-cdn.github.com
185.199.111.153 assets-cdn.github.com
````

## 修改host文件，在host文件添加上面地址
````

140.82.113.3 github.com
199.232.69.194 github.global.ssl.fastly.net
185.199.108.153 assets-cdn.github.com
185.199.109.153 assets-cdn.github.com
185.199.110.153 assets-cdn.github.com
185.199.111.153 assets-cdn.github.com
````
