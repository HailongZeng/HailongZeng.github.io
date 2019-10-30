---
title: "JavaWeb Servlet"
date: 2019-10-25T19:05:24-07:00
draft: true
---

# Servlet: server applet

1、概念：运行在服务端的小程序

    * Servlet就是一个接口，定义了Java类被浏览器访问到(tomcat识别)的规则
    * 将来我们自定义一个类，实现Servlet接口，复写方法

2、快速入门：

    * 创建JavaEE项目
    * 定义一个类，实现Servlet接口
        public class ServletDemo1 implements Servlet
    * 实现接口中的抽象方法
    * 配置Servlet
    在web.xml中配置
        <!--配置Servlet-->
        <servlet>
            <servlet-name>demo1</servlet-name>
            <servlet-class>servlet.ServletDemo1</servlet-class>
        </servlet>

        <servlet-mapping>
            <servlet-name>demo1</servlet-name>
            <url-pattern>/demo1</url-pattern>
        </servlet-mapping>

3、执行原理

    * 当服务器接收到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
    * 查找web.xml文件，是否有对应的<url-pattern>标签体内容
    * 如果有，则再找到对应的<servlet-class>全类名
    * tomcat会将字节码文件加载进内存，并且创建器对象
    * 调用其方法

4、Servlet中的生命周期

    被创建：执行init方法，只执行一次。
        * Servlet什么时候被创建？   
            1、第一次被访问时创建，默认情况下
                * <load-on-startup>的值时负数
            2、在服务器启动时，创建
                * <load-on-startup>的值为0或正整数
        * Servlet的init方法只执行一次，说明一个Servlet在内存中只存在一个对象，Servlet是单例的
            1、多个用户同时访问时，可能存在线程安全问题
            2、解决：尽量不要在Servlet中定义成员变量。即使定义了成员变量，也不要修改值
    提供服务：执行service方法，执行多次。
        * 每次访问Servlet时，Service方法都会被调用一次。
    被销毁：执行destory方法，执行一次。
        * Servlet被销毁时执行。服务器关闭时，Servlet被销毁。
        * 只有服务器正常关闭时，才会执行destory方法。
    * init方法：初始化方法。在Servlet被创建时执行，只执行一次。
    * getServletConfig方法：获取ServletConfig对象。Servlet的配置对象。
    * service方法：提供服务方法。每一次Servlet被访问时执行，执行多次。
    * getServletInfo方法：获取Servlet的一些信息。版本，作者。
    * destory方法：销毁方法。在Servlet被杀死时执行(在服务器正常关闭时)，执行一次

5、Servlet3.0

    * 好处：
        支持注解配置。可以不需要web.xml了
    * 步骤：
        1、创建JavaEE项目，选择Servlet的版本3.0以上，可以不创建web.xml
        2、定义一个类，实现Servlet接口
        3、复写方法
        4、在类上使用@WebServlet注解进行配置
            * WebServlet("资源路径")

6、Servlet的体系结构

    * Servlet--接口
    * GenericServlet--抽象类：
        将Servlet接口中其他的方法做了默认空实现，只将service()方法作为抽象
        将来定义Servlet类时，可以继承GenericServlet抽象类，实现service()方法即可
    * HttpServlet--抽象类：对http协议的一种封装，简化操作
        定义类继承HttpServlet抽象类
        复写doGet()和doPost()方法

7、Servlet的相关配置

    * urlpattern：Servlet访问路径
        一个Servlet可以定义多个访问路径：@WebServlet({"/d4", "/dd4", "/ddd4"})
    * 路径定义规则：
        1、/xxx
        2、/xxx/xxx  目录结构
        3、*.do

# IDEA与Tomcat的相关配置

1、IDEA会为每一个tomcat单独建立一份配置文件
    
    查看控制台的log: 

2、工作空间项目和tomcat部署的web项目

    * tomcat真正访问的是“tomcat部署的web项目”，“tomcab部署的web项目”对应着“工作空间项目”的web目录下的所有资源
    * WEB-INF目录下的资源不能被浏览器直接访问

3、断点调试：使用"小虫子"启动  debug启动