# MyBatis

## 1、简介

- 什么是MyBatis
  - MyBatis 是一款优秀的持久层框架
  - 它支持自定义 SQL、存储过程以及高级映射
  - MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集
  - MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

- 优点：
  - 简单易学
  - 灵活
  - sql和代码的分离，提高了可维护性。
  - 提供映射标签，支持对象与数据库的orm字段关系映射。
  - 提供对象关系映射标签，支持对象关系组建维护。
  - 提供xml标签，支持编写动态sql。

## 2、第一个MyBatis程序

思路：搭建环境—>导入Mybatis—>编写代码---->测试

### 2.1、搭建环境

搭建数据库----新建myBatis数据库创建user表添加数据（这里使用Navicat）

新建项目

1. 新建一个普通的Maven项目
2. 删除src目录
3. 导入maven依赖

### 2.2、创建一个模块

- 编写mybatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--核心配置文件-->
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&amp;characterEncoding=utf-8&amp;useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <!--每一个Mapper.xml，都需要在MyBatis核心配置文件中注册！-->
    <mappers>
        <mapper resource="com/landon/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

mybatis静态资源过滤问题maven中配置

```xml
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



- 编写mybatis的工具类

### 2.3、编写代码

- 实体类
- Dao接口
- 接口实现类由原来的UserDaoImpl转变为一个Mapper配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace=绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.landon.dao.UserDao">
    <!--select查询语句-->
    <select id="getUserList" resultType="com.landon.pojo.User">
        select * from user
    </select>
</mapper>
```



### 2.4、测试

注意：org.apache.ibatis.binding.BindingException: Type interface com.qjd.dao.UserDao is not known to the MapperRegistry.

**MapperRegistry是什么？**

核心配置文件中注册Mappers

- junit

```java
public class UserDaoTest {
    @Test
    public void test() {
        //第一步获取SqlSession对象
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        //方式一：执行SQL
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        List<User> userList = mapper.getUserList();

        for (User user : userList) {
            System.out.println(user);
        }

        //关闭SqlSession
        sqlSession.close();
    }

```

注意问题

1. 配置文件没有注册
2. 绑定接口错误
3. 方法名不对
4. 返回类型不对
5. maven导出资源问题

## 3、CRUD

### 1、namespace

namespace中的包名要和Dao/Mapper接口的包名一致

### 2、select

选择：查询语句

- id：对应的namespace中的方法名
- resultType：sql语句返回值！
- parameterType：参数类型



1. 编写接口
2. 编写对应的Mapper中的sql语句
3. 测试

### 3、insert

### 4、update

### 5、delete

注意点：

- 增删改要提交事务



### 6、常见错误

- 标签不要匹配错
- resource绑定mapper,需要使用路径
- 程序配置文件必须符合规范
- 空指针异常，没有注册到资源
- target输出xml文件中存在中文乱码问题
- maven资源没有导出问题

### 7、万能map

假设，我们的实体类，或者数据库表，字段或者参数过多，我们应当考虑使用map!

- Map传递参数，直接在sql中取出key即可！【parameterType = "map"】
- 对象传递参数，直接在sql中去对象的属性即可！【parameterType = "Object"】
- 只有一个基本类型参数的情况下，可以直接在sql中取到！
- 多个参数用Map,或者注解

### 8、模糊查询

- java代码执行的时候，传递通配符%%

```java
List<User> userList = mapper.getUserLike("%lan%");
```

- 在sql拼接中使用通配符（存在sql注入问题）

```mysql
select * from user where name like  "%"#{value}"%"
```



## 4、配置解析

### 1、核心配置文件（configuration）

- mybatis-config.xml

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：

1. configuration（配置）
   - properties（属性）
   - settings（设置）
   - typeAliases（类型别名）
   - typeHandlers（类型处理器）
   - objectFactory（对象工厂）
   - plugins（插件）
   - environments（环境配置）
   - environment（环境变量）
   - transactionManager（事务管理器）
   - dataSource（数据源）
   - databaseIdProvider（数据库厂商标识）
   - mappers（映射器）

### 2、环境配置（environments）

- MyBatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个SqlSessionFactory 实例只能选择一种环境。**

MyBatis默认的事务管理器就是JDBC，连接池：POOLED



### 3、属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置【db.properties】

