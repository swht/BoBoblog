---
title: 2017-01-14-Nodejs全栈开发之路-nodejs模块fs(可读流和可写流)
date: 2017-01-14 09:50:50
tags: Nodejs全栈开发之路
---
Author:@南非波波

### fs模块

- 可读流createReadStream

    可读流触发的事件
    - data  绑定一个data事件监听器会将流切换成流动模式，数据会被尽可能的读出
    - end   该事件会在读完数据后触发
    - error 当数据接收时发生错误时触发

    可读流的方法
    - setEncoding   指定编码
    - pause         通知对象停止触发data事件
    - resume        通知对象恢复触发事件
    - pipe          设置管道，将可读流里的内容导入到参数指定的可写流里

```
    var fs = require('fs');
    var rs = fs.createReadStream('./README.md',{
        flags:'r',//表示打开文件进行读操作
        // encoding:'utf8' //得到的数据是Buffer对象。指定编码输出
        //下面两个参数可以进行截取读取，取值范围str[i] :start<= i <= end
        start:1, //读取文件内容的起始位置
        end:3   //读取文件内容的结束位置
    });
    rs.setEncoding('utf8');//或者可以在创建流之后进行指定编码类型
    //流是EnventEmitter的子类
    rs.on('data',function (data) {
       console.log(data); //输出：<Buffer 23 32 30 31 36 31 32 6e 6f 64 65 0d 0a>
                         //输出:#201612node
    });
    //当读完文件的时候会触发end事件
    rs.on('end',function () {
        console.log('读完了!');
    });

    //当读取文件出错的时候会监听到，输出相应的错误
    rs.on('error',function (err) {
        console.log('出错了:',err);
    });
```

```
    var fs = require('fs');
    var rs = fs.createReadStream('./README.md',{
        flags:'r',
        encoding:'utf8',
        highWaterMark:5
    });
    rs.on('data',function (data) {
        console.log(data);
        rs.pause();//暂停触发data事件
    });
    setTimeout(function () {
        rs.resume();//5秒钟之后继续触发data事件
    },5000);
    rs.on('end',function () {
        console.log('读取完毕!');
    });
    rs.on('error',function (err) {
        console.log('读取数据出错:',err);
    });
    //输出:#2016
    //12nod
```

- 可写流

```
    var fs =require('fs');
    function copy(src,dst) {
        var rs = fs.createReadStream(src);
        var ws = fs.createWriteStream(dst);
        rs.on('data',function (data) {
            var flag = ws.write(data);
            if(!flag){
                rs.pause();
            }
        });
        ws.on('drain',function () {
            rs.resume()
        });
        rs.on('end',function () {
            ws.end();
        });
        rs.on('error',function (err) {
            console.log(err);
        })
    }
    copy('1.txt','2.txt');

    //复制，同copy函数
    var rs = fs.createReadStream('./1.txt');
    var ws = fs.createWriteStream('./3.txt');
    rs.pipe(ws);
```