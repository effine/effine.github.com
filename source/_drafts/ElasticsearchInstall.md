title: Elasticsearch 安装
date: 2015-11-18 14:43:14
categories: tools
tags:
  - tools
  - elasticsearch
---

下载elasticsearch

	https://download.elasticsearch.org/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/2.0.0/elasticsearch-2.0.0.tar.gz

解压

es默认使用9200，需在iptables中开启

进入es/bin目录启动

	$ ./elasticsearch

出现错误

	Exception in thread "main" java.lang.RuntimeException: don't run elasticsearch as root.
        at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:92)
        at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:138)
        at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:270)
        at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:35)

 	分析: 使用root用户运行

切换用户重新运行

	log4j:ERROR setFile(null,true) call failed.
	java.io.FileNotFoundException: /home/ylvdousweb/tools/elasticsearch-2.0.0/logs/elasticsearch.log (Permission denied)
        at java.io.FileOutputStream.open(Native Method)
        at java.io.FileOutputStream.<init>(FileOutputStream.java:221)
        at java.io.FileOutputStream.<init>(FileOutputStream.java:142)
        at org.apache.log4j.FileAppender.setFile(FileAppender.java:294)
        at org.apache.log4j.FileAppender.activateOptions(FileAppender.java:165)
        at org.apache.log4j.DailyRollingFileAppender.activateOptions(DailyRollingFileAppender.java:223)
        at org.apache.log4j.config.PropertySetter.activate(PropertySetter.java:307)
        at org.apache.log4j.config.PropertySetter.setProperties(PropertySetter.java:172)
        at org.apache.log4j.config.PropertySetter.setProperties(PropertySetter.java:104)
        at org.apache.log4j.PropertyConfigurator.parseAppender(PropertyConfigurator.java:842)
        at org.apache.log4j.PropertyConfigurator.parseCategory(PropertyConfigurator.java:768)
        at org.apache.log4j.PropertyConfigurator.configureRootCategory(PropertyConfigurator.java:648)
        at org.apache.log4j.PropertyConfigurator.doConfigure(PropertyConfigurator.java:514)
        at org.apache.log4j.PropertyConfigurator.configure(PropertyConfigurator.java:440)
        at org.elasticsearch.common.logging.log4j.LogConfigurator.configure(LogConfigurator.java:128)
        at org.elasticsearch.bootstrap.Bootstrap.setupLogging(Bootstrap.java:189)
        at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:243)
        at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:35)

    分析： 我们使用的是root解压的目录，所以出现权限不足
    解决：

    	$ chown -R <common-user> elasticsearch-{es-version} 	# 修改文件夹所属用户


    重新启动即可成功启动，测试是否允许成功：

    	$ curl -X GET http://localhost:9200/

    	# 打印信息
	    {
		  "status" : 200,
		  "name" : "Fury",
		  "cluster_name" : "elasticsearch",
		  "version" : {
		    "number" : "1.4.2",
		    "build_hash" : "927caff6f05403e936c20bf4529f144f0c89fd8c",
		    "build_timestamp" : "2014-12-16T14:11:12Z",
		    "build_snapshot" : false,
		    "lucene_version" : "4.10.2"
		  },
		  "tagline" : "You Know, for Search"
		}



Es配置集群(服务器需要在同一个网段,cluster.name相同即可自动识别组成集群)：

	在另一台服务器上安装es,然后修改 es/config/elasticsearch.yml, 取消cluster.name的注释，修改其值为相同即可

		$ cluster.name: effine-elastcsearch

	同时取消node.name的注释，修改其值为集群中不存在的任意值

		$ node.name: node-2



插件安装：

elasticsearch-servicewrapper

	在service目录下有个elasticsearch.conf配置文件,常用配置参数如下：

		#es的home路径，不用用默认值就可以(为加入环境变量必须配置，加入环境变量为尝试)
		set.default.ES_HOME=<Path to ElasticSearch Home> 

		#分配给es的最小内存 
		set.default.ES_MIN_MEM=256

		#分配给es的最大内存 
		set.default.ES_MAX_MEM=1024


		# 启动等待超时时间（以秒为单位） 
		wrapper.startup.timeout=300

		# 关闭等待超时时间（以秒为单位）

		wrapper.shutdown.timeout=300

		# ping超时时间(以秒为单位)

		wrapper.ping.timeout=300

	重新启动elasticsearch的时候测试/es/bin/elasticsearch还是/es/bin/service/elasticsearch

	1下载插件elasticsearch-servicewrapper.tar.gz（download:http://wrapper.tanukisoftware.com/doc/english/download.jsp）

	2 解压：tar-zxvf elasticsearch-servicewrapper.tar.gz。

	3 把解压得来的service目录拷贝到Es的bin目录下

	  Cdelasticsearch-servicewrapper

	  Mv service /usr/local/elasticsearch-0.18.7/bin

	 

	4启动es

	  Cd/usr/local/elasticsearch-0.18.7/bin/service

	  ./elasticsearch start    ------后台运行

	5 停止es

	   Cd/usr/local/elasticsearch-0.18.7/bin/service

	   /elasticsearch stop

	（说明：可参考https://github.com/elasticsearch/elasticsearch-servicewrapper）

	附：插件的主要命令

	   ./elasticsearchconsole  ------前台运行

	    ./elasticsearch start    ------后台运行

	    ./elasticsearchinstall   -------添加到系统自动启动

	    ./elasticsearchremove   -----取消随系统自动启动






（二）配置IK分词(服务器必须能连通网络)

1   cd  /usr/local/elasticsearch-0.18.7/bin

          ./plugin -installmedcl/elasticsearch-analysis-ik/1.0.0

    2  cd config

wgethttp://github.com/downloads/medcl/elasticsearch-analysis-ik/ik.zip--no-check-certificate

unzip ik.zip

rm ik.zip

（说明，个人觉得以上两部完成后就已经可以了，ik分词已经起作用，但网上的资料还有些配置，可参考：https://github.com/medcl/elasticsearch-analysis-ik）




使用Marve监控es: http://www.linuxidc.com/Linux/2015-02/113615.htm




	http://my.oschina.net/xiaohui249/blog/228748


清理ES存储的数据：

	# 清理掉了所有 3月份的索引文件，我发现curl 删除比rm删除要快出很多
	$ curl -XDELETE 'http://172.16.1.16:9200/logstash-2013.03.*' 