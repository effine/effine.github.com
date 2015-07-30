title: SSH安装
tags:
  - ssh
  - command
categories: web
date: 2015-06-19 14:17:34
---

检查SSH是否安装(本文采用centos测试)

	$ rpm -qa | grep ssh 	#只能检查是否通过rpm的软件

如未安装，则安装即可：

	$ yum -y install openssh-server openssh-clients

重启ssh：
	
	$ service sshd restart

<!-- more -->

查看ssh默认端口22是否启动：

	$ netstat -antp | grep sshd 
	或者
	$ iptables -nL 

如端口未打开，那开启防火墙

	$ vi /etc/sysconfig/iptables
	插入： -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT

检查SSH是否为开机启动：
	
	$ chkconfig --list sshd

设置SSH为开机启动：

	$ chkconfig sshd [on|off]    #开机启动[是|否]

SSH的配置文件：/etc/ssh/sshd_config

	# 限制ip连接
	sshd: All 	#允许所有ip ssh连接该服务器
	sshd: 192.168.0.125		#只允许125 ssh连接，可以配置多台

远程ssh连接,默认为22端口可以省略该参数
	
	$ ssh -p 22 root@192.168.0.125	#ssh -p port username@ip