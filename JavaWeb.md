# JavaWeb

## 1、Tomcat

conf/server.xml

配置启动端口号

Tomcat默认：8080

MySQL：3306

http：80

https：443

```xml
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"
               maxParameterCount="1000"
               />
```

可以配置主机名称

- 默认主机名localhost ->127.0.0.1
- 默认网站应用存放位置：webapps

```xml
<Host name="localhost"  appBase="webapps"

​      unpackWARs="true" autoDeploy="true">
```

### 面试题

网站是如何进行访问的！

1、输入一个域名，回车

2、检查本机hosts文件下有没有对应域名映射

​	1、有：直接返回对应ip地址，这个地址有我们要访问的web程序，可以直接访问

​	2、没有：去DNS服务器找，找到返回ip



### 发布网站

- 自己网站放到服务器（Tomcat）中web应用文件夹（webapps）下

网站应有结构

```java
-- webapps: Tomcat服务器web目录
    --ROOT
    --网站目录名
    	--index.html：网站默认首页
    	-- WEB-INF
    		-- web.xml
    		-- class: java程序
            -- lib：web应用依赖的jar包
        -- static
          -- css
             --style.css
          -- js
          -- img
        -- ...
```



HTTP协议：面试

Maven：构建工具

- Maven安装包

Servlet

- Servlet配置
- 原理



## 2、HTTP

HTTP（超文本传输协议）是一个简单的请求-响应协议，通常运行在TCP之上

- 80

https：安全的

- 443

- http1.0
  - HTTP/1.0：客户端可以与web服务器连接后，只能获得一个web资源，断开链接

- http2.0
  - HTTP/1.1：客户端可以与web服务器连接后，可以获得多个web资源



### 2.1、HTTP请求

客户端---请求---服务器

#### **1、请求行**

- 请求行中的请求方式：GET
- 请求方式**Get，Post**，head，delete，put
  - get：请求能携带的参数比较少，大小有限制，会在浏览器URL地址栏显示数据内容，不安全，但高效
  - post：请求携带的参数没有限制，大小没有限制，不会在浏览器URL栏显示参数内容，安全，但不高效

#### 2、消息头

```sql
Accept:告诉浏览器，他所支持的数据类型
Accept-Encoding：支持哪种编码格式 GBK UTF-8 GB2312 ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是连接
Host: 主机、、、
```



### 2.2、HTTP响应

服务器---响应---客户端

#### 1、响应体

```java
Accept:告诉浏览器，他所支持的数据类型
Accept-Encoding：支持哪种编码格式 GBK UTF-8 GB2312 ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是连接
Host: 主机、、、
Refrush：告诉客户端，多久刷新一次
Location：让网页重新定位    
```

#### 2、响应状态码（重点）

200：请求响应成功 200

3**：请求重定向

- 重定向：你重新到我给你的位置去

4**：找不到资源 404

5**：服务器代码错误 500   502：网关错误



### 面试题：

当你的浏览器中地址栏输入地址并回车一瞬间到页面能够展示，经历了什么？

## 3、Maven

自动帮忙导入和配置jar包

### 3.1、Maven项目架构管理工具

核心思想：**约定大于配置**

- 有约束，不要去违反

Maven会规定好如何去编写Java代码（目录结构），必须按这个规范来

maven由于他的约定大于配置，我们之后可能会遇到我们写的配置文件，无法被导入或者导出问题，解决方案

```xml
<!-- 在build中配置resources，来防止我们资源导出失败问题-->
<build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
</build>
```



## 4、Servlet

### 4.1、什么是servlet

- sun公司开发动态web的一门技术

- sun公司在这些api中提供了一个接口较：Servlet，开发Servlet程序两个小步骤：

  - 编写一个类实现Servlet
  - 开发好的类部署到web服务器

  把实现Servlet接口的Java程序叫做，Servlet

### 4.2、HelloServlet

servlet接口sun公司有两个默认的实现类：HttpServlet



1、构建一个普通Maven项目，删掉src目录，在项目中构建module；这个空的工程就是Maven主工程

2、Maven父子工程

父项目会有

```xml
<modules>
        <module>Servlet-01</module>
</modules>
```

子项目会有

```xml
<parent>
        <artifactId>JavaWeb-02-Servlet</artifactId>
        <groupId>com.landon</groupId>
        <version>1.0-SNAPSHOT</version>
</parent>
```

父项目中的jar包子项目可以直接使用

````java
son extends father 
````

3、Maven环境优化

1. 修改web.xml为最新的
2. 将Maven结构搭建完整

4、编写Servlet程序

1. 编写一个普通类
2. 实现servlet接口，这里我们直接继承HttpServlet

