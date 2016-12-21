---
title: 2016-12-21-Nodejs全栈开发之路-Nodejs基础(console、process)
date: 2016-12-21 18:03:50
tags: Nodejs全栈开发之路
---
Author:@南非波波



### 一、console控制台输出

#### 1. console.log()

    信息输出,主要负责在程序调试中打印某个变量的值、输出程序逻辑注释
	示例：
	console.log('Server running at http://127.0.0.1:8000/'); //服务监听在127.0.0.1:8000

	var data = process.stdin.read(); //读取用户输入的信息并赋给变量data
	console.log(data); //输出用户输入的信息

#### 2. console.info()

	等级日志--一般信息输出，主要是一般信息的打印输出到控制台或指定日志文件
	示例：
	#test.js
		console.info('this is info！'); //node test.js 1>log.txt 
	#log.txt
		this is info！

#### 3. console.warn()

	等级日志--警告信息输出，主要是指具有警告信息的日志信息打印输出到控制台或指定日志文件，该类信息被打印在控制台时表现出不同颜色
	#test.js
		console.info('this is warn！'); //node test.js 2>err.txt 
	#log.txt
		this is warn！

#### 4. console.err()

	等级日志--错误信息输出，主要是指具有错误信息的日志信息打印输出到控制台或指定日志文件，该类信息被打印在控制台时表现出不同颜色
	#test.js
		console.info('this is error！'); //node test.js 2>err.txt 
	#log.txt
		this is error！

#### 5. 等级日志信息输出总结
	
	等级日志，按照不同等级在控制台输出呈现不同颜色。也可以根据等级对日志进行分流输出到不同的文件：
	示例：
	#test.js
		console.info('this is info！');
		console.info('this is warn！'); 
		console.info('this is error！');
		//node test.js 1>log.txt 2>err.txt 
	#log.txt
		this is info！
	#err.txt
		this is warn！
		this is error！
	1 info信息被输入到日志文件log.txt
	2 warn/error信息被输入到日志文件err.txt

### 二、__dirname/__filename/cwd

	console.log(__filename);//输出当前执行文件所在绝对路径+文件名
	console.log('__dirname:',__dirname);//输出当前执行文件所在的绝对路径
	console.log('process.cwd():',process.cwd()); //cwd输出执行node命令所在目录的绝对路径


### 三、process

#### 1. process.stdout

	标准信息输出，不建议使用字段出来日志的输出
	process.stdout.write('this is stdout write\n'); //标准信息输出

#### 2. process.stderr

	标准错误信息输出
	process.stderr.write('this is stderr write\n'); //标准错误输出

#### 3. process.stdin

	标准输入
	process.stdin.setEncoding('utf-8'); //设置process输出的编码格式
	process.stdin.on('data',function(data){
		console.log(data);
	});//on监听事件，当用户输入信息，通过事件监听将信息传递给data，通过回调函数输出

	示例：
		process.stdin.setEncoding('utf-8'); //设置process输出的编码格式
		process.stdin.on('readable',function(){
		var data = process.stdin.read(); //读取用户输入的信息
			console.log(data);
		})；

#### 4. process.on

	事件监听
		exit 监听程序的正常退出
		SIGINT 监听键盘的输入

	示例：

	#test.js
		process.stdin.setEncoding('utf-8'); //设置process输出的编码格式
		process.stdin.on('data',function(data){
			console.log(data);
		});
		process.on('exit',function(){
			console.log('exit is exit');
		});//监听事件，程序正常退出的时候打印exit信息
		process.on('SIGINT',function(){
			console.log('exit');
			process.exit(); //监听正常退出事件，打印'exit is exit'并退出
		});//监听键盘输入，当用户按下Ctrl+c之后，程序不会退出
	#node test.js 
	#用户输入信息，直接打印相应信息，当用户按下Ctrl+c之后，程序会打印exit退出信息，在走到process.exit()之后会打印'exit is exit'。

#### 5. process.argv

	获取执行执行程序的命令和程序名，以及用户输入的其他参数
	#test.js
		console.log(process.argv); //获取执行程序时用户输入的一个配置参数 process.argv[2] process.argv[3]

![](http://i.imgur.com/QqveM9L.jpg)	