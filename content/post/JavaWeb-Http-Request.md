---
title: "JavaWeb Http"
date: 2019-10-27T20:54:37-07:00
draft: true
---

# HTTP协议

1、HTTP概念：HyperText transfer protocol 超文本传输协议

    * 传输协议：定义了客户端和服务器端通信时发送数据的格式
    * 格式：
        1、基于TP/IP的高级协议
        2、默认端口号：80
        3、基于请求响应模型的：一次请求对应一次响应
        4、无状态的：每次请求之间相互独立，不能交互数据
    * 历史版本：
        1、1.0：每一次请求响应都会建立新的连接
        2、1.1：复用连接

2、请求消息数据格式

    * 请求行
        请求方式 请求url 请求协议/版本
        GET /login.html HTTP/1.1  
        请求方式：
            HTTP协议中有7种请求方式，常用的有两种
                * GET
                    1、请求参数在请求行中，在url后
                    2、请求的url长度是有限制的
                    3、不太安全
                * POST
                    1、请求参数在请求体中
                    2、请求的url长度是没有限制的
                    3、相对安全
    * 请求头：客户端浏览器告诉服务器一些信息
        请求头名称: 请求头值
        常见的请求头：
        1、HOST
        2、User-Agent：浏览器告诉服务器，我访问你使用的浏览器版本信息
            * 可以在服务器端获取该头的信息，解决浏览器的兼容问题
        3、Referer：http://localhost/login.html
            告诉服务器，我(当前请求)从哪里来？
            作用：
                防盗链
                统计工作
        4、Connection：keep-alive   可复用
    * 请求空行
        空行，用于分割请求头和请求体的
    * 请求体(正文)
        封装POST请求消息的请求参数

3、Request

    * 浏览器和服务器交互原理
        1、tomcat服务器会根据请求url中的资源路径，创建对应的ServletDemo1的对象
        2、tomcat服务器会创建request和response对象，request对象中封装请求消息数据
        3、tomcat将request和response两个对象传递给service方法，并且调用service方法
        4、程序员(我们)可以通过request对象来获取请求消息数据，可以通过response对象来设置响应消息数据
        5、服务器在给浏览器做出响应之前，会从response对象中拿程序员设置的响应消息数据

    * request对象和response对象的原理
        1、request和response对象是由服务器创建的。我们来使用它们
        2、request对象是来获取请求消息，response对象是设置响应消息
    
    * request对象继承体系结构
        ServletRequest -- 接口
            | 继承
        HTTPServletRequest  -- 接口
            | 实现
        org.apache.catalina.connector.RequestFacade 类(tomcat)

    * request功能：
        1、获取请求消息数据
            * 获取请求行数据
                1、请求格式：请求方式 请求url 请求协议/版本
                        GET /day14/demo1?name=zhangsan HTTP/1.1  
                2、方法：
                    * 获取请求方式：String getMethod()-->GET
                    * 请求url：
                        1、获取虚拟目录：String getContextPath()-->/day14
                        2、获取Servlet路径：String getServletPath()-->/demo1
                        3、获取GET方式的请求参数：String getQueryString()-->name=zhangsan
                        4、获取请求的URI：
                            String getRequestURI()-->/day14/demo1  
                            StringBuffer getRequestURL()-->http://localhost/day14/demo1

                            URL：统一资源定位符：http://localhost/day14/demo1
                            URI：统一资源标识符：/day14/demo1
                    * 获取协议及版本号：String getProtocol()-->HTTP/1.1
                    * 获取客户机的IP地址：String getRemoteAddr()
            * 获取请求头数据
                方法：
                    String getHeader(String name)：通过请求头的名称来获取请求头的值
                    Enumeration<String> getHeaderNames()：获取所有的请求头名称
            * 获取请求体数据
                只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数
                步骤：
                    * 获取流对象
                        BufferedReader getReader()：获取字符输入流，只能操作字符数据
                        ServletInputStream getInputStream()：获取字节输入流，可以操作所有类型数据
                            在文件上传知识点后讲解
                    * 再从流对象中拿数据
        2、其他功能：
            * 获取请求参数(通用方式):不论是post还是get请求方式都能用这些方法进行获取参数
                1、String getParameter(String name)：根据参数名称来获取参数值  username=zs&password=123
                2、String getParameterValues(String name):根据参数名称获取参数值得数组 hobby=xx&hobby=game
                3、Enumeration<String> getParameterNames():获取所有请求的参数名称
                4、Map<String, String[]> getParameterMap():获取所有请求参数的键值对集合
                中文乱码问题：
                    get方式：tomcat 8y以上已经将get方式乱码问题解决了
                    post方式：会乱码
                        在获取参数前，设置request的编码方式：request.setCharacterEncoding("utf-8");
            * 请求转发：一种在服务器内部的资源跳转方式
                步骤：
                    1、通过request对象获取请求转发器对象：RequestDispatcher getRequestDispatcher(String path)
                    2、通过RequestDispatcher对象进行转发：forward(ServletRequest request, ServletResponse response)
                特点：
                    1、浏览器地址栏路径不发生变化
                    2、转发只能转发到当前服务器内部资源中
                    3、转发是一次请求
            * 共享数据
                域对象：一个有作用范围的对象，可以在范围内共享数据
                request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
                方法：
                    void setAttribute(String name, Object obj): 存储数据
                    Object getAttribute(String name): 通过键获取值
                    void removeAttribute(String name): 通过键移除键值对
            * 获取ServletContext
                ServletContext getServletContext()

