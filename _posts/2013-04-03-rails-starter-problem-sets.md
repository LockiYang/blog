---
layout: post
title: Rails新手常见问题集锦
categories: [rails]
tags: [ruby, rails]
---

首次运行Rails环境总是会遇到这样的、那样的问题，怒开一贴，记录之。

1、 Ubuntu安装好Rails环境后，重启进入系统打开命令行，发现`rails`、`bundle`命令没有了。    

错误原因：这是由于Ubuntu终端的设置问题。

解决办法：打开终端，菜单选项依次选择，*Edit*->*Profile Preferences*->*Command*，勾选上*Run command as a login shell*。重启终端就可以了。
	
2、 执行`rails server`报错

>Could not find a JavaScript runtime	

错误原因：Ubuntu默认缺少Javascript引擎。    

解决办法： 

	# 在项目Gemfile文件中添加	
	gem 'execjs'
	gem 'therubyracer'
	# 执行
	bundle install

一般只需要第一个项目安装即可，以后的项目不用重复安装。
	
3、 执行`bundle install`卡住很久

错误原因：这个问题一般是来源于不可抗拒的**GFW**。

解决办法：

	# 把系统的sources改成淘宝源，一般安装阶段已经完成
	gem sources --remove "https://rubygems.org/"
	gem sources -a "http://ruby.taobao.org/"
	# `Ctrl+C`停止执行，把项目Gemfile的source行改成
	source "http://ruby.taobao.org"

重新执行`bundle install`
	
未完待续...