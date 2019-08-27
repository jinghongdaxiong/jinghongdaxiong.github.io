---
title: node.js教程
date: 2019-08-26 14:08:08
tags: [node ]
---

# 简介
* 简单的说 Node.js 就是运行在服务端的 JavaScript。
  
* Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。
  
* Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。
  
## 查看版本
    
    node -v
    v12.9.0

## Hello World
   
   vim helloWorld.js
   
   console.log("Hello World");
   
   node helloWorld.js
   
## 交互模式

    $ node
    > console.log('Hello World!');
    Hello World!
    
# Node.js 安装配置

    Mac安装
    brew install node

# Node.js 创建第一个应用
    
## 步骤一、引入 required 模块

我们使用 require 指令来载入 http 模块，并将实例化的 HTTP 赋值给变量 http，实例如下:

    var http = require("http");
    
## 步骤二、创建服务器
接下来我们使用 http.createServer() 方法创建服务器，并使用 listen 方法绑定 8888 端口。 函数通过 request, response 参数来接收和响应数据。

实例如下，在你项目的根目录下创建一个叫 server.js 的文件，并写入以下代码：

    var http = require('http');
    http.createServer(function (request, response) {
    
        // 发送 HTTP 头部 
        // HTTP 状态值: 200 : OK
        // 内容类型: text/plain
        response.writeHead(200, {'Content-Type': 'text/plain'});
    
        // 发送响应数据 "Hello World"
        response.end('Hello World\n');
    }).listen(8888);
    
    // 终端打印如下信息
    console.log('Server running at http://127.0.0.1:8888/');

以上代码我们完成了一个可以工作的 HTTP 服务器。

使用 node 命令执行以上的代码：

    node server.js
    Server running at http://127.0.0.1:8888/

接下来，打开浏览器访问 http://127.0.0.1:8888/，你会看到一个写着 "Hello World"的网页。

## 分析Node.js 的 HTTP 服务器：
   
* 第一行请求（require）Node.js 自带的 http 模块，并且把它赋值给 http 变量。

* 接下来我们调用 http 模块提供的函数： createServer 。这个函数会返回 一个对象，这个对象有一个叫做 listen 的方法，这个方法有一个数值参数， 指定这个 HTTP 服务器监听的端口号。


# NPM 使用介绍

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

* 允许用户从NPM服务器下载别人编写的第三方包到本地使用。

* 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。

* 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

## 查看版本

    $ npm -v
    6.10.3

## 升级

    $ sudo npm install npm -g

如果是 Window 系统使用以下命令即可：

    npm install npm -g
使用淘宝镜像的命令：

    npm install -g cnpm --registry=https://registry.npm.taobao.org
    
## 使用 npm 命令安装模块
npm 安装 Node.js 模块语法格式如下：

    $ npm install <Module Name>
以下实例，我们使用 npm 命令安装常用的 Node.js web框架模块 express:

    $ npm install express
安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 require('express') 的方式就好，无需指定第三方包路径。

    var express = require('express');
    
## 全局安装与本地安装
npm 的包安装分为本地安装（local）、全局安装（global）两种，从敲的命令行来看，差别只是有没有-g而已，比如

    npm install express          # 本地安装
    npm install express -g   # 全局安装

如果出现以下错误：

    npm err! Error: connect ECONNREFUSED 127.0.0.1:8087 
    
解决办法为：

    $ npm config set proxy null

## 本地安装

* 1.将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。

* 2.可以通过 require() 来引入本地安装的包。

## 全局安装

* 1.将安装包放在 /usr/local 下或者你 node 的安装目录。
* 2.可以直接在命令行里使用。

如果你希望具备两者功能，则需要在两个地方安装它或使用 npm link。
接下来我们使用全局方式安装 express

    $ npm install express -g

## 查看安装信息

你可以使用以下命令来查看所有全局安装的模块：
    
    $ npm list -g
    
    ├─┬ cnpm@4.3.2
    │ ├── auto-correct@1.0.0
    │ ├── bagpipe@0.3.5
    │ ├── colors@1.1.2
    │ ├─┬ commander@2.9.0
    │ │ └── graceful-readlink@1.0.1
    │ ├─┬ cross-spawn@0.2.9
    │ │ └── lru-cache@2.7.3
    ……

如果要查看某个模块的版本号，可以使用命令如下：

    $ npm list grunt
    
    projectName@projectVersion /path/to/project/folder
    └── grunt@0.4.1
    
