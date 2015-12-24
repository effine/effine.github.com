title: Maven 命令详解
tags:
  - maven
  - command
categories: tools
date: 2015-07-15 10:43:38
---
<img src="http://7xlmfk.com1.z0.glb.clouddn.com/imgs/article/maven-standard-dir.png" alt="Maven标准目录" />

用了maven很多年，针对常用的命令做个总结，方便查阅
    
    $ mvn archetype:create	# 创建 maven 项目

    # maven生成web项目
    $ mvn archetype:create -DgroupId='package-name' 
      -DartifactId='projectname' -DarchetypeArtifactId=maven-archetype-webapp

    # maven生成java项目
    $ mvn archetype:create  -DgroupId=package-name  -DartifactId=project-name  

    $ mvn compile     # 编译源码
    $ mvn test-compile    # 编译测试源码
    $ mvn test        # 运行应用的单元测试
    $ mvn clean   # 清理项目编译文件

<!-- more -->

    $ mvn package     # 打包（mvn -Dtest package  只打包不测试）
    $ mvn clean package  # 组合命令，清理编译文件并打包
    $ mvn jar:jar     # 打包成jar文件
    $ mvn install     # 安装项目打包文件至本地仓库Repository
    $ mvn site    # 生成项目相关网站信息(/target/site)
    $ mvn validate    # 验证工程是否正确，所需要的资源是否可用
    $ mvn deploy      # 发布项目

    # 启动服务前需执行mvn package及配置相应插件
    $ mvn jetty:run   # 启动jetty服务
    $ mvn tomcat:run  # 启动tomcat服务

    $ mvn eclipse:eclipse     # 生成eclipse项目文件,将项目转换成eclipse项目
    $ mvn eclipse:clean       # 清理eclipse项目文件
    $ mvn idea:idea       # 生成idea项目文件,将项目转换成idea项目
    $ mvn idea:clean      # 清理idea项目文件

技巧：maven跳过编译(检验)测试代码进行打包(可以衍生至其他操作，如mvn package等)；可以直接体现在命令,也可在项目文件pom.xml中配置插件

    $ mvn install -DskipTests
    或者
    $ mvn install -Dmaven.test.skip=true

    # 插件配置
    <plugin>  
        <groupId>org.apache.maven.plugins</groupId>  
        <artifactId>maven-surefire-plugin</artifactId>  
        <version>2.18.1</version>  
        <configuration>  
          <skipTests>true</skipTests>  
        </configuration>  
    </plugin>  




