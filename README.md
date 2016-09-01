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
	  repo: git@github.com:swht/swht.github.io.git
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

###文章存放路径

	存放路径source/_posts,建议使用hexo new "postName"创建新的文章


###问题

	在配置deploy的过程中可能会遇到问题，
	首先使用https://gihub.com/swht/swht.github.io.git的方式提交的会遇到无效的信息格式的提示；
	看到网上会有很多朋友推荐使用git@github.com:swht/swht.github.io.git的方式
	使用git协议的必须进行配置ssh-key：

		使用git Bash工具打开：
			cd ~/.ssh/
			ssh-keygen -t rsa -C "qingbo.song@gmail.com"
			#默认回车，不设置密码
			#在~/.ssh/会产生两个文件 id_rsa   id_rsa.pub
			#复制id_rsa.pub文件中的内容，即公钥
			#在github上创建ssh keys，将上述公钥粘贴进去
			#配置账户
			git config --global user.name "swht"
			git config --global user.email "qingbo.song@gmail.com"			
			#测试是否配置成功
			ssh -T git@github.com

详细说明请参考：[github免密钥配置](http://jingyan.baidu.com/article/a65957f4e91ccf24e77f9b11.html "github免密钥配置")