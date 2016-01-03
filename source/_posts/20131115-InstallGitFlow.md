---
title: Git Flow安装
date: 2013-11-15 01:27:09
categories: tools
tags: [git,工具]
---
### windows 安装
Git Flow的诸多特性，我就不再叙述；这里着重介绍git flow在windows的安装;

1. 因为windows安装的git属于msysgit版，所以需要按照msysgit的安装步骤来安装git flow；首先需要安装[util-linux-ng for Windows](http://gnuwin32.sourceforge.net/packages/util-linux-ng.htm)，下载zip格式的二进制Binaries文件，如下图：
![util-linux-ng for Windows](/images/git/Install-Git-Flow-One.png)
![libintl](/images/git/Install-Git-Flow-Two.png)
将下载的两个文件解压，将util-linux-ng-2.14.1-bin/bin/getopt.exe和libintl-0.14.4-bin/bin/libintl3.dll文件复制到本地Git安装路径的bin目录；
2. clone GitHub的gitflow项目到你的临时工作目录（安装完成可以删除），执行命令 `git clone --recursive git://github.com/nvie/gitflow.git`；clone完成，win + R运行"cmd"进入windows控制台，进入该gitflow/contrib目录，执行命令`msysgit-install.cmd "Git Install Path"(如E:\Install\Git)`
3. 测试；进入windows控制台，执行命令`git flow`看提示信息(前提：git必须设置环境变量；如没有设置则使用Git Bash)

参考资料：
[[作者说明](https://github.com/nvie/gitflow/wiki/Windows)]、
[[资料一](http://www.360doc.com/content/13/0720/22/2569758_301389862.shtml)]