```java
package com.landon.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class HelloServlet extends HttpServlet {
    //由于get post只是请求实现的不同方式，可以互相调用，业务逻辑都一样
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("进入doGet方法");
        //ServletOutputStream outputStream = resp.getOutputStream();
        PrintWriter writer = resp.getWriter(); //响应流
        writer.print("hello servlet");

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

5、编写servlet映射

 为什么要映射：我们写的是Java程序，但是需要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要在web服务中注册我们写的servlet，还需要给他一个浏览器能够访问的路径。

```xml
 <!--注册servlet-->
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.landon.servlet.HelloServlet</servlet-class>
    </servlet>
    <!--servlet请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
	</servlet-mapping>
```

6、配置Tomcat

7、启动测试



### 4.3、Servlet原理

web服务器将req，resp对象给servlet接口，处理请求，返回响应结果

### 4.4、Mapping问题

1.一映一

2.一映多

3.一映通用

```xml
	<servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello/*</url-pattern>
    </servlet-mapping>
```

4.默认请求路径

```xml
	<servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
```

5.指定前后缀

```xml
<!-- 可以自定义后缀实现请求映射
	 注意点 .*前面不能加项目映射路径
	 hello/*.landon
	 /*.landon
-->
	<servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>*.landon</url-pattern>
    </servlet-mapping>
```

6.优先级问题

指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求

```xml
<servlet>
    <servlet-name>404</servlet-name>
    <servlet-class>com.landon.servlet.ErrorServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>404</servlet-name>
    <url-pattern>/*</url-pattern>
</servlet-mapping>
```



### 4.5、ServletContext

web容器在启动的时候，他会为每个web程序都创建一个对相应ServletContext对象，它代表了当前的web应用。

#### 1.共享数据

- 共享数据

​	我在这个Servlet中保存的数据，可以在另一个Servlet中拿到

#### 2、获取初始化参数

#### 3、请求转发

#### 4、读取资源文件

Properties类

- 在java目录下新建properties
- 在resources目录下新建properties

我们发现：都被打包在了同一个路径下，classes，我们俗称这个目录为classpath。

## 5、HttpServletResponse

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，代表响应的一个HttpServletResponse；

- 如果要获取客户端请求过来的参数：找HttpServletRequest
- 如果要给客户端响应信息：找HttpServletResponse

### 常见应用

#### 1、向浏览器输出消息（前面讲的）

#### 2、下载文件

1. 要获取下载文件的路径
2. 下载的文件名是什么？
3. 设置浏览器能支持下载我们需要的东西
4. 获取文件下载的输入流
5. 创建缓冲区
6. 获取OutputStream对象
7. 将FileOutputStream流写入到buffer缓冲区
8. 使用OutputStream将缓冲区中的数据输出到客户端

#### 3、验证码功能

#### 4、**实现重定向（*）**

一个web资源收到客户端的请求后，他会通知客户端去访问另外一个web资源，这个过程叫重定向

常见场景：

- 用户登录

```java
public void sendRedirect(String location) throws IOException;
```

测试：

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        /*
        resp.setHeader("Location","/r/img");
        resp.setStatus(302);
         */
        resp.sendRedirect("/resp/img");//重定向
    }
```

##### **面试题**

**聊聊重定向和转发的区别：**

相同点：

- 页面都会实现跳转

不同点：

- 请求转发的时候，url不会产生变化
- 重定向的时候，url地址栏会到发生变化

## 6、HttpServletRequest

HttpServletRequest代表客户端的请求，用户通过通过Http协议访问服务器，Http请求中所有信息会被封装到HttpServletRequest，通过HttpServletRequest的方法，获得客户端所有信息

### 1、获取前端传递的参数

```java
req.getParameter(); //String s
req.getParameterValues(); //String[]
```



2、请求转发

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    req.setCharacterEncoding("utf-8");
    String username = req.getParameter("username");
    String password = req.getParameter("password");
    String[] hobbys = req.getParameterValues("hobbys");
    System.out.println("============================");
    //后台接受乱码问题
    System.out.println(username);
    System.out.println(password);
    System.out.println(Arrays.toString(hobbys));
    System.out.println("============================");

    //请求转发
    //这里的/ 代表当前web应用
    req.getRequestDispatcher("/success.jsp").forward(req,resp);
    resp.setCharacterEncoding("utf-8");
}

@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    doGet(req, resp);
}
```

##### **面试题**

**聊聊重定向和转发的区别：**

相同点：

- 页面都会实现跳转

不同点：

- 请求转发的时候，url不会产生变化       307
- 重定向的时候，url地址栏会到发生变化    302

## 7、Cookie、Session

