title: Git 分支
tags:
  - git
  - branch
categories: git
date: 2015-10-23 22:35:35
---

	$ git branch --help		# 查看关于git branch帮助

[查看分支]

	$ git branch 	# 查看本地分支（星号*标注为当前分支）
	$ git branch -r 	# 查看所有远程[remote]分支
	$ git branch -a 	# 查看所有[all]本地及远程分支
	$ git branch -v 	# 查看各个分支最后的提交

[创建\切换\重命名分支]

	$ git branch <name>		# 创建name分支(继承当前分支来创建)
	$ git checkout <name> 	# 切换分支到name(分支name需已存在)
	$ git checkout -b <name>		# 创建分支name并切换到该分支(前提name分支不存在)
	$ git checkout -b <name> <master>	# 创建分支name并切换到该分支，继承master分支创建而来
	$ git branch -m <name> <test>		# 重命名name分支为test

[删除分支]

	# 删除本地分支[name]
	$ git branch [-d | -D] <name>
		-d 		# 删除[delete]本地name分支(存在修改或未合并删除失败,多个分支用空格分隔)
		-D 		# 忽略修改强制删除本地name分支（多个分支用空格分隔）

	# 删除远程分支[name]
	$ git branch -d -r origin/<name> 	# 删除远程分支name
	$ git push origin :<name> 	# 冒号前面的空格不能少,即把空分支push到远程达到删除效果[since Git v1.5.0]
	$ git push origin --delete <name> 	# [since Git v1.7.0]

[合并分支]

	$ git branch --merged 	# 查看已合并的分支列表
	$ git branch --no-merged	# 查看未合并的分支列表
	$ git merge <name> 	# 合并name分支到当前分支,发生冲突修复后再次提交

[关联分支]

	# 设置本地分支跟踪远程分支(与远程分支关联)，分支设置跟踪后直接git pull、git push即可直接查找到远程分支进行操作
	$ git branch --set-upstream-to=origin/<branch> <branch>  (推荐)
	$ git branch --set-upstream <branch> <origin/branch>	
	
本地跟踪远程分支，会在当前项目的config文件产生如下配置

	[branch "master"]
		remote = origin
		merge = refs/heads/master