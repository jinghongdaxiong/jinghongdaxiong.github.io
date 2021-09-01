---
title: java9JShell
date: 2021-06-12 15:06:38
tags:
categories:
---
# Java 9 REPL (JShell)
REPL(Read Eval Print Loop)意为交互式的编程环境。

JShell 是 Java 9 新增的一个交互式的编程环境工具。它允许你无需使用类或者方法包装来执行 Java 语句。它与 Python 的解释器类似，可以直接 输入表达式并查看其执行结果。

执行 JSHELL

````
$ jshell
|  Welcome to JShell -- Version 9-ea
|  For an introduction type: /help intro
jshell>

````
查看 JShell 命令

输入 /help 可以查看 JShell相关的命令：
````
jshell> /help
|  Type a Java language expression, statement, or declaration.
|  Or type one of the following commands:
|  /list [<name or id>|-all|-start]
|  list the source you have typed
|  /edit <name or id>
|  edit a source entry referenced by name or id
|  /drop <name or id>
|  delete a source entry referenced by name or id
|  /save [-all|-history|-start] <file>
|  Save snippet source to a file.
|  /open <file>
|  open a file as source input
|  /vars [<name or id>|-all|-start]
|  list the declared variables and their values
|  /methods [<name or id>|-all|-start]
|  list the declared methods and their signatures
|  /types [<name or id>|-all|-start]
|  list the declared types
|  /imports 
|  list the imported items

````
执行 JShell 命令
/imports 命令用于查看已导入的包：

````
jshell> /imports
|    import java.io.*
|    import java.math.*
|    import java.net.*
|    import java.nio.file.*
|    import java.util.*
|    import java.util.concurrent.*
|    import java.util.function.*
|    import java.util.prefs.*
|    import java.util.regex.*
|    import java.util.stream.*
jshell>

````
JShell 执行计算
以下实例执行 JShell 简单计算：

````
jshell> 3+1
$1 ==> 4
jshell> 13%7
$2 ==> 6
jshell> $2
$2 ==> 6
jshell>

````
JShell 创建与使用函数
创建一个函数 doubled() ，将传入的整型参数乘于 2 后返回：

````
jshell> int doubled(int i){ return i*2;}
|  created method doubled(int)
jshell> doubled(6)
$3 ==> 12
jshell>
````
退出 JShell

输入 /exit 命令退出 jshell：

````
jshell> /exit
| Goodbye 

````




