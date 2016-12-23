---
title: 2016-12-22-Nodejs全栈开发之路-Nodejs基础(模块、包、文件读取、非阻塞异步模式)
date: 2016-12-22 10:30:50
tags: Nodejs全栈开发之路
---
Author:@南非波波


### 一、模块

	任何具有特定功能的js文件都可以被封装成模块。
	通过require将模块引入。
	使用exports将函数全局化，使得其他文件可以通过引入该模块引用函数。

	示例：
	#test.js
		var sum = require('./sum'); //导入自定义模块
		var data = sum['sum'](2,3);
		var data1 = sum['product'](3,5);
		console.log(data); //5
		console.log(data1); //60
	#sum.js
		module.exports = {
		    sum: function (startnum, endnum) {
		        var sumnum = 0
		        for (i = startnum; i <= endnum; i++) {
		            sumnum += i;
		        }
		        return sumnum;
		    },
		    product: function (startnum, endnum) {
		        var productnum = 1;
		        for (j = startnum; j <= endnum; j++) {
		            productnum *= j;
		        }
		        return productnum;
		    }
		}

### 二、包管理

	使用nodejs的包管理工具npm，可以从网络上下载并安装已经被封装好实现某个功能的接口模块。
	开发者也可以将自己的模块封装成包上传到网络供其他开发者使用。

### 三、文件相关

	使用fs对文件进行操作

	示例：
	#test.js //异步回调读取文件内容
		var fs = require('fs');
		fs.readFile('./err.txt',function(err,data){
			if(err){
				console.log('读取文件出错!',err);
			}else{
				console.log('文件读取成功:',data.toString());//如果不加编码格式，这里data输出的是16进制的字符串
			}
		
		});
		// fs.readFile('./err.txt','utf-8',function(err,data){
		// 	if(err){
		// 		console.log('读取文件出错!',err);
		// 	}else{
		// 		console.log('文件读取成功:',data);
		// 	}
		
		// });
	#输出：
		文件读取成功: warn this！
		error this！
	#test.js  //同步读取文件内容
		var fs = require('fs');
		var data = fs.readFileSync('./err.txt','utf-8');
		console.log('文件读取成功:',data);
	#输出：
		文件读取成功: warn this！
		error this！
### 四、 path模块

#### 1. path.normalize(p)

	规范化路径，注意'..' 和 '.'。

	示例：
	#test.js
		var path = require('path');
		filepath = './upload-aws/app.js';
		console.log( path.normalize(filepath)); //返回指定文件的规范的路径
		console.log('normalization : ' + path.normalize('/test/test1//2slashes/1slash/tab/..')); 
	#输出：
		upload-aws/app.js
		normalization : \test\test1\2slashes\1slash

#### 2.	path.join([path1][, path2][, ...])
	
	用于连接路径。该方法的主要用途在于，会正确使用当前系统的路径分隔符，Unix系统是"/"，Windows系统是"\"。
	
	示例：
		var path = require('path');
		console.log('join path : ' + path.join('/test', 'test1', '2slashes/1slash', 'tab/ddd/', '..'));//join path : \test\test1\2slashes\1slash\tab
		console.log('join path : ' + path.join('/test', 'test1', '2slashes/1slash','..'));//join path : \test\test1\2slashes
	#该方法会把最后一个正常的路径名称识别为文件，不会拼接在路径中。这一点儿很奇怪

#### 3. path.resolve([from ...], to)
	
	将 to 参数解析为绝对路径。
	
	示例：
		var path = require('path');
		filepath = './upload-aws/app.js';
		console.log(path.resolve(filepath)); //将指定文件相对路径转换成绝对路径
	输出：
		E:\projects\all-code\node\upload-aws\app.js 

#### 4.	path.isAbsolute(path)

	判断参数 path 是否是绝对路径。
	返回值是：true false,根据返回值判断是否是绝对路径

#### 5.	path.relative(from, to)

	用于将相对路径转为绝对路径。from是指定源路径，to是指定的目的路径，输出结果是从源路径到目的路径的相对路径。

	示例：
		console.log(path.relative('upload-aws/app.js','swht.github.io')); //..\swht.github.io
		解析：upload-aws 和swht.github.io在同一级路径，从upload-aws路径进入到swht.github.io路径，需要的相对路径是..\swht.github.io
	
#### 6. path.dirname(p)
	
	返回路径中代表文件夹的部分，同 Unix的 __dirname 命令类似,但是_dirname获取的是文件所在位置的绝对路径

#### 7.	path.basename(p[, ext])

	返回路径中的最后一部分。同 Unix 命令 bashname 类似。

#### 8.	path.extname(p)

	返回路径中文件的后缀名，即路径中最后一个'.'之后的部分。如果一个路径中并不包含'.'或该路径只包含一个'.' 且这个'.'为路径的第一个字符，则此命令返回空字符串。

#### 9.	path.parse(pathString)

	返回路径字符串的对象。

	示例：
		var path = require('path');
		filepath = './upload-aws/app.js';
		console.log(path.parse(filepath));
		// { root: '',
		//   dir: './upload-aws',
		//   base: 'app.js',
		//   ext: '.js',
		//   name: 'app' }	

#### 10. path.format(pathObject)

	从对象中返回路径字符串，和 path.parse 相反。

	示例：
		data = { root: '',
		  dir: './upload-aws',
		  base: 'app.js',
		  ext: '.js',
		  name: 'app' }
		  console.log(path.format(data));
	输出：./upload-aws\app.js

#### 11. path.sep

	平台的文件路径分隔符，'\\' 或 '/'。

#### 12. path.delimiter

	平台的分隔符, ; or ':'.

#### 13. path.posix

	提供上述 path 的方法，不过总是以 posix 兼容的方式交互。

#### 14. path.win32

	提供上述 path 的方法，不过总是以 win32 兼容的方式交互。