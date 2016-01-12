title: Node.js Day 01
date: 2015-02-28 16:30:02
tags: Node.js
categories: Node.js
description: play with ode.js，introduce js、Node.js and the first server side hello world program
---

在说Node.js之前，我们先来说说js，如果你也曾开发过前端，那么你一定接触到了这个叫JavaScript有趣的东西，而对于JavaScript，你只会基本的操作——为web页面添加交互。但是这远远不够，远远不是你想象中的站点，没有复杂酷炫的功能所以你不得不学起Java、PHP、Ruby、Python来为站点写后端程序。这期间，js扩展了，有了Jquery以及Prototype等框架来使你能更简便的编写js，不过这还仅仅是前端啊~就在此时，Node.js凌空出世，犹如一把珍藏的绝世好剑突然间爆发出夺目的光芒，被称作服务器端的js。从中世纪90年代的作为对DHTML进行增强的小玩具，到像jQuery那样严格意义上的前端技术，一直到现在的服务端技术，js发生了很多变化，也进步了许多。下面进行服务器端js的介绍。


##服务器端js

JavaScript最早是运行在浏览器中，然而浏览器只是提供了一个上下文，它定义了使用JavaScript可以做什么，但并没有“说”太多关于JavaScript语言本身可以做什么。事实上，JavaScript是一门“完整”的语言： 它可以使用在不同的上下文中，其能力与其他同类语言相比有过之而无不及。

Node.js事实上就是另外一种上下文，它允许在后端（脱离浏览器环境）运行JavaScript代码。

要实现在后台运行JavaScript代码，代码需要先被解释然后正确的执行。Node.js的原理正是如此，它使用了Google的V8虚拟机（Google的Chrome浏览器使用的JavaScript执行环境），来解释和执行JavaScript代码。

除此之外，伴随着Node.js的还有许多有用的模块，它们可以简化很多重复的劳作，比如向终端输出字符串。

因此，Node.js事实上既是一个运行时环境，同时又是一个库。那么问题来了，Node.js能做什么呢？

##Node.js能做什么　

NodeJS的作者说，他创造NodeJS的目的是为了实现高性能Web服务器，他首先看重的是事件机制和异步IO模型的优越性，而不是JS。但是他需要选择一种编程语言实现他的想法，这种编程语言不能自带IO功能，并且需要能良好支持事件机制。JS没有自带IO功能，天生就用于处理浏览器中的DOM事件，并且拥有一大群程序员，因此就成为了天然的选择。

如他所愿，NodeJS在服务端活跃起来，出现了大批基于NodeJS的Web服务。而另一方面，NodeJS让前端众如获神器，终于可以让自己的能力覆盖范围跳出浏览器窗口，更大批的前端工具如雨后春笋。

因此，对于前端而言，虽然不是人人都要拿NodeJS写一个服务器程序，但简单可至使用命令交互模式调试JS代码片段，复杂可至编写工具提升工作效率。

要使用Node.js,首先需要进行安装。关于如何安装Node.js，这里就不赘述了，可以直接参考[官方的安装指南](https://github.com/joyent/node/wiki/Installation)。安装完成后，继续回来阅读下面的内容。


##第一个程序：Hello World

安装完了麽？如果已经OK的话可以来看看在终端中输入：

	$ node --version
	v0.10.35

查看版本号，然后输入“node”进入如下界面，进行交互：

	$ node
	> 1+1
	2

现在可以输入.help看看有哪些用得上的指令（PS：不要忘了有“.”）：

	> .help
	.break  Sometimes you get stuck, this gets you out
	.clear  Alias for .break
	.exit   Exit the repl
	.help   Show repl options
	.load   Load JS from a file into the REPL session
	.save   Save all evaluated commands in this REPL session to a file

那么现在使用.exit来退出吧node吧。

好了，“废话”不多说了，马上开始我们第一个Node.js应用：“Hello World”。

打开你最喜欢的编辑器，创建一个hello.js文件。我们要做就是向STDOUT输出“Hello World”，如下是实现该功能的代码：


	console.log("Hello World");

保存该文件，并通过Node.js来执行：

	$ node hello.js
	Hello World!

正常的话，就会在终端输出Hello World 。

好吧，我承认这个应用是有点无趣，那么下面我们就来点“干货”。

##Hello World服务器版

使用你喜欢的编辑器新建个文件，名为server.js或者其它你喜欢的名字，该栗子中你可以在网页的页面上看到“Hello World”，如下是实现该功能的代码:

	var http = require("http");
	
	http.createServer(function(request, response) {
	  response.writeHead(200, {"Content-Type": "text/plain"});
	  response.write("Hello World");
	  response.end();
	}).listen(8888);
	console.log('Server is running at http://localhost:8888')


下面先对代码进行解释：

* 第一行的意思就是引入 `http` 模块，并将它赋予 `http` 这个变量等待使用。

* 第三行到第六行是使用http模块来创建一个服务器端的服务，函数会接收 req 和 res 两个对象，他们分别是请求的 request 和 response。request 中包含了浏览器传来的各种信息，比如 query 啊，body 啊，headers 啊之类的，都可以通过 req 对象访问到。 res 对象，我们一般不从里面取信息，而是通过它来定制我们向浏览器输出的信息，比如 header 信息，比如想要向浏览器输出的内容。这里我们调用了它的 send 方法，向浏览器输出一个字符串。

* 第七行是定义完之后让它监听8888端口。

* 最后一行会在 listen动作成功后执行，我们这里执行了一个命令行输出操作，告诉我们监听动作已完成。
　　

保存该文件，并通过Node.js来执行：

	$ node server.js
	Server is running at http://localhost:8888

正常的话，打开浏览器，在地址栏输入<http://localhost:8888/>会看到“Hello World”。