db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatsi?useUnicode=true&characterEncoding=utf-8&useSSL=false
username=root
password=123456
```

在核心配置文件中引入(在xml中，所有标签可以规定顺序)

```xml
<!--引入配置文件-->
    <properties resource="db.properties">
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </properties>
```

- 可以直接引入外部文件
- 可以在其中增加一些属性配置
- 如果两个文件有同一个字段，有限使用外部配置文件

### 4、类型别名（typeAliases）

- 类型别名可为 Java 类型设置一个缩写名字。 
- 意在降低冗余的全限定类名书写

```xml
<!--可以给实体类起别名-->
    <typeAliases>
        <typeAlias type="com.landon.pojo.User" alias="User"/>
    </typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean

扫描实体类的包，它的默认别名就为这个类的类名，首字母小写

```xml
<!--可以给实体类起别名-->
<typeAliases>
    <package name="com.landon.pojo"/>
</typeAliases>
```

在实体类比较少的时候，使用第一种方式；

如果实体类十分多，建议使用第二种

第一种可以DIY别名，第二种则不行，如果非要改，需要在实体类上增加注解

```java
@Alias("user")
public class User {}
```

一些为常见的 Java 类型内建的类型别名。它们都是不区分大小写的，注意，为了应对原始类型的命名重复，采取了特殊的命名风格

### 5、设置（setting）

这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为

![img](D:\Studyspace\Markdown\Note\MyBatis.assets\f3b7c0459af622a9f4a0e847d6745ee8.png)

![img](D:\Studyspace\Markdown\Note\MyBatis.assets\7978996b1ac60940dc83f5e218c41141.png)



### 6、其他配置

- typeHandlers（类型处理器）
- objectFactory（对象工厂）
- plugins（插件）
  - mybatis-generator-core
  - mybatis-plus
  - 通用mapper

### 7、映射器（mappers）

MapperRegistry：注册绑定我们的Mapper文件；

方式一：

```xml
<!--每一个Mapper.xml，都需要在MyBatis核心配置文件中注册！-->
<mappers>
    <mapper resource="com/landon/dao/UserMapper.xml"/>
</mappers>
```



方式二：使用class文件绑定注册

```xml

<mappers>
    <mapper class="com.landon.dao.UserMapper"/>
</mappers>
```

注意点：

- 接口和他的Mapper配置文件必须同名
- 接口和它的Mapper配置文件不在同一个包下



方式三：使用扫描包进行注入绑定

```xml
<mappers>
	<package name="com.landon.dao"/>
</mappers>
```

- 接口和他的Mapper配置文件必须同名
- 接口和它的Mapper配置文件不在同一个包下

练习：

1. 将数据库配置文件外部引入
2. 实体类别名
3. 保证UserMapper接口和UserMapper.xml名称一致，且放在同一个包下

### 8、生命周期

![image-20240205144326661](D:\Studyspace\Markdown\Note\MyBatis.assets\image-20240205144326661.png)

生命周期类别是至关重要的，因为错误的使用会导致非常严重的**并发问题**。

**SqlSessionFactoryBuilder**

- 一旦创建了SqlSessionFactory，就不需要它了
- 局部变量

**SqlSessionFactory**

- 说白了就是可以想象为：数据库连接池
- 一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例**
- 最佳作用域是应用作用域。
- 最简单的就是使用**单例模式**或者静态单例模式

**SqlSession**

- 连接到连接池的一个请求！
- SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域

- 用完之后要赶紧关闭，否则资源被占用！

![image-20240205145403782](D:\Studyspace\Markdown\Note\MyBatis.assets\image-20240205145403782.png)

这里面的每个Mapper，就代表一个具体业务



## 5、解决属性名和字段名不一致的问题

### 1、问题

数据库中的字段和实例对象属性字段不一致

会出现取不到值的情况

解决方案：

- 起别名

```mysql
<select id="getUserById" parameterType="int" resultType="user">
select id,name,pwd as password from user where id = #{id}
</select>
```



### 2、ResultMap

结果集映射

```
id	 name	pwd
id	 name	password
```

```xml
<resultMap id="userMap" type="user">
        <!--column数据库中字段，property实体类中的属性-->
        <result column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="pwd" property="password"/>
</resultMap>
<select id="getUserById" parameterType="int" resultMap="userMap">
        select * from user where id = #{id};
</select>
```