## 使用 package.json
package.json 位于模块的目录下，用于定义包的属性。接下来让我们来看下 express 包的 package.json 文件，位于 node_modules/express/package.json 内容：

## Package.json 属性说明

    name - 包名。
    
    version - 包的版本号。
    
    description - 包的描述。
    
    homepage - 包的官网 url 。
    
    author - 包的作者姓名。
    
    contributors - 包的其他贡献者姓名。
    
    dependencies - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
    
    repository - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
    
    main - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
    
    keywords - 关键字
    
## 卸载模块

我们可以使用以下命令来卸载 Node.js 模块。

    $ npm uninstall express

卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：

    $ npm ls
    
## 更新模块

    $ npm update express
    
## 搜索模块
    
    $ npm search express
    
## 创建模块

创建模块，package.json 文件是必不可少的。我们可以使用 NPM 生成 package.json 文件，生成的文件包含了基本的结果。

    $ npm init
    This utility will walk you through creating a package.json file.
    It only covers the most common items, and tries to guess sensible defaults.
    
    See `npm help json` for definitive documentation on these fields
    and exactly what they do.
    
    Use `npm install <pkg> --save` afterwards to install a package and
    save it as a dependency in the package.json file.
    
    Press ^C at any time to quit.
    name: (node_modules) runoob                   # 模块名
    version: (1.0.0) 
    description: Node.js 测试模块(www.runoob.com)  # 描述
    entry point: (index.js) 
    test command: make test
    git repository: https://github.com/runoob/runoob.git  # Github 地址
    keywords: 
    author: 
    license: (ISC) 
    About to write to ……/node_modules/package.json:      # 生成地址
    
    {
      "name": "runoob",
      "version": "1.0.0",
      "description": "Node.js 测试模块(www.runoob.com)",
      ……
    }
    
    
    Is this ok? (yes) yes
    
以上的信息，你需要根据你自己的情况输入。在最后输入 "yes" 后会生成 package.json 文件。

接下来我们可以使用以下命令在 npm 资源库中注册用户（使用邮箱注册）：

    $ npm adduser
    Username: mcmohd
    Password:
    Email: (this IS public) mcmohd@gmail.com

接下来我们就用以下命令来发布模块：

    $ npm publish
如果你以上的步骤都操作正确，你就可以跟其他模块一样使用 npm 来安装。

## 版本号
使用NPM下载和发布代码时都会接触到版本号。NPM使用语义版本号来管理代码，这里简单介绍一下。

语义版本号分为X.Y.Z三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新。

* 如果只是修复bug，需要更新Z位。
* 如果是新增了功能，但是向下兼容，需要更新Y位。
* 如果有大变动，向下不兼容，需要更新X位。

版本号有了这个保证后，在申明第三方包依赖时，除了可依赖于一个固定版本号外，还可依赖于某个范围的版本号。例如"argv": "0.0.x"表示依赖于0.0.x系列的最新版argv。                    
        
