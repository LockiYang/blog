---
layout: post
title: Hello Jekyll!
categories: [toolsoft]
tags: [jekyll, github, markdown]
---

国际惯例，每到一个新的平台上第一个例程当是Hello World。这里也不例外，写下这篇Hello Jekyll记录本站搭建过程，姑且认为是一篇教程吧，其实纯粹是零零散散记录了一下自己的经验。说到独立博客程序，见得最多的要属Wordpress了，我并属于不是从WP平台转到Jekyll的，我也并不反感WP，那么为何放着WP这样的上乘之作不用，跑过来折腾Jeklly呢？我总结了几点：

- 我只想简单的记录些东西，不希望被诸如模板、主题、插件等干扰
- 我害怕数据库丢失，我也懒得备份
- 我经常会使用[Markdown](http://daringfireball.net/projects/markdown/)做一些笔记
- 没错，这是个静态站点
- ...	

如果这也正好契合于你，那么，你可以继续往下看...

# 一、Github与Jekyll

如果你是程序员就一定听说过[Github](https://github.com/)，如果没听说过也没关系。简单的来说，它是一个代码云存储的平台，是一个具有版本管理功能的代码仓库，用于存放项目的源代码。Github设计了一个功能叫Github Pages功能，允许用户自定义项目首页来替代看得让人头痛的源码列表，就跟我们常见的README一样。简单的来说，Github Pages就是托管于Github服务器上的静态网页。Github Pages有两种生成方式：

- 利用Github提供的模板自动生成网页
- 用户上传自己编写的静态网页

要借助Github建站，我们显然是希望利用第二种方式，上传自己编写的页面，但是我们要自己编写一个静态的博客系统，工作量大而且不灵活。所以我们要借助Jekyll。[Jekyll](http://jekyllrb.com/)是一个静态站点生成器，根据源码生成静态文件。而Github Pages的后台引擎就是Jekyll。看到这里，思路就比较明显了，先在本地编写符合Jeklly规范的网站源码，然后上传到Github，由Github托管并由Github Pages生成整个网站。Jekyll有自己的一套语法规范，具体参考其[官方文档](https://github.com/mojombo/jekyll/wiki)。当然，我们不必自己动手编写Jekyll网站源码，我们甚至完全不用了解Jekyll语法规则。使用Jekyll生成的站点，[这里](https://github.com/mojombo/jekyll/wiki/Sites)已经有一大把大把了，其源码都托管在Github上，我们只需要挑选自己喜欢的clone然后修改诸如站点名称、作者信息等站点信息。剩下的就是用方便的Markdown语言写文章，并运用一点点的git知识发布文章了。

# 二、Windows下搭建本地Jekyll环境

1、 下载安装[Github for Windows](http://windows.github.com/)。    

2、 下载安装[RubyInstaller](http://rubyinstaller.org/downloads/)，然后可以在命令行终端下输入`gem update --system`来升级gem。    

3、 下载安装[DevKit](http://rubyinstaller.org/add-ons/devkit/)，它是Windows下编译和使用本地C/C++扩展包的工具。

	cd C:\DevKit
	ruby dk.rb init
	ruby dk.rb install

4、 使用RubyGems的方式安装Jekyll。

	gem install jekyll	
	jekyll --version		# 检查是否安装成功

5、 一般而言还要安装Rdiscount这个用来解析Markdown标记语言的包。

	gem install rdiscount

完成以上步骤之后，Jekyll的本地环境就配置完成了。我们可以Clone我们之前说的Github上托管的[Jekyll项目](https://github.com/mojombo/jekyll/wiki/Sites)，当然也可以使用[Jekyll-bootstrap](https://github.com/plusjade/jekyll-bootstrap)。我们可以在命令行终端进入Clone的项目目录中，运行`jekyll --server`进行本地调试，在浏览器输入http://127.0.0.1:4000可以进行本地访问。

# 三、推送本地项目到Github

Jekyll生成的网站仅仅能本地访问显然不是我们想要的，我们要把它推送到互联网上。注册Github帐号，当然这是必须的，并且需要验证邮箱，否则可能无法正常使用Github Pages。以下假定用户名为username，用户email为g@gmail.com。

Github上能托管静态网页并提供访问的仓库分为两种。

（1）仓库名为“username.github.com”，通过http://username.github.com访问，位于master分支。

（2）仓库名任意合法皆可，如“demo”，通过http://username.github.com/demo形式访问，必须位于gh-pages分支。

首先我们要对本地git环境做如下配置。
	
	git config --global user.email "g@gmail.com"
	git config --global user.name "username"

1、 仓库“username.github.com”初始化上传步骤如下：

	#在命令行下进入要推送的项目目录下
	git init
	git add .
	git commit -m "信息"
	git remote add origin https://github.com/username/username.github.com.git
	git push -u origin master

2、 普通仓库“demo”初始化上传步骤如下：

	git init
	git checkout --orphan gh-pages
	git add .
	git commit -m "信息"
	git remote add origin https://github.com/username/username.github.com.git
	git push -u origin gh-pages

两种情况的区别在于方式(1)上传的master分支，方式(2)上传到gh-pages分支。更多Github知识请参见[这里](http://rogerdudler.github.com/git-guide/index.zh.html)。至此，我们已经可以通过Github提供的二级域名来访问我们的网站了。

## 绑定域名

如果你不想用Github提供的这个二级域名，你也可以绑定自己的域名。具体方法是在仓库根目录下新建一个名为CNAME的文本文件，里面写入你要绑定的域名如：example.com或者blog.example.com。如果要绑定的是顶级域名，则DNS要新建一条A记录，指向204.232.175.78。如果要绑定的是二级域名，则DNS要新建一条CNAME记录，指向username.github.com。到这里，就可以使用example.com或blog.example.com来访问仓库“username.github.com”了，同时也可以用example.com/demo或blog.example.com/demo目录的形式来访问“demo”仓库。

# 四、使用Markdown写文章并发表

Jekyll支持Markdown，这对我们来说是一个好消息。Markdown的目标是实现了「易读易写」的网络的书写语言，具体语法看[这里](http://wowubuntu.com/markdown/)。本文的Markdown格式部分原文如下：

	---
	layout: post
	title: Hello Jekyll!
	categories: [Software]
	tags: [Jekyll, Github, Markdown]
	---

	国际惯例，... 我总结了几点：

	- 我只想简单的记录些东西，不希望被诸如模板、主题、插件等干扰
	- 我害怕数据库丢失，我也懒得备份
	- 我经常会使用[Markdown](http://daringfireball.net/projects/markdown/)做一些笔记
	- 没错，这是个静态站点
	- ...	

	如果这也正好契合于你，那么，你可以继续往下看...

	# 一、Github与Jekyll

	如果你是程序员就一定听说过[Github](https://github.com/)，... Github Pages有两种生成方式：

	...

用Markdown格式在相关目录下写下一篇新文章之后，我们利用`git`命令进行推送发表。
		
	git add .
	git commit -m "a new post"
	git push origin master(gh-pages) 	# master和gh-pages的区别上文已有说明
		
# 五、问题集锦

1、 `jekyll --server`在本地测试出现以下错误
>Liquid error: invalid byte sequencd in GBK
	
临时解决：在执行jekyll命令前，将当前控制台代码格式转换为UTF-8
	
	$export LC_ALL=en_US.UTF-8
	$export LANG=en_US.UTF-8
		
永久解决：把这两个变量添加到操作系统环境变量中。

2、 `jekyll --server`时出现编码问题
>C:/Ruby193/lib/ruby/gems/1.9.1/gems/jekyll-0.12.0/lib/jekyll/convertible.rb:29:in `read_yaml':invalid byte sequence in GBK
	
解决办法：找到ruby目录下gems/jekyll-0.12.0/lib/jekyll/convertible.rb29行修改为下面内容：
			
	self.content = File.read(File.join(base,name),:encoding => "utf-8")
			
3、 每次`git push`都要输入用户名、密码

- 检查~/.ssh文件夹，看rsa密钥文件是否存在
- 检查Github控制面板的SSH Keys，看公钥是否添加。
- 进入命令行切换到项目目录，输入`git remote -v`，如果显示
		
>origin https://...		
>origin https://...
		
代表使用的是https协议，没有使用ssh协议，解决办法如下：

	git remote rm origin									# 删掉现有远程仓库地址
	git remote add origin git@github.com:username/demo.git 	# 然后添加使用ssh协议的地址

- 再次`git push`，不再要求提供密码。
		
4、 怎么删除Github上远程分支？
由于上述“username.github.com”、“demo”两个仓库要被`push`到特定的分支，操作失误难免会出错，使其出现两个分支。删除远程分支的原理是`push`一个空的项目到该分支上：
	
	remote_repo="https//github.com/username/demo.git"	# 这是我想删除分支所在仓库地址
	remote_branch="master"								# 这是我想删除分支的名字
	mkdir /tmp/git-empty								# Linux命令，新建名为git-empty文件夹
	cd /tmp/git-empty									# 在命令行下切换到该文件夹
	git init 											# 初始化Git
	git push $remote_repo:$remote_branch
			
先到这里，未完待续...




