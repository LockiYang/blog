---
layout: post
title: sqoop源码学习(一)：eclipse调试环境搭建
categories: [sqoop]
tags: [hadoop, sqoop, eclipse]
---

公司使用sqoop进行hadoop平台的数据导入导出工具，作为传统数据库与hadoop之间的桥梁，sqoop本身还不成熟，在使用过程中发现其对某些特殊字段的处理和性能方面都存在问题，需要进行改进。最近打算对sqoop源码进行学习，顺便学习高级mapreduce应该怎么写。
学习源码，避免不了要对源码进行调试，首先介绍如何搭建sqoop的eclipse调试环境。

1、 把sqoop导入为eclipse工程
	
下载sqoop的源码解压，在命令行下进入到sqoop源码目录，执行`ant eclipse`就可以生成eclipse的项目文件。	这一步主要是下载依赖的jar包和相关环境变量的设置。
	
2、 在hadoop环境中安装sqoop

sqoop作为hadoop的客户端，要在hadoop环境中才能执行，所以我们需要在已经搭建好的hadoop环境中安装sqoop，推荐在虚拟机中搭建hadoop。具体安装方法参见官方文档。
	
3、 sqoop远程调试设置

在sqoop安装目录下打开bin/sqoop，最后一行如下

	exec ${HADOOP_HOME}/bin/hadoop org.apache.sqoop.Sqoop "$@"
	
说明是执行hadoop命令，传入相应的类和参数，要进行远程调试我们需要修改这一句为：
	
	exec ${HADOOP_HOME}/bin/hadoop -Xdebug -Xrunjdwp:transport=dt_socket,address=8991,server=y,suspend=y org.apache.sqoop.Sqoop "$@"

然后执行sqoop的测试语句

	$ sqoop list-tables --connect jdbc:mysql://localhost/hadoop --username hadoop --password hadoop

我们发现sqoop会卡住等待远程eclipse的连接。
	
4、 eclipse连接设置

右键单击eclipse中的sqoop工程，打开*debug as*->*debug configurations*->*remote java application*，*Connection Type*选择*Socket Attach*，*Host*和*Port*	填入相应的hadoop所在机器的ip和sqoop调试运行的端口，然后单击*debug*阻塞的sqoop测试语句会继续执行了。

到此我们就可以设置断点进行sqoop程序的调试了。
