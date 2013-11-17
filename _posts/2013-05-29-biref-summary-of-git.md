---
layout: post
title: Git使用小结
categories: [toolsoft]
tags: [git]
---

# Git配置

## 配置文件路径
- `/etc/gitconfig`：系统中对所有用户有效的配置。使用`git config --system`修改。
- `~/.gitconfig`：只对某一个用户有效。使用`git config --global`配置。
- 当前项目的`.gitconfig`：只对当前项目有效。在当前项目中使用git config配置。

## 常用配置选项
	$ git config --global user.name "Jomin Yang"
	$ git config --global user.email jomin.me@gmail.com

	$ git config --global core.editor vim 		//编辑器
	$ git config --global merge.tool vimdiff    //差异化分析工具

	$ git config --list
	$ git help <verb>

## 常用命令

	$ git init

	$ git add .
	$ git add *.c
	$ git add README

	$ git commit -m "first version" 
	$ git commit -a -m "second version"

	$ git clone git://github.com/schacon/grit.git
	$ git clone git://github.com/schacon/grit.git mygrit

	$ git status  查看文件状态
	$ git diff  查看工作目录文件和暂存区域快照之间的差异
	$ git imgs --staged  查看已经暂存的文件和上次提交时的快照之间的差异

	$ git rm
	$ git rm -f  如果某文件删除之前修改过或已暂存，使用-f删除
	$ git rm --cached README  从Git仓库中删除，保留在当前项目中
	$ git mv file_from file_to

	$ git log  查看提交历史
	$ git checkout -- filename  取消对文件的修改

	# 远程仓库
	$ git remote [-v]  查看当前配置的远程仓库
	$ git remote add remote-name remote_url  添加远程仓库
	$ git push remote-name branch_name  将改动提交到远端仓库
	$ git fetch remote-name  丢弃所有本地更改与提交，重新获取远端仓库最新版本
	$ git pull remote-name  更新本地仓库到最新远端仓库版本

	# 分支
	$ git checkout -b branch_name  新建一个分支
	$ git branch -d branch_name  删除一个分支
	$ git checkout master  切换回主分支
	$ git merge branch_name  合并分支到当前分支
	$ git diff branch_1 branch_2  对比两分支的内容

