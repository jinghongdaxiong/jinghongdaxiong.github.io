# springboot项目中使用filter无法注入bean采坑

## 原因:web容器加载顺序所致
加载顺序是 listener ---> filter ---> servlet,当项目启动时，
filter先于servlet初始化，而Spring中默认Bean的初始化是在Servlet后进行的,
所以会注入失败。



