# blog
南非波波个人博客项目代码

参考：[http://www.jianshu.com/p/465830080ea9](http://www.jianshu.com/p/465830080ea9 "参考链接")
###部署

*正式安装Hexo*

	npm install -g hexo  #全局安装Hexo

	hexo init  #初始化

*生成静态页面*

	hexo generate #或者hexo g

*本地启动测试*

	hexo server  # 浏览器输入http://localhost:4000

###配置Github

建立Repository

	创建的github项目 swht.github.io

配置_config.yml

	deploy:
	  type: git
	  repo: https://github.com/swht/swht.github.io.git
	  branch: master
	  message: Site updated:{{ now('YYYY-MM-DD HH:mm:ss') }}
	  name: swht
	  email: qingbo.song@gmail.com

安装deployer插件

	npm install hexo-deployer-git --save

本地同步到github

	hexo deploy

	#执行步骤

	hexo clean

    hexo generate

    hexo deploy


###一些常用命令：

	hexo new "postName" #新建文章
	
	hexo new page "pageName" #新建页面
	
	hexo generate #生成静态页面至public目录
	
	hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
	
	hexo deploy #将.deploy目录部署到GitHub
	
	hexo help # 查看帮助
	
	hexo version #查看Hexo的版本


###静态页面转移

	生成的静态页面存放在public，只需要将public目录下面的文件传送到指定服务器路径，即可配置成一个静态的网页博客。


###文章md格式

	---
	title: python
	date: 2015-12-16 15:30:00
	tags: Python开发之路
	---
	Author:@南非波波

存放路径source/_posts,建议使用hexo new "postName"创建新的文章