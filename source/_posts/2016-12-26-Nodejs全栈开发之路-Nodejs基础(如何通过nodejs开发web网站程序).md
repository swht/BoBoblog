---
title: 2016-12-26-Nodejs全栈开发之路-Nodejs基础(如何通过nodejs开发web网站程序)
date: 2016-12-26 11:50:50
tags: Nodejs全栈开发之路
---
Author:@南非波波

### 一、http模块

	创建一个服务器监听指定端口，浏览器等客户端会通过该服务器端获取到相应的信息

	示例：
	#test.js
		var fs = require('fs');
		var http = require('http');
		//创建server
		http.createServer(function(request,response){
			fs.readFile('E:/projects/all-code/笔记/js && html/Html模板主题/登录模板/login5/index.html','utf-8',function(error,data){
				if(error){
					console.log("读取文件出错！");
					response.end('页面获取失败!');
				}else{
					console.log("读取文件成功,返回页面数据!");
					response.end(data);
				}
				
			});
		}).listen(8000); //绑定指定端口号
		
		console.log('监听8000端口...');
	#控制台会首先打印监听8000端口...,当浏览器访问localhost:8000的时候，控制台打印“读取文件成功，返回页面数据！”，页面返回指定的html页面

### 二、url模块

#### 1. parse

	获取url的相关信息
	#url.parse(request.url)  http://localhost:8000/query.html
		Url {
		  protocol: null,
		  slashes: null,
		  auth: null,
		  host: null,
		  port: null,
		  hostname: null,
		  hash: null,
		  search: null,
		  query: null,
		  pathname: '/query.html',
		  path: '/query.html',
		  href: '/query.html' }
		Url {
		  protocol: null,
		  slashes: null,
		  auth: null,
		  host: null,
		  port: null,
		  hostname: null,
		  hash: null,
		  search: null,
		  query: null,
		  pathname: '/favicon.ico',
		  path: '/favicon.ico',
		  href: '/favicon.ico' }
	
	示例：
		var fs = require('fs');
		var http = require('http');
		var url = require('url');
		http.createServer(function(request,response){
			var urlObj = url.parse(request.url);
			console.log(urlObj);
			var pathname = urlObj.pathname;
			fs.readFile(pathname.substr(1),'utf-8',function(error,data){
				response.end(pathname.substr(1));
			});
			
		}).listen(8000);
		console.log("端口监听8000端口...");




	示例：

		var fs = require('fs');
		var http = require('http');
		var url = require('url');
		http.createServer(function(request,response){
			var urlObj = url.parse(request.url);
			console.log(urlObj);
			var pathname = urlObj.pathname;
			if(pathname === '/'){
				requestAndresponse('/index.html',response);
			}else if(pathname === '/test.html'){
				response.end('msg:{hostname:"intel",IP:192.168.13.1}');
			}else{
				requestAndresponse(pathname,response);
			}
		
			
		}).listen(8000);
		console.log("端口监听8000端口...");
		
		function requestAndresponse(pathname,response){
				fs.readFile(pathname.substr(1),'utf-8',function(error,data){
				if(error){
					response.writeHead(404);
					response.end('file not found!');
				}else{
					response.end(pathname.substr(1));
				}
				
			});
		};