4、案例：用户登录

    * 用户登录案例需求
        1、编写login.html登录页面
            username & password两个输入框
        2、使用Druid数据库连接池技术，操作mysql，day14数据库中user表
        3、使用JdbcTemplate技术封装JDBC
        4、登录成功跳转到SuccessServlet展示：登录成功！用户名，欢迎您
        5、登录失败跳转到FailServlet展示：登录失败，用户名或密码错误
    * 分析
    * 开发步骤
        1、创建项目，导入html页面，配置文件，导入jar包
        2、创建数据库环境
            CREATE DATABASE day14;
            USE day14;
            CREATE TABLE USER(
                id INT PRIMARY KEY AUTO_INCREMENT,
                username VARCHAR(32) UNIQUE NOT NULL,
                PASSWORD VARCHAR(32) NOT NULL
            );

        3、创建包User，创建User类
            package Case;

            /**
            * 用户的JavaBean 实体类
            */
            public class User {
                
                private int id;
                private String username;
                private String password;

                public int getId() {
                    return id;
                }

                public void setId(int id) {
                    this.id = id;
                }

                public String getUsername() {
                    return username;
                }

                public void setUsername(String username) {
                    this.username = username;
                }

                public String getPassword() {
                    return password;
                }

                public void setPassword(String password) {
                    this.password = password;
                }

                @Override
                public String toString() {
                    return "User{" +
                            "id=" + id +
                            ", username='" + username + '\'' +
                            ", password='" + password + '\'' +
                            '}';
                }
            }

        4、创建包util，创建JDBCUtils静态类
            package util;

            import com.alibaba.druid.pool.DruidDataSourceFactory;

            import javax.sql.DataSource;
            import java.io.IOException;
            import java.io.InputStream;
            import java.sql.Connection;
            import java.sql.SQLException;
            import java.util.Properties;

            /**
            * JDBC工具类，使用Druid连接池
            */
            public class JDBCUtils {

                private static DataSource ds;

                static{
                    try {
                        //1.加载配置文件properties
                        Properties pro = new Properties();
                        //使用ClassLoader加载配置文件，获取字节输入流
                        InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
                        assert is != null;
                        pro.load(is);
                        //2.初始化连接池对象
                        ds = DruidDataSourceFactory.createDataSource(pro);
                    } catch (IOException e) {
                        e.printStackTrace();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }

                /**
                * 获取连接池对象
                * @return
                */
                public static DataSource getDataSource(){
                    return ds;
                }

                /**
                * 获取连接Connection对象
                * @return
                */
                public static Connection getConnection() throws SQLException {
                    return ds.getConnection();
                }
            }

        5、创建包dao，创建UserDao类，提供login方法
            package dao;

            import User.User;
            import org.springframework.dao.DataAccessException;
            import org.springframework.jdbc.core.BeanPropertyRowMapper;
            import org.springframework.jdbc.core.JdbcTemplate;
            import util.JDBCUtils;

            /**
            * 操作数据库中User表的类
            */
            public class UserDao {
                //声明JDBCTemplate对象共用
                private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
                /**
                * 登录方法
                * @param loginUser 只有用户名和密码
                * @return 包含用户全部数据，没有查询到，返回null
                */
                public User login(User loginUser){
                    try {
                        //1.编写sql
                        String sql = "select * from user where username = ? and password = ?";
                        //2.调用query方法
                        User user = template.queryForObject(sql, new BeanPropertyRowMapper<User>(User.class), loginUser.getUsername(), loginUser.getPassword());
                        return user;
                    }catch(DataAccessException e){
                        e.printStackTrace();//记录日志
                        return null;
                    }
                }
            }
   
        6、创建servlet包，编写LoginServlet类
            * LoginServlet类
                package servlet;
                import User.User;
                import dao.UserDao;

                import javax.servlet.ServletException;
                import javax.servlet.annotation.WebServlet;
                import javax.servlet.http.HttpServlet;
                import javax.servlet.http.HttpServletRequest;
                import javax.servlet.http.HttpServletResponse;
                import java.io.IOException;
                import java.io.UnsupportedEncodingException;

                @WebServlet("/loginServlet")
                public class LoginServlet extends HttpServlet {

                    @Override
                    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException {
                        //1.设置编码
                        req.setCharacterEncoding("utf-8");

                        <!-- //2.获取请求参数
                        String username = req.getParameter("username");
                        String password = req.getParameter("password");
                        //3.封装user对象
                        User loginUser = new User();
                        loginUser.setUsername(username);
                        loginUser.setPassword(password); -->

                        //2.获取所有请求参数
                        Map<String, String[]> map = req.getParameterMap();
                        //3.创建User对象
                        User loginUser = new User();
                        //3.2使用BeanUtils封装
                        try {
                            BeanUtils.populate(loginUser, map);
                        } catch (IllegalAccessException e) {
                            e.printStackTrace();
                        } catch (InvocationTargetException e) {
                            e.printStackTrace();
                        }

                        //4.调用UserDao的login方法
                        UserDao dao = new UserDao();
                        User user = dao.login(loginUser);

                        //5.判断user
                        if (user == null){
                            //登录失败
                            req.getRequestDispatcher("/failServlet").forward(req, resp);
                        } else{
                            //登录成功
                            //存储数据
                            req.setAttribute("user", user);
                            //转发
                            req.getRequestDispatcher("/successServlet").forward(req, resp);
                        }
                    }

                    @Override
                    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException {
                        this.doGet(req, resp);
                    }
                }

            * SuccessServlet类
                package servlet;
                import User.User;

                import javax.servlet.ServletException;
                import javax.servlet.annotation.WebServlet;
                import javax.servlet.http.HttpServlet;
                import javax.servlet.http.HttpServletRequest;
                import javax.servlet.http.HttpServletResponse;
                import java.io.IOException;

                @WebServlet("/successServlet")
                public class SuccessServlet extends HttpServlet {
                    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                        //获取request域中共享的user对象
                        User user = (User) request.getAttribute("user");
                        if (user != null){
                            //给页面写一句话
                            //设置编码
                            response.setCharacterEncoding("utf-8");

                            //输出
                            response.getWriter().write("登录成功!" + user.getUsername() + "用户名,欢迎您!");
                        }
                    }

                    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                        this.doPost(request, response);
                    }
                }

            * FailServlet类
                package servlet;
                import javax.servlet.ServletException;
                import javax.servlet.annotation.WebServlet;
                import javax.servlet.http.HttpServlet;
                import javax.servlet.http.HttpServletRequest;
                import javax.servlet.http.HttpServletResponse;
                import java.io.IOException;

                @WebServlet("/failServlet")
                public class FailServlet extends HttpServlet {
                    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                        //给页面写一句话
                        //设置编码
                        response.setCharacterEncoding("utf-8");

                        //输出
                        response.getWriter().write("登录失败，用户名或密码错误");
                    }

                    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                        this.doPost(request, response);
                    }
                }

        7、login.html中form表单的action路径的写法
            虚拟路径 + Servlet的资源路径
            <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="UTF-8">
                <title>Title</title>
            </head>
            <body>
                <form action = "/登录案例/loginServlet" method = "post">
                    用户名:<input type = "text" name = "username"><br/>
                    密码:<input type = "password" name = "password"><br/>
                    <input type = "submit" value = "登录">
                </form>

            </body>
            </html>
        
        8、BeanUtils工具类，简化数据封装
            * 用于封装JavaBean的
            1、JavaBean：标准的Java类
                * 要求
                    1、类必须被public修饰
                    2、必须提供空参的构造器
                    3、成员变量必须使用private修饰
                    4、提供公共setter和getter方法
                * 功能：封装数据
            2、概念：
                成员变量
                属性：setter和getter方法截取后的产物
            3、方法：
                setProperty():
                getProperty():
                populate(Object obj, Map map):将map集合的键值对信息，封装到对应的JavaBean对象中

        连接数据库报错解决方案：https://blog.csdn.net/qq_37874220/article/details/84848850