NPM支持的所有版本号范围指定方式可以查看[官方文档](https://docs.npmjs.com/)。

## NPM 常用命令
使用npm help可查看所有命令。

* NPM提供了很多命令，例如install和publish，使用npm help可查看所有命令。

* 使用npm help <command>可查看某条命令的详细帮助，例如npm help install。

* 在package.json所在目录下使用npm install . -g可先在本地安装当前命令行程序，可用于发布前的本地测试。

* 使用npm update <package>可以把当前目录下node_modules子目录里边的对应模块更新至最新版本。

* 使用npm update <package> -g可以把全局安装的对应命令行程序更新至最新版。

* 使用npm cache clear可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人。

* 使用npm unpublish <package>@<version>可以撤销发布自己发布过的某个版本代码。

## 使用淘宝 NPM 镜像
大家都知道国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

    $ npm install -g cnpm --registry=https://registry.npm.taobao.org
这样就可以使用 cnpm 命令来安装模块了：

    $ cnpm install [name]

    
# Node.js REPL(交互式解释器)
Node.js REPL(Read Eval Print Loop:交互式解释器) 表示一个电脑的环境，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。

Node 自带了交互式解释器，可以执行以下任务：

* 读取 - 读取用户输入，解析输入了Javascript 数据结构并存储在内存中。
  
* 执行 - 执行输入的数据结构
  
* 打印 - 输出结果
  
* 循环 - 循环操作以上步骤直到用户***两次***按下  **ctrl-c**  按钮退出。
  
Node 的交互式解释器可以很好的调试 Javascript 代码。

## 启动 Node 的终端：
    
    $ node
    Welcome to Node.js v12.9.0.
    Type ".help" for more information.
    >
    
## 简单的表达式运算
这时我们就可以在 > 后输入简单的表达式，并按下回车键来计算结果。

    $ node
    > 1 +4
    5
    > 5 / 2
    2.5
    > 3 * 6
    18
    > 4 - 1
    3
    > 1 + ( 2 * 3 ) - 4
    3
    >
## 使用变量
你可以将数据存储在变量中，并在你需要的时候使用它。

变量声明需要使用 var 关键字，如果没有使用 var 关键字变量会直接打印出来。

使用 var 关键字的变量可以使用 console.log() 来输出变量。

    $ node
    > x = 10
    10
    > var y = 10
    undefined
    > x + y
    20
    > console.log("Hello World")
    Hello World
    undefined
    > console.log("www.runoob.com")
    www.runoob.com
    undefined
    
## 多行表达式
Node REPL 支持输入多行表达式，这就有点类似 JavaScript。接下来让我们来执行一个 do-while 循环：

    $ node
    > var x = 0
    undefined
    > do {
    ... x++;
    ... console.log("x: " + x);
    ... } while ( x < 5 );
    x: 1
    x: 2
    x: 3
    x: 4
    x: 5
    undefined
    >
... 三个点的符号是系统自动生成的，你回车换行后即可。Node 会自动检测是否为连续的表达式。
    
## 下划线(_)变量
你可以使用下划线(_)获取上一个表达式的运算结果：
    
    $ node
    > var x = 10
    undefined
    > var y = 20
    undefined
    > x + y
    30
    > var sum = _
    undefined
    > console.log(sum)
    30
    undefined
    >
    
## REPL 命令

* ctrl + c - 退出当前终端。
  
* ctrl + c 按下两次 - 退出 Node REPL。
  
* ctrl + d - 退出 Node REPL.
  
* 向上/向下 键 - 查看输入的历史命令
  
* tab 键 - 列出当前命令
  
* .help - 列出使用命令
  
* .break - 退出多行表达式
  
* .clear - 退出多行表达式
  
* .save filename - 保存当前的 Node REPL 会话到指定文件
  
* .load filename - 载入当前 Node REPL 会话的文件内容。
  
## 停止 REPL
前面我们已经提到按下两次 ctrl + c 键就能退出 REPL:

    $ node
    >
    (^C again to quit)
    >      
    
# Node.js 回调函数

Node.js 异步编程的直接体现就是回调。

异步编程依托于回调来实现，但不能说使用了回调后程序就异步化了。

回调函数在完成任务后就会被调用，Node 使用了大量的回调函数，Node 所有 API 都支持回调函数。

例如，我们可以一边读取文件，一边执行其他命令，在文件读取完成后，我们将文件内容作为回调函数的参数返回。这样在执行代码时就没有阻塞或等待文件 I/O 操作。这就大大提高了 Node.js 的性能，可以处理大量的并发请求。

回调函数一般作为函数的最后一个参数出现：

    function foo1(name, age, callback) { }
    function foo2(value, callback1, callback2) { }
    
## 阻塞代码实例
    
创建一个文件 input.txt ，内容如下：

        菜鸟教程官网地址：www.runoob.com

创建 main.js 文件, 代码如下：

    var fs = require("fs");
    var data = fs.readFileSync('input.txt');
    console.log(data.toString());
    console.log("程序执行结束!");

以上代码执行结果如下：

    $ node main.js
    菜鸟教程官网地址：www.runoob.com

    程序执行结束!
    
## 非阻塞代码实例
创建一个文件 input.txt ，内容如下：

    菜鸟教程官网地址：www.runoob.com
    
创建 main.js 文件, 代码如下：

    fs.readFile('input.txt', function (err, data) {
        if (err) return console.error(err);
        console.log(data.toString());
    });
    
    console.log("程序执行结束!");
    
以上代码执行结果如下：

    $ node main.js
    程序执行结束!
    菜鸟教程官网地址：www.runoob.com
    
以上两个实例我们了解了阻塞与非阻塞调用的不同。第一个实例在文件读取完后才执行完程序。 第二个实例我们不需要等待文件读取完，这样就可以在读取文件时同时执行接下来的代码，大大提高了程序的性能。

因此，阻塞是按顺序执行的，而非阻塞是不需要按顺序的，所以如果需要处理回调函数的参数，我们就需要写在回调函数内。

# Node.js 事件循环

* Node.js 是单进程单线程应用程序，但是因为 V8 引擎提供的异步执行回调接口，通过这些接口可以处理大量的并发，所以性能非常高。    

* Node.js 几乎每一个 API 都是支持回调函数的。

* Node.js 基本上所有的事件机制都是用设计模式中观察者模式实现。
  
* Node.js 单线程类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数.
  
## 事件驱动程序

Node.js 使用事件驱动模型，当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。

当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户。

这个模型非常高效可扩展性非常强，因为webserver一直接受请求而不等待任何读写操作。（这也被称之为非阻塞式IO或者事件驱动IO）

在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件时触发回调函数。

![](https://www.runoob.com/wp-content/uploads/2015/09/event_loop.jpg)

整个事件驱动的流程就是这么实现的，非常简洁。有点类似于观察者模式，事件相当于一个主题(Subject)，而所有注册到这个事件上的处理函数相当于观察者(Observer)。

Node.js 有多个内置的事件，我们可以通过引入 events 模块，并通过实例化 EventEmitter 类来绑定和监听事件，如下实例：

    // 引入 events 模块
    var events = require('events');
    // 创建 eventEmitter 对象
    var eventEmitter = new events.EventEmitter();

以下程序绑定事件处理程序：

    // 绑定事件及事件的处理程序
    eventEmitter.on('eventName', eventHandler);

我们可以通过程序触发事件：

    // 触发事件
    eventEmitter.emit('eventName');
        
## 实例
创建 main.js 文件，代码如下所示：

    // 引入 events 模块
    var events = require('events');
    // 创建 eventEmitter 对象
    var eventEmitter = new events.EventEmitter();
    
    // 创建事件处理程序
    var connectHandler = function connected() {
       console.log('连接成功。');
      
       // 触发 data_received 事件 
       eventEmitter.emit('data_received');
    } 
    
    // 绑定 connection 事件处理程序
    eventEmitter.on('connection', connectHandler);
     
    // 使用匿名函数绑定 data_received 事件
    eventEmitter.on('data_received', function(){
       console.log('数据接收成功。');
    });    
    
    // 触发 connection 事件 
    eventEmitter.emit('connection');
    
    console.log("程序执行完毕。");

接下来让我们执行以上代码：

    $ node main.js
    连接成功。
    数据接收成功。
    程序执行完毕。
    
## Node 应用程序是如何工作的？
在 Node 应用程序中，执行异步操作的函数将回调函数作为最后一个参数， 回调函数接收错误对象作为第一个参数。

接下来让我们来重新看下前面的实例，创建一个 input.txt ,文件内容如下：

    菜鸟教程官网地址：www.runoob.com
创建 main.js 文件，代码如下：

    var fs = require("fs");
    
    fs.readFile('input.txt', function (err, data) {
       if (err){
          console.log(err.stack);
          return;
       }
       console.log(data.toString());
    });
    console.log("程序执行完毕");
以上程序中 fs.readFile() 是异步函数用于读取文件。 如果在读取文件过程中发生错误，错误 err 对象就会输出错误信息。

如果没发生错误，readFile 跳过 err 对象的输出，文件内容就通过回调函数输出。

执行以上代码，执行结果如下：
    
    程序执行完毕
    菜鸟教程官网地址：www.runoob.com      
接下来我们删除 input.txt 文件，执行结果如下所示：

    程序执行完毕
    Error: ENOENT, open 'input.txt'
因为文件 input.txt 不存在，所以输出了错误信息。

# Node.js EventEmitter
          
Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。

Node.js 里面的许多对象都会分发事件：一个 net.Server 对象会在每次有新连接时触发一个事件， 一个 fs.readStream 对象会在文件被打开的时候触发一个事件。 所有这些产生事件的对象都是 events.EventEmitter 的实例。

## EventEmitter 类
events 模块只提供了一个对象： events.EventEmitter。EventEmitter 的核心就是事件触发与事件监听器功能的封装。

你可以通过require("events");来访问该模块。

    // 引入 events 模块
    var events = require('events');
    // 创建 eventEmitter 对象
    var eventEmitter = new events.EventEmitter();
EventEmitter 对象如果在实例化时发生错误，会触发 error 事件。当添加新的监听器时，newListener 事件会触发，当监听器被移除时，removeListener 事件被触发。

### 下面我们用一个简单的例子说明 EventEmitter 的用法：
    
    //event.js 文件
    var EventEmitter = require('events').EventEmitter; 
    var event = new EventEmitter(); 
    event.on('some_event', function() { 
        console.log('some_event 事件触发'); 
    }); 
    setTimeout(function() { 
        event.emit('some_event'); 
    }, 1000); 
          
执行结果如下：

运行这段代码，1 秒后控制台输出了 'some_event 事件触发'。其原理是 event 对象注册了事件 some_event 的一个监听器，然后我们通过 setTimeout 在 1000 毫秒以后向 event 对象发送事件 some_event，此时会调用some_event 的监听器。

    $ node event.js 
    some_event 事件触发          
EventEmitter 的每个事件由一个事件名和若干个参数组成，事件名是一个字符串，通常表达一定的语义。对于每个事件，EventEmitter 支持 若干个事件监听器。

当事件触发时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递。

让我们以下面的例子解释这个过程：

    //event.js 文件
    var events = require('events'); 
    var emitter = new events.EventEmitter(); 
    emitter.on('someEvent', function(arg1, arg2) { 
        console.log('listener1', arg1, arg2); 
    }); 
    emitter.on('someEvent', function(arg1, arg2) { 
        console.log('listener2', arg1, arg2); 
    }); 
    emitter.emit('someEvent', 'arg1 参数', 'arg2 参数'); 
    
执行以上代码，运行的结果如下：

    $ node event.js 
    listener1 arg1 参数 arg2 参数
    listener2 arg1 参数 arg2 参数    

以上例子中，emitter 为事件 someEvent 注册了两个事件监听器，然后触发了 someEvent 事件。

运行结果中可以看到两个事件监听器回调函数被先后调用。 这就是EventEmitter最简单的用法。

EventEmitter 提供了多个属性，如 on 和 emit。on 函数用于绑定事件函数，emit 属性用于触发一个事件。接下来我们来具体看下 EventEmitter 的属性介绍。
    
## 方法

### 1. addListener(event, listener)
为指定事件添加一个监听器到监听器数组的尾部。

### 2. on(event, listener)
为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数。
    
    server.on('connection', function (stream) {
      console.log('someone connected!');
    });
    
### 3. once(event, listener)
为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器。

    server.once('connection', function (stream) {
      console.log('Ah, we have our first user!');
    });

### 4. removeListener(event, listener)
移除指定事件的某个监听器，监听器必须是该事件已经注册过的监听器。

它接受两个参数，第一个是事件名称，第二个是回调函数名称。

    var callback = function(stream) {
      console.log('someone connected!');
    };
    server.on('connection', callback);
    // ...
    server.removeListener('connection', callback);
    
### 5. removeAllListeners([event])
移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。

### 6. setMaxListeners(n)
默认情况下， EventEmitters 如果你添加的监听器超过 10 个就会输出警告信息。 setMaxListeners 函数用于提高监听器的默认限制的数量。

### 7. listeners(event)
返回指定事件的监听器数组。

### 8. emit(event, [arg1], [arg2], [...])
按监听器的顺序执行执行每个监听器，如果事件有注册监听返回 true，否则返回 false。

## 类方法
### listenerCount(emitter, event)
返回指定事件的监听器数量。
    
    events.EventEmitter.listenerCount(emitter, eventName) //已废弃，不推荐
    events.emitter.listenerCount(eventName) //推荐
    
## 事件
### newListener 
* event - 字符串，事件名称

* listener - 处理事件函数

该事件在添加新监听器时被触发。

### removeListener
* event - 字符串，事件名称
  
* listener - 处理事件函数
  
从指定监听器数组中删除一个监听器。需要注意的是，此操作将会改变处于被删监听器之后的那些监听器的索引。
  
## 实例
以下实例通过 connection（连接）事件演示了 EventEmitter 类的应用。

创建 main.js 文件，代码如下：

    var events = require('events');
    var eventEmitter = new events.EventEmitter();
    
    // 监听器 #1
    var listener1 = function listener1() {
       console.log('监听器 listener1 执行。');
    }
    
    // 监听器 #2
    var listener2 = function listener2() {
      console.log('监听器 listener2 执行。');
    }
    
    // 绑定 connection 事件，处理函数为 listener1 
    eventEmitter.addListener('connection', listener1);
    
    // 绑定 connection 事件，处理函数为 listener2
    eventEmitter.on('connection', listener2);
    
    var eventListeners = eventEmitter.listenerCount('connection');
    console.log(eventListeners + " 个监听器监听连接事件。");
    
    // 处理 connection 事件 
    eventEmitter.emit('connection');
    
    // 移除监绑定的 listener1 函数
    eventEmitter.removeListener('connection', listener1);
    console.log("listener1 不再受监听。");
    
    // 触发连接事件
    eventEmitter.emit('connection');
    
    eventListeners = eventEmitter.listenerCount('connection');
    console.log(eventListeners + " 个监听器监听连接事件。");
    
    console.log("程序执行完毕。");

以上代码，执行结果如下所示：

    $ node main.js  
    2 个监听器监听连接事件。
    监听器 listener1 执行。
    监听器 listener2 执行。
    listener1 不再受监听。
    监听器 listener2 执行。
    1 个监听器监听连接事件。
    程序执行完毕。
## error 事件

EventEmitter 定义了一个特殊的事件 error，它包含了错误的语义，我们在遇到 异常的时候通常会触发 error 事件。

当 error 被触发时，EventEmitter 规定如果没有响 应的监听器，Node.js 会把它当作异常，退出程序并输出错误信息。

我们一般要为会触发 error 事件的对象设置监听器，避免遇到错误后整个程序崩溃。例如：

    var events = require('events'); 
    var emitter = new events.EventEmitter(); 
    emitter.emit('error'); 
运行时会显示以下错误：

    node.js:201 
    throw e; // process.nextTick error, or 'error' event on first tick 
    ^ 
    Error: Uncaught, unspecified 'error' event. 
    at EventEmitter.emit (events.js:50:15) 
    at Object.<anonymous> (/home/byvoid/error.js:5:9) 
    at Module._compile (module.js:441:26) 
    at Object..js (module.js:459:10) 
    at Module.load (module.js:348:31) 
    at Function._load (module.js:308:12) 
    at Array.0 (module.js:479:10) 
    at EventEmitter._tickCallback (node.js:192:40)    
    
## 继承 EventEmitter
大多数时候我们不会直接使用 EventEmitter，而是在对象中继承它。包括 fs、net、 http 在内的，只要是支持事件响应的核心模块都是 EventEmitter 的子类。

为什么要这样做呢？原因有两点：

首先，具有某个实体功能的对象实现事件符合语义， 事件的监听和发生应该是一个对象的方法。

其次 JavaScript 的对象机制是基于原型的，支持 部分多重继承，继承 EventEmitter 不会打乱对象原有的继承关系。


# Node.js Buffer(缓冲区)
JavaScript 语言自身只有字符串数据类型，没有二进制数据类型。

但在处理像TCP流或文件流时，必须使用到二进制数据。因此在 Node.js中，定义了一个 Buffer 类，该类用来创建一个专门存放二进制数据的缓存区。

在 Node.js 中，Buffer 类是随 Node 内核一起发布的核心库。Buffer 库为 Node.js 带来了一种存储原始数据的方法，可以让 Node.js 处理二进制数据，每当需要在 Node.js 中处理I/O操作中移动的数据时，就有可能使用 Buffer 库。原始数据存储在 Buffer 类的实例中。一个 Buffer 类似于一个整数数组，但它对应于 V8 堆内存之外的一块原始内存。

>> 在v6.0之前创建Buffer对象直接使用new Buffer()构造函数来创建对象实例，但是Buffer对内存的权限操作相比很大，可以直接捕获一些敏感信息，所以在v6.0以后，官方文档里面建议使用 Buffer.from() 接口去创建Buffer对象。    

## Buffer 与字符编码
Buffer 实例一般用于表示编码字符的序列，比如 UTF-8 、 UCS2 、 Base64 、或十六进制编码的数据。 通过使用显式的字符编码，就可以在 Buffer 实例与普通的 JavaScript 字符串之间进行相互转换。

    const buf = Buffer.from('runoob', 'ascii');
    
    // 输出 72756e6f6f62
    console.log(buf.toString('hex'));
    
    // 输出 cnVub29i
    console.log(buf.toString('base64'));
    
Node.js 目前支持的字符编码包括：

* ascii - 仅支持 7 位 ASCII 数据。如果设置去掉高位的话，这种编码是非常快的。
  
* utf8 - 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8 。
  
* utf16le - 2 或 4 个字节，小字节序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。
  
* ucs2 - utf16le 的别名。
  
* base64 - Base64 编码。
  
* latin1 - 一种把 Buffer 编码成一字节编码的字符串的方式。
  
* binary - latin1 的别名。
  
* hex - 将每个字节编码为两个十六进制字符。
  
## 创建 Buffer 类

Buffer 提供了以下 API 来创建 Buffer 类：

                    
   