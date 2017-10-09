---
layout: single
title: websocket通信的实现
date: 2017-10-09 22:01 +0800
category: back-end
---

## 什么是websocket？用处是什么？

WebSocket协议是基于TCP的一种新的网络协议。它实现了浏览器与服务器全双工(full-duplex)通信——允许服务器主动发送信息给客户端。通俗的说，就是基于TCP的HTTP协议只能客户端发起请求，服务器来应答，是单向的，而现在这个协议，使得服务器变得更加灵活更加聪明不那么高冷了而也有主动的一面了，同时减少了很多客户端资源的浪费。

搬个场景来说明它的好处（http协议下的ajax轮询场景）：

客户端：啦啦啦，有没有新信息(Request)

服务端：没有（Response）

客户端：啦啦啦，有没有新信息(Request)

服务端：没有。。（Response）

客户端：啦啦啦，有没有新信息(Request)

服务端：你好烦啊，没有啊。。（Response）

客户端：啦啦啦，有没有新消息（Request）

服务端：好啦好啦，有啦给你。（Response）

客户端：啦啦啦，有没有新消息（Request）

服务端：。。。。。没。。。。没。。。没有（Response） —- loop

为了解决这种问题，于是出现了websocket技术。就说这么多，如果想了解更多可以去查找更多的资料，接下来我们就来实现一个这样的通信。

## 基于node的websocket通信的实现

nodeJS的socket.io模块可以很简单的让你将websocket通信玩起来。还是直接上代码吧，以下是一个非常简单的聊天室用户登录栗子，大家可以实时看到哪些人上机了。

下面是服务器端代码：

```
// 引入必要的模块
var http = require('http')
var express = require('express')
var sio = require('socket.io')
var fs = require('fs')

var app = express()
var server = http.createServer(app)

app.get('/', function (req, res) {
    res.sendFile(__dirname + '/index.html')
})

server.listen(3000)

// socket.io监听服务器得到一个io对象，可对websocket做处理
var io = sio.listen(server)

var names = []

// 监听连接事件，连接成功可获得一个socket对象，可对websocket通信进行处理
io.on('connection', function (socket) {
    socket.emit('login', names)
    socket.on('login', function (data) {
        names.push(data)
        io.sockets.emit('login', names)
    })
})

```

nodeJS提供的socket.io可以监听创建的服务器获得一个io对象，此对象可监听客户端发起的websocket连接，一旦连接事件触发，就能通过回调函数获取到一个socket对象，该对象对应一个客户端连接，多个客户端连接会在内存里生成多个，它可on监听事件和emit发送事件，达到数据通信的目的。io下面有一个sockets对象，它是一个集合，代表内存中存在的所有socket对象，这个对象就可以轻松的达到群发消息的目的了。上面例子中，就可以给所有连接成功的客户端派发login事件。

下面是客户端代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="/socket.io/socket.io.js"></script>
    <style>
        ul, li, ol {
            margin: 0;
            padding: 0;
        }
    </style>
</head>
<body>
    <div>
        <label for="userName">用户名：</label>
        <input type="text" id="userName" name="userName">
        <button id="button">登录</button>
    </div>
    <ul id="content">

    </ul>
    <script>
        var userNameInput = document.getElementById('userName');
        var button = document.getElementById('button');
        var content = document.getElementById('content');
        var socket = io.connect();
        var items = []

        button.addEventListener('click', function (e) {
            socket.emit('login', userNameInput.value.trim())
        })
        socket.on('login', function (names) {
            items = []
            names.forEach(function (name, idx) {
                items.push(`<li>${name}已登录</li>`)
            })
            content.innerHTML = items.join('')
        })
    </script>
</body>
</html>
```

客户端的处理需要引入socket.io.js文件，这个无需在本地存在，只要服务器端使用了nodeJS的socket.io模块，就可以通过script标签获取到。如果正确的获取到了socket.io.js这个文件，那么其将生成一个io全局对象，这个对象就可以发起websocket连接请求，连接成功就能获取到一个socket对象，同样可对on监听事件和emit派发事件来和服务器端进行数据通信。前后端的书写都很一致，是不是很爽。

当然这只是一个简单的例子，socket.io提供的api还有很多需要学习的，如果有兴趣，就可以深入下去。官方的api文档还是很全的，可以查阅一下[http://nodejs.cn/api/(中文文档)](http://nodejs.cn/api/)。
