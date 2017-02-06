---
title: 2016-12-31-Nodejs全栈开发之路-git基础
date: 2017-12-31 11:50:50
tags: Nodejs全栈开发之路
---
Author:@南非波波

### 课件地址

- https://github.com/zhufengzhufeng/zhufengnodejs_node12

#### 一、git本地操作
##### git特点

- 分布式，svn(集中式)
- 回到过去，回到未来(更改状态点)
- 多端共享
- 解决冲突(冲突被显示出来,但是解决需要开发者手动去处理)
- 控制版本

##### 安装git

需要选择在cmd命令下使用unix简单命令

##### cd 
charge dir 改变目录

##### pwd
打印当前工作目录，print working dir

##### 配置git config
如果没有配置过git是不能进行提交操作的
```
git config --global user.name 'github username'
git config --global user.email 'mail'
git config --list //查看所有配置`
```
##### 初始化git
```
git init
```
##### git有三个区

- 工作区 
- 暂存区/缓存区
- 历史区/版本库

##### 查看提交版本

```
git log --oneline  //查看日志一行
```
> git log 命令是只能查看当期那版本以前的历史记录

##### 查看所有的操作历史
```
git reflog
```
##### 代码回滚

```
git checkout <filename> //从暂存区拉回历史文件
git checkout .

git reset HEAD <filename>
git reset HEAD . //回滚已更改文件

git reset --hard <版本号>//回滚版本
```
##### 一步提交

- 缺点：如果有文件没有在历史提交过，则该命令提交失败
```
git commit -a -m "message"
```
##### 在历史区中查询关键字

```
git log --grep=<关键字>
git log --author=<作者名字>
```
##### 代码比较
- 工作区和暂存区
```
    git diff
```
- 暂存区和历史区
```
    git diff --cached
```
- 工作区和历史区
 ```
    git diff <分支名>
``` 
#### 分支
- 主要是写代码时，不会影响主代码。
##### 查看分支
```
    git branch
```
##### 创建分支
```
    git branch dev //创建本地分支
    git checkout dev //切换分支
    git checkout -b dev //创建并切换到新的分支
```
##### 删除分支
```
    git branch -D dev //删除指定分支
```
##### 分支合并
```
    git merge dev //在主分支，合并dev分支
```
##### 解决冲突
```
    只能手动确认提交
```

### 二、将本地'历史'提交到github版本库

#### 初始化目录
```
    git init
```
#### 创建忽略文件
```
    touch .gitignore
      .idea
      node_modules
      .DS_Store
      bower_components
```
#### 将本地的内容推送到远程仓库
- 如果远程仓库有内容，需要先从远程仓库拉取到本地，然后再将本地的内容推送到远程
- 增加远程仓库
```
    git remote add origin <URL>
```
- 查看远程仓库的列表
```
    git remote -v
```
- 移除远程仓库
```
    git remote rm 仓库别名
```
- 推送到远端
```
    git push origin master -u //-u 下次默认使用origin远程仓库
```
#### 远程代码与本地代码出现冲突

- 先解决冲突再进行本地推送到远程

#### 强制将本地代码覆盖掉远程代码
```
    git push origin master -f 
```
#### 在github上挂载一个静态页面
```
    git push origin gh-pages
```
- 挂载静态页必须将代码提交到固定的分支上(gh-pages)
- 将这个分支推送到github上
- 在settings上可以看到面网址中的页静态网页的地址