---
layout: post
title: Ubuntu 12.04下安装Rails开发环境
categories: [rails]
tags: [ruby, rails, ubuntu]
---

最近接触了一个使用Rails开发的项目，但是按照Ruby-China社区Wiki里面的[安装指南](http://ruby-china.org/wiki/install_ruby_guide)进行安装出现了问题，试了几次都以失败告终。特地在虚拟机下折腾了两个小时，记录过程如下。

# 环境

VMware workstation 9.0	
Ubuntu 12.04

# 一、安装RVM

1、 安装curl    	

	sudo apt-get install curl
	
2、 安装git    

	sudo apt-get install git
	
3、 安装RVM


	curl -L https://get.rvm.io | bash -s stable
	
4、 载入RVM环境

	source ~/.rvm/scripts/rvm
	
5、 切换到taobao镜像站

	sed -i -e 's!ftp.ruby-lang.org/pub/ruby!ruby.taobao.org/mirrors/ruby!' $rvm_path/config/db
	
# 二、解决RVM依赖

在terminal输入    

	rvm requirements

把列出的‘missing required packages’依次用
	
	sudo apt-get install

安装，直到

	rvm requirements

不出现‘missing required packages’报错为止。

# 三、用RVM安装Ruby

	# 列出可用的Ruby版本
	rvm list known
	# 选择其中一个版本安装
	rvm install 2.0.0
	# 设置Ruby版本
	rvm 2.0.0 --default
	# 测试是否安装正确
	ruby -v
	gem -v
	# 更换rubygems源
	gem source -r https://rubygems.org/
	gem source -a http://ruby.taobao.org
	
当`rvm install 2.0.0`时，报了一个错，“Installtion of rubygems did not complete successfully”。所以我们手动更新rubygems。
	gem update --system
	gem update
	
# 四、安装rails环境
	gem install bundler rails
然后测试安装是否正确
	bundle -v
	rails -v
经过测试，rails环境安装正确，欢迎来到Rails世界！

# 五、安装RubyMine4.5
作为一个新手，选择一款好的IDE，往往会有事半功倍的效果，这里推荐一下[RubyMine](http://www.jetbrains.com/ruby/)。安装过程如下：

1、 RubyMine需要JDK环境的支持，所以先下载安装SUN JDK。[下载地址]()。    

2、 解压缩。这里以解压到`/home/colby/opt/jdk-7/`下为例。    

3、 设置环境变量，用文本编辑器打开`~/.bashrc`。添加如下行：    

	JAVA_HOME=/home/colby/opt/jdk-7		# 注意更改成实际解压缩目录 
	JRE_HOME=${JAVA_HOME}/jre  
	CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
	PATH=${JAVA_HOME}/bin:$PATH
		
4、 关闭终端，重新打开，运行：    

	sudo update-alternatives --install /usr/bin/java java /home/colby/opt/jdk-7/bin/java 300  
	sudo update-alternatives --install /usr/bin/javac javac /home/colby/opt/jdk-7/bin/javac 300
	# 然后，运行
	sudo update-alternatives --config java 	# 在出现的选项中选择刚刚安装的JDK

5、 验证。输入`java -version`看JDK是否改为刚刚安装的版本。
		


	