- `resultMap` 元素是 MyBatis 中最重要最强大的元素
- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了
- 这就是 `ResultMap` 的优秀之处——你完全可以不用显式地配置它们

## 6、日志

### 1、日志工厂

如果一个数据库操作出现了异常，我们需要排错。日志就是最好的助手！

曾经：sout、debug

现在：日志工厂！

![img](D:\Studyspace\Markdown\Note\MyBatis.assets\f3b7c0459af622a9f4a0e847d6745ee8.png)

- SLF4J |
- LOG4J（3.5.9 起废弃）【掌握】
- LOG4J2
- JDK_LOGGING
- COMMONS_LOGGING
- STDOUT_LOGGING 【掌握】
- NO_LOGGING

在Mybatis中具体使用哪一个日志实现，在设置中设定

**STDOUT_LOGGING 标准日志输出**

在mybatis核心配置文件中，配置我们的日志

```XML
<settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```



![image-20240205165430473](D:\Studyspace\Markdown\Note\MyBatis.assets\image-20240205165430473.png)

### 2、Log4j

什么是Log4j

- Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件
- 我们也可以控制每一条日志的输出格式
- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程
- 通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码

1. 先导包

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

2. log4j.properties

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/landon.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```



3. 配置log4j为日志的实现

```xml
<settings>
        <setting name="logImpl" value="LOG4J"/>
</settings>
```

4. Log4j的使用

内容看测试类控制台

**简单使用**

1. 在要使用Log4j的类中，导入包import org.apache.log4j.Logger;
2. 日志对象，参数为当前类的class

```java
static  Logger logger = Logger.getLogger(UserMapperTest.class);
```

3. 日志级别

```java
logger.info("info:进入了testLog4j");
logger.debug("debug:进入了testLog4j");
logger.error("error:进入了testLog4j");
```

项目导了log4j的包，就要配log4j.properties

## 7、分页

**思考：为什么要分页？**

- 减少数据的处理量

### 1、使用limit分页

```sql
语法：select * from user limit startIndex, pageSize;
select * from user limit 3;  #[0,n)
```



使用MyBatis实现分页，核心SQL

1. 接口

```java
//分页
List<User> getUserByLimit(Map<String, Integer> map);
```



2. Mapper.xml

```xml
<select id="getUserByLimit" parameterType="map" resultMap="userMap">
        select * from user limit #{startIndex}, #{pageSize}
</select>
```



3. 测试

```java
@Test
public void getUserByLimit() {
    SqlSession sqlSession = MyBatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    Map<String, Integer> map = new HashMap<>();
    map.put("startIndex",0);
    map.put("pageSize",2);
    List<User> userList = mapper.getUserByLimit(map);
    for (User user : userList) {
        System.out.println(user);
    }
    sqlSession.close();
}
```





### 2、RowBounds分页

不再使用SQL实现分页

1. 接口

```java
//分页2
List<User> getUserByRowBounds();
```



2. mapper.xml

```xml
<!--分页2-->
<select id="getUserByRowBounds" resultMap="userMap">
        select * from user
</select>
```



3. 测试

```java
@Test
public void getUserByRowBounds() {
    SqlSession sqlSession = MyBatisUtils.getSqlSession();
    RowBounds rowBounds = new RowBounds(1,2);

    //Java代码层面实现分页
    List<User> userList = sqlSession.selectList("com.landon.dao.UserMapper.getUserByRowBounds", null, rowBounds);
    for (User user : userList) {
        System.out.println(user);
    }

    sqlSession.close();
}
```



### 3、分页插件

PageHelper



## 8、使用注解开发

### 1、面向接口编程

我们之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程

**根本原因 : 解耦 , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好**

在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了；

而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。



**关于接口的理解**

- 接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

- 接口的本身反映了系统设计人员对系统的抽象理解。

- 接口应有两类：
  - 第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；
  - 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；

- 一个体有可能有多个抽象面。抽象体与抽象面是有区别的。



**三个面向区别**

-面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .

-面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .

-接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题.更多的体现就是对系统整体的架构



### 2、利用注解开发

1. 注解在接口上实现

```java
@Select("select * from user")
List<User> getUsers();
```



2. 需要在核心配置文件中绑定接口！

```xml
<!--绑定接口-->
<mappers>
    <mapper class="com.landon.dao.UserMapper"/>
