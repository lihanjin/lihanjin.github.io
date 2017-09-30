---
title: web Socket和Socket.IO框架
date: 2016-01-10 12:35:08
tags: 
    - NodeJS
categories: NodeJS
---
### web Socket和Socket.IO框架
####HTTP无法轻松实现实时应用：
● HTTP协议是无状态的，服务器只会响应来自客户端的请求，但是它与客户端之间不具备持续连接。
● 我们可以非常轻松的捕获浏览器上发生的事件（比如用户点击了盒子），这个事件可以轻松产生与服务器的数据交互（比如Ajax）。但是，反过来却是不可能的：服务器端发生了一个事件，服务器无法将这个事件的信息实时主动通知它的客户端。只有在客户端查询服务器的当前状态的时候，所发生事件的信息才会从服务器传递到客户端。

但是，确实聊天室确实存在。

方法：
● 长轮询：客户端每隔很短的时间，都会对服务器发出请求，查看是否有新的消息，只要轮询速度足够快，例如1秒，就能给人造成交互是实时进行的印象。这种做法是无奈之举，实际上对服务器、客户端双方都造成了大量的性能浪费。


● 长连接：客户端只请求一次，但是服务器会将连接保持，不会返回结果（想象一下我们没有写res.end()时，浏览器一直转小菊花）。服务器有了新数据，就将数据发回来，又有了新数据，就将数据发回来，而一直保持挂起状态。这种做法的也造成了大量的性能浪费。

WebSocket协议能够让浏览器和服务器全双工实时通信，互相的，服务器也能主动通知客户端了。


● WebSocket的原理非常的简单：利用HTTP请求产生握手，HTTP头部中含有WebSocket协议的请求，所以握手之后，二者转用TCP协议进行交流（QQ的协议）。现在的浏览器和服务器之间，就是QQ和QQ服务器的关系了。
所以WebSocket协议，需要浏览器支持，更需要服务器支持。

● 支持WebSocket协议的浏览器有：Chrome 4、火狐4、IE10、Safari5

● 支持WebSocket协议的服务器有：Node 0、Apach7.0.2、Nginx1.3

Node.js上需要写一些程序，来处理TCP请求。
● Node.js从诞生之日起，就支持WebSocket协议。不过，从底层一步一步搭建一个Socket服务器很费劲（想象一下Node写一个静态文件服务都那么费劲）。所以，有大神帮我们写了一个库Socket.IO。

● Socket.IO是业界良心，新手福音。它屏蔽了所有底层细节，让顶层调用非常简单。并且还为不支持WebSocket协议的浏览器，提供了长轮询的透明模拟机制。

● Node的单线程、非阻塞I/O、事件驱动机制，使它非常适合Socket服务器。

网址：http://socket.io/

先要npm下载这个库


npm install socket.io
#### 1.写原生的JS，搭建一个服务器，server创建好之后，创建一个io对象
- scoket.js
```
/**
 * Created by Administrator on 2017/7/19 0019.
 */
var app = require('http');

var fs=require('fs');

var server=app.createServer(function(req,res){

    res.writeHead(200,{"Content-Type":"text/html;charset='uft-8'"});


    if(req.url=='/'){

        fs.readFile('./socket_io.html',function(err,data){

            res.end(data);

        })
    }

});



var io = require('socket.io')(server);

//connection有客户端连接
io.on('connection', function(socket){

    //console.log('connection');

    socket.on('to-server',function(data){

        console.log(data);

        //socket.emit('to-client','你好：'+data)  /*端对端*/
        io.emit('to-client','广播：'+data);
    })

});


server.listen(3000);
```
- scoket.html

```
- <!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<h1>我是index页面，我引用了秘密script文件</h1>


	<input type="text" value="" id="message"/>

	<input type="button" value="发送给服务端" id="btn"/>

	<script type="text/javascript" src="/socket.io/socket.io.js"></script>
	<script type="text/javascript">



	  iosocket = io();
      iosocket.on('connect', function () {  /*连接*/
				console.log('client--connect');
      })

      iosocket.on('disconnect', function () { /*断开连接*/
        	console.log('服务端关闭');
      })

	  //接收服务器的广播
	  iosocket.on('to-client', function (data) { /*断开连接*/
		  console.log(data);
	  })


		var input =document.getElementById('message');
		document.getElementById("btn").onclick = function(){
			iosocket.emit('to-server', input.value);   /*发送数据*/
		}

	</script>
</body>
</html>
```