### 7.1、会话

**会话：**用户打开一个浏览器，点击了很多超链接，访问了很多web资源，关闭浏览器，这个过程称为会话。

**有状态会话：**一个同学来过教室，下次再来教室，我们会知道这个同学，曾经来过，称之为有状态会话。

举例：

如何证明自己的学生身份？

1、学生证 学校给的

2、学校登记表 学校标记你来过了

一个网站，如何证明自己来过？

客户端 服务端

怎样证明服务端客户端来过

1、服务端给客户端一个信件，客户端下次访问服务端带上信件就可以了（cookie）

2、服务器登记客户端来过了，下次客户端访问时来匹配。session

### 7.2、保存会话的两种技术

Cookie

- 客户端技术（响应、请求）

Session

- 服务器技术，利用这个技术，可以保存用户的会话信息，我们可以把信息或者数据放在Session中

常见场景：网站登录之后，下次不用登陆，第二次访问就直接登上去了。

### 7.3、Cookie

1、从请求中拿到cookie信息

2、服务器相应给客户端cookie

```java
 Cookie[] cookies = req.getCookies();//获得Cookie
 cookie.getName();//获得cookie名字key
 cookie.getValue();//获得cookie的值
 Cookie cookie = new Cookie("lastLoginTime",  System.currentTimeMillis() + "");//新建一个cookie
 cookie.setMaxAge(24*60*60);//设置cookie的有效期，单位是秒
 resp.addCookie(cookie);//响应给客户端一个cookie
```

**cookie：一般保存在本地的 用户目录下的appdata；**

一个网站的cookie是否存在上限

- 一个cookie’只能保存一个信息；
- 一个web站点可以给浏览器发送多个cookie，最多存放20个cookie
- cookie大小有限制4kb；
- 300个cookie浏览器上限；

删除cookie：

- 不设置有效期，关闭浏览器，自动失效；
- 设置有效期为0

```java
cookie.setMaxAge(0);
```

编码解码：

```java
URLEncoder.encode("信息","utf-8");//编码
URLDecoder.decode(cookie.getValue(),"utf-8");//解码
```

### 7.4、Session（重点）

什么是Session：

- 服务器会给每个用户（浏览器）创建一个Session对象
- 一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就存在；
- 用户登陆之后，整个网站都可以访问！--–>保存用户的信息；保存购物车信息。。。（长久保存）



Session和cookie的区别

- Cookie是把用户的数据写给用户的浏览器，浏览器保存。（可以保存多个）
- Session是把用户的数据写到用户独占的Session中，服务器端保存（保存重要的信息，减少资源占用）
- Session对象由服务器创建



使用场景：

- 保存一个登录用户的信息；
- 购物车信息；
- 在整个项目中经常会使用的数据，我们将它保存在Session中；



## JSP（老东西）





## 8、JavaBean

实体类

JavaBean有特定的写法：

- 必须要有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法

一般用来和数据库字段做映射。ORM

ORM：对象关系映射

- 表----->类
- 字段------->属性
- 行记录----->对象

id|name|age|address
--|--|--|--
1|landon|3|北京
2|奥特曼|18|上海
3|Arcobaleno|8|杭州

```java
class People {
    private int id;
    private String name;
    private int age;
    private String address;
}

class A{
    new People(1,"landon",3,"北京");
    new People(2,"奥特曼",18,"上海");
    new People(4,"Arcobaleno",8,"杭州");
}
```

## 9、MVC三层架构

什么是MVC：model  view  controller 模型、视图、控制器

### 9.1、早些年

![image-20240125103337231](D:\Studyspace\Markdown\Note\JavaWeb.assets\image-20240125103337231.png)



用户直接访问控制层 ，控制层就可以直接操作数据库:

```java
servlet --CRUD—>数据库

弊端：程序十分臃肿，不利于维护 sevlet代码中：处理请求，响应、视图跳转、处理JDBC 、处理业务代码、处理逻辑代码

架构：没有什么是加一层解决不了的
程序员调用
    |
   JDBC
    |
MySQL Oracle SQLServer    
```

### 9.2、MVC三层架构

![image-20240125104025786](D:\Studyspace\Markdown\Note\JavaWeb.assets\image-20240125104025786.png)

Model

- 业务处理：业务逻辑（Service）
- 数据持久层：CRUD（DAO）

View

- 展示数据
- 提供链接发起Servlet请求（a、form、img...）

Controller

- 接受用户请求：（req：请求参数、Session信息...）
- 交给业务层处理对应代码
- 控制视图的跳转

```java
登录--->接受用户的登录请求--->处理用户的请求（获取用户登录的参数，username、password）--->交给业务层处理登录业务（判断用户名密码是否正确；事务）--->DAO查询是否正确--->数据库
```