</mappers>
```



3. 测试





本质：反射机制实现

底层：动态代理！

![img](D:\Studyspace\Markdown\Note\MyBatis.assets\72b9b5c5544d423fa0d910651999cd82.png)



**MyBatis详细执行流程！**

![mybatis执行流程](D:\Studyspace\Markdown\Note\MyBatis.assets\watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pm05pum,size_18,color_FFFFFF,t_70,g_se,x_16.png)

### 3、CRUD

我们可以在工具类创建的时候实现自动提交事务！

```java
public static SqlSession getSqlSession() {
    return sqlSessionFactory.openSession(true);
}
```



编写接口，增加注解

```java
public interface UserMapper {

    @Select("select * from user")
    List<User> getUsers();

    //方法存在多个参数，所有参数前面必须加上@Param("id")注解
    @Select("select * from user where id = #{id}")
    @Result(property = "password", column = "pwd")
    User getUserById(@Param("id") int id);

    @Insert("insert into user (id, name, pwd) values (#{id}, #{name}, #{password})")
    int addUser(User user);

    @Update("update user set name = #{name}, pwd = #{password} where id = #{id}")
    int updateUser(User user);

    @Delete("delete from user where id = #{uid}")
    int deleteUser(@Param("uid") int id);
}
```



测试

【注意：我们必须要将接口注册绑定到我们的核心配置文件中】



**关于@Param()注解**

- 基本类型的参数类型或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型，可以忽略，但建议加上
- 我们SQL中引用的就是我们这里的@Param("uid")中设定的属性名！



**#{}和${}区别**

#{}

```
    1. #{} 的作用主要是替换**预编译**语句(PrepareStatement)中的占位符? 【推荐使用】

    2. 底层是preparedStatement

    3. 预编译

    4. 能防止sql注入，自动添加了‘ ’ 引号
```



${}

```
  1. 不防注入，就是**字符串拼接**

  2. 只能${}的情况：order by、like 语句只能用${}了,用#{}会多个' '导致sql语句失效；此外动态拼接sql也要用${}

  3. #{} 这种取值是编译好SQL语句再取值

```



详细讲解：

```
1. #将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。如：
order by #user_id#，如果传入的值是111,那么解析成sql时的值为order by "111",
 如果传入的值是id，则解析成的sql为order by "id".
2. $将传入的数据直接显示生成在sql中。如：order by $user_id$，
如果传入的值是111,那么解析成sql时的值为order by user_id, 
 如果传入的值是id，则解析成的sql为order by id.
