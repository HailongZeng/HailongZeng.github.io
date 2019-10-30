---
title: "JavaWeb Tomcat"
date: 2019-10-27T15:17:06-07:00
draft: true
---

# web相关概念回顾

1、软件架构

    * C/S：客户端/服务器端
    * B/S：浏览器/服务器端

2、资源分类

    * 静态资源：所有用户访问后，得到的结果都是一样的，称为静态资源。静态资源可以直接被浏览器解析
        如：html，css，JavaScript
    * 动态资源：每个用户访问相同资源后，得到的结果可能不一样，称为动态资源。动态资源被访问后，需要先转换为静态资源，再返回给浏览器，直接被浏览器解析
        如：servlet/jsp，php，asp...

3、网络通信三要素

    * IP：电子设备(计算机)在网络中的唯一标识。
    * 端口：应用程序在计算机中的唯一标识。0~65536
    * 传输协议：规定了数据传输的规则
        基础协议：
            tcp：安全协议，三次握手。速度稍慢
            udp：不安全协议。速度快

# web服务器软件

1、服务器：安装了服务器软件的计算机

2、服务器软件：接收用户的请求，处理请求，做出响应

3、web服务器软件：接收用户的请求，处理请求，做出响应

    在web服务器软件中，可以部署web项目，让用户通过浏览器来访问这些项目
    web容器

4、常见的java相关的web服务器软件：

    * weblogic：oracle公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的
    * webSphere：IBM公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的
    * JBoss：JBoss公司的，大型的JavaEE服务器，支持所有的JavaEE规范，收费的
    * Tomcat：Apache基金组织，中小型的JavaEE服务器，仅仅支持少量的JavaEE规范 servlet/jsp，开源的，免费的。

    JavaEE：Java语音在企业级开发中使用的技术规范的总和，一共规定了13项大的规范

5、Tomcat：web服务端软件

    * 下载：http://tomcat.apache.org/
    * 安装：解压压缩包即可
        主要：安装目录建议不要有中文路径
    * 卸载：删除目录就行了
    * 启动：
        1、bin目录下的启动文件startup.bat或startup.sh
        2、访问：浏览器输入：http://localhost:8080 回车访问  http://127.0.0.1:8080
                        http://别人的ip:8080  访问别人
        3、可能遇到的问题：
            * 黑窗口一闪而过
                原因：没有正确配置JAVA_HOME环境变量
                解决方案：正确去配置JAVA_HOME环境变量
            * 启动报错
                暴力解决：找到占用的端口号，并且找到对应的进程，杀死该进程
                    netstate -ano
                    启动任务管理器，查看进程，选择pid，结束对应pid进程
                温柔解决：修改自身的端口号  
                    conf目录下server.xml文件修改<Connector port = "8080" protocol = "HTTP/1.1" connectionTimeout = "20000" redirectPort = "8443" />的port
                    一般会将tomcat的默认端口号修改为80。80端口号是http协议的默认端口
                    好处：在访问时，就不用输入端口号

    * 关闭：
        1、正常关闭：
            bin目录下shutdown.bat或者shutdown.sh
            ctrl + c关闭
        2、强制关闭：点击启动窗口的x
    * 配置
        1、部署项目的方式：
            * 直接将项目放到webapps目录下即可
                项目的访问路径-->虚拟目录，例如：localhost/hello/hello.html在网页中访问
                简化部署：将项目打成war包，再讲war包放置到webapps目录下。war包会自动解压缩
            * 在conf目录下修改server.xml文件，在<host>标签体下添加<Context docBase = “项目存放的路径” path = "虚拟目录" />    这种方式不安全
            * conf目录下Catalina目录中localhost目录创建新的任意名称的xml(如bbb.xml)，在文件中编写<Context docBase = “项目存放的路径” />，现在的虚拟目录就是xml的名称，访问localhost/bbb/hello.html
        
        2、静态项目和动态项目
            静态项目只能放静态资源
            * 目录结构
                java动态项目的目录结构：
                    项目的根目录
                        --WEB-INF目录：
                            --web.xml：web项目的核心配置文件
                            --classes目录：放置字节码文件的目录
                            --lib目录：放置web的jar包
        
        3、将Tomcat集成到IDEA中，并且创建JavaEE项目