## 10、Filter：过滤器（重点）

shiro（安全框架）

Filter：过滤网站的数据；

- 处理中文乱码
- 登录验证...

Filter开发步骤

1、导报

2、编写过滤器

1. 导包不要错

实现Filter接口，重写对应方法即可

3、在web.xml配置Filter过滤器

## 11、监听器

实现一个监听器的接口：（有N种）

1.编写一个监听器

 实现监听器的接口

2、web.xml中注册监听器

3、看情况是否使用

## 12、过滤器监听器常见应用

监听器：GUI编程中经常用

过滤器实现：用户登录之后才能进入主页，用户注销之后不能进入主页

1、用户登录后，向Session中放入用户的数据

2、进入主页时，判断用户是否已经登录；要求：在过滤器中实现



## 13、JDBC

什么是JDBC：java链接数据库

![image-20240125164939026](D:\Studyspace\Markdown\Note\JavaWeb.assets\image-20240125164939026.png)

导入的包

```java
java.sql.
mysql-connecter-java
```



导入数据库依赖

```xml
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
</dependency>
```

1. 加载驱动
2. 连接数据库，代表数据库
3. 向数据库中发送SQL的对象Statement：CRUD
4. 编写SQL（根据业务，不同的SQL）
5. 执行sql
6. 关闭连接



**事务**

**Junit单元测试**

依赖

```xml
<dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
</dependency>
```

@Test注解只在方法上有效，只要加了这个注解的方法可以直接运行



## 14、SMBMS

### 准备工作

1. 搭建一个Maven项目
2. 配置Tomcat
3. 测试项目能否跑起来
4. 导入项目中会遇到的jar包
5. 创建项目包结构
6. 编写实体类：

​		ORM：表-类映射

7. 编写基础公共类
   1. 数据库配置文件
   2. 编写数据库公共类
   3. 编写字符编码过滤器
8. 导入静态资源

### 登录功能实现

1. 编写前端页
2. 设置首页
3. 编写dao层登录用户登录的接口
4. 编写dao接口的实现类
5. 业务层接口
6. 业务层实现类
7. 编写Servlet
8. 注册servlet
9. 测试访问，确保以上功能成功

### 登录功能优化

注销功能

思路：移除Session，返回登录页面

注册xml

登录拦截优化

编写一个过滤器，并注册

### 密码修改

1. 导入前端素材
2. 写项目，建议从底层向上
3. UserDao接口
4. UserDao接口实现类
5. UserService层
6. UserService实现类
7. 实现复用，要提取出方法
8.  测试

**优化密码修改使用Ajax**

1. 阿里巴巴的fastjson
2. 后台代码修改
3. 测试

### 用户管理实现

思路：

![image-20240129162146510](D:\Studyspace\Markdown\Note\JavaWeb.assets\image-20240129162146510.png)

1. 导入分页工具类

2. 用户列表页面导入

   userlist.jsp

#### 获取用户数量

1. UserDao
2. UserDaoImpl
3. UserService
4. UserServiceImpl

#### 获取用户列表

1. UserDao
2. UserDaoImpl
3. UserService
4. UserServiceImpl

#### 获取角色列表

为了职责统一，可以把角色的操作单独放在一个包中，和POJO类对应

1. RolerDao
2. RoleDaoImpl
3. RoleService
4. RoleServiceImpl

#### 用户显示的Servlet

1. 获取用户前端的数据（查询）
2. 判断请求是否需要执行，看参数值判断
3. 为了实现分页，需要计算出当前页面和总页面，页面大小
4. 用户列表展示
5. 返回前端

## 文件上传

1.导入jar包

```java
commons-io
commons-fileupload
```

![image-20240131110914711](D:\Studyspace\Markdown\Note\JavaWeb.assets\image-20240131110914711.png)

[文件上传](https://blog.csdn.net/qq_43167873/article/details/121968044)

## 邮件发送

![img](D:\Studyspace\Markdown\Note\JavaWeb.assets\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNzM0,size_16,color_FFFFFF,t_70.png)

[邮件发送代码连接](https://blog.csdn.net/weixin_45433031/article/details/119900941?ops_request_misc=%7B%22request%5Fid%22%3A%22163961963016780366578915%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=163961963016780366578915&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-119900941.pc_search_all_es&utm_term=%E7%8B%82%E7%A5%9E%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81&spm=1018.2226.3001.4187)

[乱码问题](https://www.bilibili.com/video/BV1UN411x7xe/?p=85&vd_source=d1ae2624740bc9231728a9431f9eaf93)

MVC三层架构

dao

service

controller