3. #方式能够很大程度防止sql注入。
4.$方式无法防止Sql注入。
5.$方式一般用于传入数据库对象，例如传入表名.
6.一般能用#的就别用$.
MyBatis排序时使用order by 动态参数时需要注意，用$而不是#
字符串替换
默认情况下，使用#{}格式的语法会导致MyBatis创建预处理语句属性并以它为背景设置
安全的值（比如?）。这样做很安全，很迅速也是首选做法
有时你只是想直接在sql语句中插入一个不改变的
字符串。比如，像ORDER BY，你可以这样来使用：ORDER BY ${columnName}
这里MyBatis不会修改或转义字符串。重要：接受从用户输出的内容并提供给语句中
不变的字符串，这样做是不安全的。这会导致潜在的sql注入攻击，因此你不应该
允许用户输入这些字段，或者通常自行转义并检查。
```



## 9、Lombok

### 1、使用步骤

1. 在IDEA中安装lombok插件：File —> Settings —> Plugins —> Browse repositories —> 搜索lombok
2. 在pom.xml中导入lombok的maven依赖（包）

```xml
<dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.28</version>
</dependency>
```

3. 在实体类上加注解即可



### 2、lombok常用注解

@Data : 自动生成set/get方法，toString方法，equals方法，hashCode方法，不带参数的构造方法

@NoArgsConstructor/@RequiredArgsConstructor/@AllArgsConstructor
自动生成无参/有参构造方法，显示定义有参，要手动加无参构造

@Setter/@Getter : 自动生成set和get方法

@ToString : 自动生成toString方法

@EqualsAndHashcode : 从对象的字段中生成hashCode和equals方法

以下为不常用注解：

@NonNull : 用在成员方法或者构造方法的参数前面，会自动产生一个关于此参数的非空检查，如果参数为空，则抛出一个空指针异常

@CleanUp : 自动资源管理：不用再在finally中添加资源的close方法

@Value : 用于注解final类

@Builder : 产生复杂的构建器api类

@SneakyThrows : 异常处理（谨慎使用）

@Synchronized : 同步方法安全的转化

@Getter(lazy=true) :

@Log: 支持各种logger对象，使用时用对应的注解，如：@Log4j

### 3、优缺点

优点:

能通过注解的形式自动生成构造器、getter/setter、equals、hashcode、 toString等方法，提高了一定的开发效率

让代码变得简洁，不用过多的去关注相应的方法

属性做修改时，也简化了维护为这些属性所生成的getter/setter方法等

缺点:

不支持多种参数构造器的重载

虽然省去了手动创建getter/seter方法的麻烦，但大大降低了源代码的可读性和完整性，降低了阅读源代码的舒适度



## 10、多对一处理

- 多个学生，对应一个老师
- 对于学生这边而言，**关联**。多个学生，关联一个老师【多对一】
- 对于老师而言，**集合**。一个老师，有很多学生【一对多】

![n：1](D:\Studyspace\Markdown\Note\MyBatis.assets\bd2c72d4817781f78d0c9056d7f93976.png)



SQL:

```mysql
CREATE TABLE `teacher` (
   `id` INT(10) NOT NULL,
   `name` VARCHAR(30) DEFAULT NULL,
   PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');

CREATE TABLE `student` (
   `id` INT(10) NOT NULL,
   `name` VARCHAR(30) DEFAULT NULL,
   `tid` INT(10) DEFAULT NULL,
   PRIMARY KEY (`id`),
   KEY `fktid` (`tid`),
   CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;


INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1');
```



**测试环境搭建**

1. 导入lombok
2. 新建实体类Teacher，Student
3. 建立Mapper接口
4. 建立Mapper.XML文件
5. 在核心配置文件中绑定注册我们的Mapper接口或者文件!【方式很多。随心选】
6. 测试查询是否能够成功!

### 按照查询嵌套处理

```xml
<mapper namespace="com.landon.dao.StudentMapper">
    <!--
    思路
        1、查询所有学生信息
        2、根据查询出来的学生的tid，寻找对应的老师！
    -->
    <select id="getStudent" resultMap="StudentTeacher">
        select * from student
    </select>
    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <!--复杂的属性，我们要单独处理
            对象：association
            集合：collection
        -->
        <association property="teacher" column="tid" javaType="Teacher"
        select="getTeacher"/>
    </resultMap>

    <select id="getTeacher" resultType="teacher">
        select * from teacher where id = #{id}
    </select>
</mapper>
```

### 按照结果嵌套处理

```xml
<!--按照结果 嵌套处理-->
<select id="getStudent2" resultMap="StudentTeacher2">
    select s.id sid, s.name sname, t.name tname from student s, teacher t where s.tid = t.id
</select>
<resultMap id="StudentTeacher2" type="Student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
```



- 按照查询进行嵌套处理就像SQL中的子查询
- 按照结果进行嵌套处理就像SQL中的联表查询



## 11、一对多处理

一个老师拥有多个学生！

对于老师，就是一对多的关系！

### 按照结果嵌套处理

```xml
<!--按结果嵌套查询-->
<select id="getTeacher" resultMap="TeacherStudent">
    select s.id sid, s.name sname, t.name tname, t.id tid
    from student s, teacher t
    where s.tid = t.id and t.id = #{tid}
</select>

<resultMap id="TeacherStudent" type="Teacher">
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>
    <!--复杂的属性，我们要单独处理
        对象：association
        集合：collection
        javaType指定属性的类型！
        集合中的泛型信息，使用ofType获取
    -->
    <collection property="students" ofType="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>
```



### 按照查询嵌套处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
    select * from teacher where id = #{tid};
</select>
<resultMap id="TeacherStudent2" type="Teacher">
    <collection property="students" javaType="ArrayList" ofType="Student" select="getStudentByTeacherId" column="id"/>
</resultMap>

<select id="getStudentByTeacherId" resultType="student">
    select * from student where tid = #{id}
</select>
```



### 小结

1. 关联 association 【多对一】
2. 集合 collection 【一对多】
3. javaType & ofType
   1. javaType 用来指定实体类中属性的类型
   2. ofType 用来指定映射到List或者集合中的POJO类型，泛型中的约束类型！

注意点：

- 保证SQL可读性，尽量通俗易懂
- 注意一对多和多对一 中：字段和属性对应的问题
- 尽量使用Log4j，通过日志来查看自己的错误

**面试高频**：

- Mysql引擎
- InnoDB底层原理
- 索引
- 索引优化

## 12、动态SQL

什么是动态SQL：动态SQL就是指根据不同条件生成不同的SQL语句

- if
- choose (when, otherwise)
- trim (where, set)
- foreach

### 环境搭建

```mysql
CREATE TABLE `blog` (
`id` varchar(50) NOT NULL COMMENT '博客id',
`title` varchar(100) NOT NULL COMMENT '博客标题',
`author` varchar(30) NOT NULL COMMENT '博客作者',
`create_time` datetime NOT NULL COMMENT '创建时间',
`views` int(30) NOT NULL COMMENT '浏览量'
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

创建一个基础工程

1. 导包
2. 编写配置文件
3. 编写实体类
4. 编写实体类对应的Mapper接口和Mapper.xml文件

### IF

```xml
<select id="queryBlogIF" parameterType="Map" resultType="blog">
    select * from blog where 1=1
    <if test="title != null">
        and title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</select>
```

### choose、when、otherwise

```xml
<select id="queryBlogChoose" parameterType="Map" resultType="blog">
    select * from blog
    <where>
        <choose>
            <when test="title != null">
                title = #{title}
            </when>
            <when test="author != null">
                and author = #{author}
            </when>
            <otherwise>
                and views = #{views}
            </otherwise>
        </choose>
    </where>
</select>
```

类似于switch case

### trim(where, set)

```xml
<select id="queryBlogIF" parameterType="Map" resultType="blog">
    select * from blog
    <where>
        <if test="title != null">
            title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </where>
</select>
```

```xml
<update id="updateBlog" parameterType="map">
    update blog
    <set>
        <if test="title != null">
            title = #{title},
        </if>
        <if test="author != null">
            author = #{author}
        </if>
    </set>
    where id = #{id}
</update>
```



所谓的动态SQL，本质还是SQL语句，只是我们可以再SQL层面，去执行一个逻辑代码



### SQL片段

有的时候，我们会将一些公共的部分抽取出来，方便复用！

1. 使用SQL标签抽取公共部分

   ```xml
   <sql id="if-title-author">
           <if test="title != null">
               title = #{title}
           </if>
           <if test="author != null">
               and author = #{author}
           </if>
       </sql>
   ```

   

2. 在需要使用的地方使用include标签引用即可

```xml
<select id="queryBlogIF" parameterType="Map" resultType="blog">
        select * from blog
        <where>
            <include refid="if-title-author"></include>
        </where>
    </select>
```



注意事项：

- 最好基于单表来定义SQL片段
- 不要存在where标签



### Foreach

```xml
<select id="queryBlogForeach" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <foreach collection="ids" item="id" open="and (" close=")" separator="or">
            id = #{id}
        </foreach>
    </where>
</select>
```



动态SQL就是在拼接SQL，我们只要保证SQL的正确性，按照SQL的格式，去排列组合就可以了

建议：

- 先在MySQL中写出完整生气了，再去对应的修改为我们的动态SQL实现



## 13、缓存

### 1、简介

1、什么是缓存 [ Cache ]？

- 存在内存中的临时数据。

- 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。

2、为什么使用缓存？

- 减少和数据库的交互次数，减少系统开销，提高系统效率。
  3、什么样的数据能使用缓存？

- 经常查询并且不经常改变的数据。



### 2、MyBatis缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。

- MyBatis系统中默认定义了两级缓存：一级缓存和二级缓存
  - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
  - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存



### 3、一级缓存

一级缓存也叫***本地(会话)缓存***：

- 与数据库同一次会话(sqlSession)期间查询到的数据会放在本地缓存中。
- 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库；

测试步骤：

1. 开启日志
2. 测试在一个Session中查询两次相同记录
3. 查看日志输出



#### 一级缓存失效的四种情况

一级缓存是SqlSession级别的缓存，是一直开启的，我们关闭不了它；

一级缓存失效情况：没有使用到当前的一级缓存，效果就是，还需要再向数据库中发起一次查询请求！

1. 查询不同的记录
2. 增删改操作，可能会改变原来的数据，所以必定会刷新缓存！
3. 查询不同的Mapper.xml
4. 手动清理缓存！



一级缓存就是一个map

小结：一级缓存默认是开启的，只在一次SqlSession中有效，也就是拿到连接到关闭连接这个区间段(相当于一个用户不断查询相同的数据，比如不断刷新)，一级缓存就是一个map



### 4、二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存
- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；
- 工作机制
  - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中；
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中；

#### 使用步骤

1、开启全局缓存 【mybatis-config.xml】

```xml
<!--cacheEnabled的默认值为true-->
<setting name="cacheEnabled" value="true"/>
```



2、去每个mapper.xml中配置使用二级缓存，这个配置非常简单【xxxMapper.xml】

可以不加属性，也可以自定义属性

```xml
<!--在当前mapper.xml文件中开启二级缓存-->
<cache/>

官方示例=====>查看官方文档
<cache
 eviction="FIFO"
 flushInterval="60000"
 size="512"
 readOnly="true"/>
这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。
```



3、代码测试

- 实体类要先实现序列化接口，否则报错：`Cause: java.io.NotSerializableException: com.kuang.pojo.User`
- 测试代码



#### 结论

- 只要开启了二级缓存，我们在同一个Mapper中的查询，可以在二级缓存中拿到数据
- 查出的数据都会被默认先放在一级缓存中
- 只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中



### 5、缓存原理图

缓存顺序：

1 先看二级缓存中有没有

1. 再看一级缓存中有没有
2. 查询数据库

注：一二级缓存都没有，查询数据库，查询后将数据放入一级缓存

![cache](D:\Studyspace\Markdown\Note\MyBatis.assets\0e25f884d40348b8837f057a54c60081.png)

### 6、自定义缓存-ehcache

- Ehcache是一种广泛使用的开源Java分布式缓存。主要面向通用缓存

**使用方法**

1. 要在应用程序中使用Ehcache，需要引入依赖的jar包

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.2.3</version>
</dependency>

```



2. 配置mapper中cache type

```xml
<mapper namespace = "com.kuang.dao.UserMapper" >
    <!--开启ehcache缓存-->
    
    <!-- 以下两个<cache>标签二选一,第一个可以输出日志,第二个不输出日志 -->
    <cache type="org.mybatis.caches.ehcache.LoggingEhcache" />

    <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
</mapper>
```



3. 加入Ehcache的配置文件

在src/main/resources/下创建编写ehcache.xml文件，如果在加载时未找到ehcache.xml资源或出现问题，则将使用默认配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
        updateCheck="false">
   <!--
      diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
      user.home – 用户主目录
      user.dir – 用户当前工作目录
      java.io.tmpdir – 默认临时文件路径
    -->
   <diskStore path="./tmpdir/Tmp_EhCache"/>
   
   <defaultCache
           eternal="false"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           diskPersistent="false"
           timeToIdleSeconds="1800"
           timeToLiveSeconds="259200"
           memoryStoreEvictionPolicy="LRU"/>

   <cache
           name="cloud_user"
           eternal="false"
           maxElementsInMemory="5000"
           overflowToDisk="false"
           diskPersistent="false"
           timeToIdleSeconds="1800"
           timeToLiveSeconds="1800"
           memoryStoreEvictionPolicy="LRU"/>
   <!--
      defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
    -->
   <!--
     name:缓存名称。
     maxElementsInMemory:缓存最大数目
     maxElementsOnDisk：硬盘最大缓存个数。
     eternal:对象是否永久有效，一但设置了，timeout将不起作用。
     overflowToDisk:是否保存到磁盘，当系统当机时
     timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
     timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
     diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
     diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
     diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
     memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
     clearOnFlush：内存数量最大时是否清除。
     memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
     FIFO，first in first out，这个是大家最熟的，先进先出。
     LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
     LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
  -->

</ehcache>
```



**自定义二级缓存**

1. 实现Cache接口
2. 配置mapper中cache type

