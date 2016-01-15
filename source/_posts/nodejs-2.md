title: Node.js Day 02
date: 2015-03-07 19:24:32
tags: Node.js
categories: Nodejs
description: deep into nodejs HTTP service and modularized the code
---

先来回顾上次的内容，上一次我们使用介绍了Node.js并写了第一个服务器端的Hello World程序，在这个Hello World程序中，请求自带的http模块并将其赋给http变量，然后调用http模块的createServer函数，这个函数会返回一个对象，这个对象有一个叫做listen的方法，而这个方法有一个数值参数，指定这个HTTP服务器监听的端口号，我们当时定义的是8888端口号。那么为什么看起来有些复杂呢？那是因为我们向createServer函数传递了一个匿名函数。其实用下面的代码也可以完成同样的目的：

	var http = require("http");
	
	function onRequest(request, response) {
	  response.writeHead(200, {"Content-Type": "text/plain"});
	  response.write("Happy Women’s Day");
	  response.end();
	}
	
	http.createServer(onRequest).listen(8888);

下面再继续详细说说服务器是如何处理请求的。

##服务器是如何处理请求的　

好的，接下来我们简单分析一下我们服务器代码中剩下的部分，也就是我们的回调函数 onRequest() 的主体部分。

　　当回调启动，我们的 onRequest() 函数被触发的时候，有两个参数被传入：request 和 response 。

　　它们是对象，你可以使用它们的方法来处理HTTP请求的细节，并且响应请求（比如向发出请求的浏览器发回一些东西）。

　　所以我们的代码就是：当收到请求时，使用 response.writeHead() 函数发送一个HTTP状态200和HTTP头的内容类型（content-type），使用 response.write() 函数在HTTP相应主体中发送文本“Hello World"。

　　最后，我们调用 response.end() 完成响应。

　　目前来说，我们对请求的细节并不在意，所以我们没有使用 request 对象。

##将程序模块化

在我们刚接触程序的时候，我们如果需要写一个大一点的项目，总会有人告诉我们要将代码写的规范、易懂，那么就Nodejs来说，应该如何写呢？我们应该把所有的代码都写进一个文件里面么？又应该如何组织这些代码呢？如果我想让代码易于扩展以便我加入一些新的功能不那么麻烦，还想保持代码的可读性又要怎么做呢？

　　实际上，我们只需要把代码根据功能来放入不同的模板中，保持代码的分离还是不难的。这种方法允许你有一个主文件，你可以使用nodejs执行它，同时你也可以有其它的功能模板，它们可以被主文件和其它模块调用。

　　事实上，我们要把上一次的程序变成模块只需要把我们希望提供的功能部分导出到请求这个模块的脚本，我们把服务器脚本放到一个叫做start的函数里，然后我们会导出这个函数，下面创建一个server.js文件并写入如下代码。

	var http = require("http");
	
	function start() {
	  function onRequest(request, response) {
	    console.log("Request received.");
	    response.writeHead(200, {"Content-Type": "text/plain"});
	    response.write("Happy Women‘s Day");
	    response.end();
	  }
	
	  http.createServer(onRequest).listen(8888);
	  console.log("Server has started.");
	}
	
	exports.start = start;

这样，我们现在就可以创建我们的主文件 index.js 并在其中启动我们的HTTP了，虽然服务器的代码还在 server.js 中。

创建 index.js 文件并写入以下内容：

	var server = require("./server");
	
	server.start();

正如你所看到的，我们可以像使用任何其他的内置模块一样使用server模块：请求这个文件并把它指向一个变量，其中已导出的函数就可以被我们使用了。

好了。我们现在就可以从我们的主要脚本启动我们的的应用了，而它还是老样子：

	node index.js　

非常好，我们现在可以把我们的应用的不同部分放入不同的文件里，并且通过生成模块的方式把它们连接到一起了。

　　我们仍然只拥有整个应用的最初部分：我们可以接收HTTP请求。但是我们得做点什么——对于不同的URL请求，服务器应该有不同的反应。

　　对于一个非常简单的应用来说，你可以直接在回调函数 onRequest() 中做这件事情。不过就像我说过的，我们应该加入一些抽象的元素，让我们的例子变得更有趣一点儿。

　　不过那将是下一次的内容了，本次内容到此就结束了。
