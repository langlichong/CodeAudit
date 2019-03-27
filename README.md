# 部署指南

**本文档不涉及源码检测引擎（Python部分）部署**

## 环境要求
- JDK 或 JRE 版本最低为JDK8 或者JRE8
- MySQL版本尽量使用最新稳定版（开发环境为8.0.11）
- MongoDB本地开发环境为：3.6.5，部署环境尽量保持最新稳定版即可
- Nginx : 开发环境为nginx/1.12.2
- 确保准备的各主机间通信畅通

## 后端部署步骤

1. 按<环境要求>部分检查目标平台环境，确保环境准备完整

2. 拉取源代码：https://git.coding.net/guyanjun/codeChecker-adminBackend.git 

3. 数据库初始化：在MYSQl主机上顺序执行源码包中的dra-DDL.sql与dra-DML.sql 文件，确保脚本正确执行

4. 修改Application配置文件：application.yml 修改项如下：

    A、修改spring.datasource.url,使其指向MySQL Server的真实主机地址   
      
    B、修改spring.datasource.username,配置MySql数据库使用的用户名    
    
    C、修改spring.datasource.password,配置MySql连接数据库用户的密码（此处值为加密后的，且需要使用ENC包含）   
    
        加密密码生成：运行codeChecker-adminBackend\src\test\java\com\aj\JasyptTest.java#encryptPwd，输出结果即为此处需要的密码，
        可以运行多次，每次运行结果都是有效的。或者自己手工配置（请参考官方jasypt操作）   
          
    D、修改spring.data.mongodb.uri:指向正确的 MongoDB Server .(若MongoDB开启了认证请自行参考相关文档设置)
    
    E、logging.path : 指定应用产生的日志存放位置，可以按需要修改路径即可
    
    F、com.aj.config.repo-path: 指定将来从源码托管平台拉取的源代码存放位置,请按需要修改
    
    G、com.aj.config.audit-url: 配置源码检测引擎请求地址，配置时请确保该RestAPI确实可以通信。
    
    **注： 未在上述列出的条目请勿随意改动** 
       
5. 打包部署     
    
- 打包    

    项目使用Gradle-Wrapper进行打包，不需要目标主机安装Gradle。     
    
    A、windows平台：使用项目源码根目录下gradlew.bat,命令: gradlew.bat bootRepackage     
    
    B、*nix： gradle bootRepackage        
    
    以上命令会在项目目录build/libs下生成名字为sca.jar 的可执行jar文件
    
- 部署    

    方式1： 执行命令 " java -jar sca.jar >sca.log 2>&1  & " ,日志会输出到sca.log 中  
    方式2:  nohup java -jar &  
    方式3:  java -jar sca.jar  
    方式4:  *nix服务方式：     
       
            创建一个指向init.d的符号连接：ln -s <打包好的可执行jar的全路径>  /etc/init.d/<自定义名称>，之后即可使用
            service <自定义名称> start 来启动应用
        

## 前端代码部署

   1. 安装nginx server，修改nginx配置文件，此处建议使用本目录下nginx.conf.template覆盖默认配置文件     
    
   2. 拉取前端代码：https://git.coding.net/guyanjun/codeChecker-ui.git 到目标机器      
         
   3. 修改nginx.conf 中server块中的< root     /opt/aj/codeChecker-ui;>部分为步骤2中拉取的源代码根目录       
    
   4. 启动nginx,centos7下为： systemctl start nginx   
    
## 项目访问     

   - 地址：http://nginx.server.host[:80]/login.html 
    
## 注意事项
