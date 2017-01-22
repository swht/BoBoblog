---
title: 2017-01-14-Nodejs全栈开发之路-nodejs-http-post请求
date: 2017-01-14 11:50:50
tags: Nodejs全栈开发之路
---
Author:@南非波波

### http

![URl](http://i.imgur.com/OHtEUi6.jpg)
- serve.js
```
var http = require('http');
var fs = require('fs');
http.createServer(function (request, response) {
   if(request.url == '/'){
       fs.createReadStream('./login.html').pipe(response);
   }else if(request.url == '/login'){
       var str = '';
       var buffers = [];
       request.on('data',function (data) {
           buffers.push(data);
           console.log(str);
       });
       request.on('end',function () {
           var result = Buffer.concat(buffers);
           response.end(buffers.toString());
       });
   }else{
       response.end('404');
   }
}).listen(8000);
console.log('服务监听在8000端口');
```
- login.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>login</title>
</head>
<body>
<form action="/login" method="post">
    用户名 <input type="text" name="username">
    密码 <input type="password" name="password">
    <input type="submit" value="登录">
</form>

</body>
</html>
```

### ajax
- 创建ajax对象
```
var XHR = new XMLHttpRequest();
```
- 打开请求
```
XHR.open(method,url,async,user,password);
```

    - method    http方法，例如：post、get、put等,大小写不敏感
    - url       请求的url地址，可以为绝对地址也可以为相对地址
    - async     布尔型，指定此请求是否为异步方式，默认为true。如果为真，当状态改变时会调用onreadystatechange属性指定的回调函数
    - user      如果服务器需要验证，此处指定用户名，默认是undefined
    - password  验证信息中的密码部分，默认是undefined
- 注册回调监听

    - readyState XMLHttpRequest对象的状态

        - 0(为初始化)   对象已建立，但是尚未初始化(尚未调用open方法)
        - 1(初始化)     对象已建立，尚未调用send方法
        - 2(发送数据)   send方法已调用，但是当前的状态及http头未知
        - 3(数据传送中)  已接收部分数据，因为响应及http头不全，这时通过responseBody和responText获取部分数据会出现错误。
        - 4(完成)       数据接收完成，此时可以通过responseBody和responText获取完整的回应数据