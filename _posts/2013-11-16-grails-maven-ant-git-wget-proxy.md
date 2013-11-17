---
layout: post
title: grails、maven、ant、git、wget代理设置
categories: [toolsoft]
tags: [grails, maven, ant, git, wget, proxy]
---

公司网络环境不容乐观，grails、maven、ant、git、wget等都无法正常联网，只能使用代理来解决这个问题。

## 代理转换
因为手边的外网代理为socks5，所以先用[privoxy](http://www.privoxy.org/)这个小工具先把socks5代理转换为更加通用的http代理。安装后打开privoxy的配置文件找到下面这行，并修改为自己的socks5地址和端口。

	forward-socks5   /               ip:port
	
privoxy默认使用`127.0.0.1:8118`作为http代理服务器地址。

## grails代理
在命令行输入：

	grails add-proxy name "--host=ip" "--port=port" "--username=user" "--password=pwd"
	grails set-proxy name
	
请修改命令中name、ip、port。用户名和密码如果没有就不用填写。

## maven代理
修改maven配置文件*settings.xml*中相应代理设置。

	<proxies>
    <proxy>
        <id>optional</id>
        <active>true</active>
        <protocol>http</protocol>
        <username></username>
        <password></password>
        <host>ip</host>
        <port>port</port>
        <nonProxyHosts>localhost|127.0.0.1</nonProxyHosts>
    </proxy>
	</proxies>
	
## ant代理
修改项目中的*build.xml*，加入代理的target，并设置要连外网的target依赖于代理target，实例如下：

	<target name="proxy">  
    	<property name="proxy.host" value="ip"/>  
    	<property name="proxy.port" value="port"/>
    	<!-- 如代理需要用户名密码请加入下面两行 -->
    	<property name="proxy.user" value="username"/>
    	<property name="proxy.pass" value="password"/>
    	<setproxy proxyhost="${proxy.host}" proxyport="${proxy.port}" proxyuser="${proxy.user}" proxypassword="${proxy.pass}"/>  
	</target>  
	
	<target name="resolve" depends="proxy">  
    	<ivy:retrieve/>  
	</target> 
	
## git代理
设置环境变量*http_proxy*和*https_proxy*，windows请直接添加，unix*导入环境变量如下，如不需要用户名密码直接去掉。

	export http_proxy="http://username:password@ip:port/"
	export https_proxy="http://username:password@ip:port/"
	
## wget代理

	wget -e http-proxy=ip:port --proxy=on -c download-url