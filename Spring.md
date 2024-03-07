# 1、Spring

## 1.1、简介

Spring : 春天 —>给软件行业带来了春天

2002年，Rod Jahnson首次推出了Spring框架雏形interface21框架。

2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。

很难想象Rod Johnson的学历 , 他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。

Spring理念 : 使现有技术更加实用 . 本身就是一个大杂烩 , 整合现有的框架技术

官网 : http://spring.io/

官方下载地址 : https://repo.spring.io/libs-release-local/org/springframework/spring/

GitHub : https://github.com/spring-projects

**配置文件**

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.9</version>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.9</version>
</dependency>
```



## 1.2、优点

1. Spring是一个开源免费的框架 , 容器 。
2. Spring是一个轻量级的框架 , 非侵入式的 。
3. **控制反转 IoC , 面向切面 Aop**
4. 对事务的支持 , 对框架的支持

...

一句话概括：

Spring是一个轻量级的[控制反转](https://so.csdn.net/so/search?q=控制反转&spm=1001.2101.3001.7020)(IoC)和面向切面(AOP)的容器（框架）。



## 1.3、组成

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png.png)

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式 .

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-17089960152962.png)

组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

- **核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是工厂模式的实现。BeanFactory 使用控制反转（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- **Spring 上下文**：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- **Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- **Spring Web 模块**：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- **Spring MVC 框架**：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。



## 1.4、拓展

**Spring Boot与Spring Cloud**

- Spring Boot 是 Spring 的一套快速配置脚手架，可以基
- Spring Boot 快速开发单个微服务;
- Spring Cloud是基于Spring Boot实现的；Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架；
- Spring Boot使用了约束优于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置 , Spring Cloud很大的一部分是基于Spring Boot来实现，Spring Boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring Boot，属于依赖的关系。
- SpringBoot在SpringClound中起到了承上启下的作用，如果你要学习SpringCloud必须要学习SpringBoot。
  

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-17089961726724.png)

- SpringBoot
  - 一个快速开发脚手架
  - 基于SpringBoot可以快速的开发单个微服务
  - 约定大于配置
- Spring Cloud
  - SpringCloud基于SpringBoot实现

# 2、IoC理论

## 1、IoC基础

创建一个空白maven项目

我们先用我们原来的方式写一段代码 .

1、先写一个UserDao接口

```java
public interface UserDao {
   public void getUser();
}
```

2、再去写Dao的实现类

```java
public class UserDaoImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("获取用户数据");
  }
}
```

3、然后去写UserService的接口

```java
public interface UserService {
   public void getUser();
}
```

4、最后写Service的实现类

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

5、测试一下

```java
@Test
public void test(){
   UserService service = new UserServiceImpl();
   service.getUser();
}
```

这是我们原来的方式 , 开始大家也都是这么去写的对吧 . 那我们现在修改一下 .

把Userdao的实现类增加一个 .

```java
public class UserDaoMySqlImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("MySql获取用户数据");
  }
}
```

紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoMySqlImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

在假设, 我们再增加一个Userdao的实现类 .

```java
public class UserDaoOracleImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("Oracle获取用户数据");
  }
}
```

那么我们要使用Oracle , 又需要去service实现类里面修改对应的实现 . 假设我们的这种需求非常大 , 这种方式就根本不适用了, 甚至反人类对吧 , 每次变动 , 都需要修改大量代码 . 这种设计的[耦合性](https://so.csdn.net/so/search?q=耦合性&spm=1001.2101.3001.7020)太高了, 牵一发而动全身 .

**那我们如何去解决呢 ?**

我们可以在需要用到他的地方 , 不去实现它 , 而是留出一个接口 , 利用set , 我们去代码里修改下 .

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao;
// 利用set实现
   public void setUserDao(UserDao userDao) {
       this.userDao = userDao;
  }

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

现在去我们的测试类里 , 进行测试 ;

```java
@Test
public void test(){
   UserServiceImpl service = new UserServiceImpl();
   service.setUserDao( new UserDaoMySqlImpl() );
   service.getUser();
   //那我们现在又想用Oracle去实现呢
   service.setUserDao( new UserDaoOracleImpl() );
   service.getUser();
}
```

大家发现了区别没有 ? 可能很多人说没啥区别 . 但是同学们 , 他们已经发生了根本性的变化 , 很多地方都不一样了 . 仔细去思考一下 , 以前所有东西都是由程序去进行控制创建 , 而现在是由我们自行控制创建对象 , 把主动权交给了调用者 . 程序不用去管怎么创建,怎么实现了 . 它只负责提供一个接口 .

这种思想 , 从本质上解决了问题 , 我们程序员不再去管理对象的创建了 , 更多的去关注业务的实现 . 耦合性大大降低 . 这也就是IOC的原型 !

## 2、IoC本质

**控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。
![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-17089967713836.png)

**IoC是Spring框架的核心内容**，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。

Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-17089968163978.png)

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

上一期中我们理解了IOC的基本思想，我们现在来看下Spring的应用：

# 3、HelloSpring

导入jar包

注 : spring 需要导入commons-logging进行日志记录 . 我们利用maven , 他会自动下载对应的依赖项 .

```xml
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>5.1.10.RELEASE</version>
</dependency>
```

编写代码

1、编写一个Hello实体类

```java
public class Hello {
   private String name;

   public String getName() {
       return name;
  }
   public void setName(String name) {
       this.name = name;
  }

   public void show(){
       System.out.println("Hello,"+ name );
  }
}
```

2、编写我们的spring文件 , 这里我们命名为beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

   <!--bean就是java对象 , 由Spring创建和管理-->
   <bean id="hello" class="com.kuang.pojo.Hello">
       <property name="name" value="Spring"/>
   </bean>

</beans>
```

3、我们可以去进行测试了

```java
@Test
public void test(){
   //解析beans.xml文件 , 生成管理相应的Bean对象
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   //getBean : 参数即为spring配置文件中bean的id .
   Hello hello = (Hello) context.getBean("hello");
   hello.show();
}
```

> 思考

- Hello 对象是谁创建的 ? 【hello 对象是由Spring创建的】
- Hello 对象的属性是怎么设置的 ? 【hello 对象的属性是由Spring容器设置的】

这个过程就叫控制反转 :

- 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的
- 反转 : 程序本身不创建对象 , 而变成被动的接收对象 .

依赖注入 : 就是利用set方法来进行注入的.

IOC是一种编程思想，由主动的编程变成被动的接收

可以通过new ClassPathXmlApplicationContext去浏览一下底层源码 .

> 修改案例一

我们在案例一中， 新增一个Spring配置文件beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

   <bean id="MysqlImpl" class="com.kuang.dao.impl.UserDaoMySqlImpl"/>
   <bean id="OracleImpl" class="com.kuang.dao.impl.UserDaoOracleImpl"/>

   <bean id="ServiceImpl" class="com.kuang.service.impl.UserServiceImpl">
       <!--注意: 这里的name并不是属性 , 而是set方法后面的那部分 , 首字母小写-->
       <!--引用另外一个bean , 不是用value 而是用 ref-->
       <property name="userDao" ref="OracleImpl"/>
   </bean>

</beans>
```

测试

```java
@Test
public void test2(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   UserServiceImpl serviceImpl = (UserServiceImpl) context.getBean("ServiceImpl");
   serviceImpl.getUser();
}
```

**OK , 到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 !**



# 4、IoC创建对象方式

> 通过无参构造方法来创建

1、User.java

```java
public class User {

   private String name;

   public User() {
       System.out.println("user无参构造方法");
  }

   public void setName(String name) {
       this.name = name;
  }

   public void show(){
       System.out.println("name="+ name );
  }

}
```

2、beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

   <bean id="user" class="com.kuang.pojo.User">
       <property name="name" value="kuangshen"/>
   </bean>

</beans>
```

3、测试类

```java
@Test
public void test(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   //在执行getBean的时候, user已经创建好了 , 通过无参构造
   User user = (User) context.getBean("user");
   //调用对象的方法 .
   user.show();
}
```

结果可以发现，在调用show方法之前，User对象已经通过无参构造初始化了！

> **通过有参构造方法来创建**

1、UserT . java

```java
public class UserT {

   private String name;

   public UserT(String name) {
       this.name = name;
  }

   public void setName(String name) {
       this.name = name;
  }

   public void show(){
       System.out.println("name="+ name );
  }

}
```

2、beans.xml 有三种方式编写

```xml
<!-- 第一种根据index参数下标设置 -->
<bean id="userT" class="com.kuang.pojo.UserT">
   <!-- index指构造方法 , 下标从0开始 -->
   <constructor-arg index="0" value="kuangshen2"/>
</bean>
<!-- 第二种根据参数名字设置 -->
<bean id="userT" class="com.kuang.pojo.UserT">
   <!-- name指参数名 -->
   <constructor-arg name="name" value="kuangshen2"/>
</bean>
<!-- 第三种根据参数类型设置 -->
<bean id="userT" class="com.kuang.pojo.UserT">
   <constructor-arg type="java.lang.String" value="kuangshen2"/>
</bean>
```

3、测试

```java
@Test
public void testT(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   UserT user = (UserT) context.getBean("userT");
   user.show();
}
```

结论：在配置文件加载的时候。其中管理的对象都已经初始化了！



# 5、Spring配置

## 1、别名

alias 设置别名 , 为bean设置别名 , 可以设置多个别名

```xml
<!--设置别名：在获取Bean的时候可以使用别名获取-->
<alias name="userT" alias="userNew"/>
```



## 2、Bean的配置

```xml
<!--bean就是java对象,由Spring创建和管理-->

<!--
   id 是bean的标识符,要唯一,如果没有配置id,name就是默认标识符
   如果配置id,又配置了name,那么name是别名
   name可以设置多个别名,可以用逗号,分号,空格隔开
   如果不配置id和name,可以根据applicationContext.getBean(.class)获取对象;

class是bean的全限定名=包名+类名
-->
 <!--
    id：bean的唯一标识符，就是相当于对象名
    class：bean对象所对应的全限定名：包名+类名
    name：也是别名,可以同时设置多个别名-->
<bean id="hello" name="hello2 h2,h3;h4" class="com.kuang.pojo.Hello">
   <property name="name" value="Spring"/>
</bean>
```



## 3、import

import，一般用于团队开发使用，他可以将多个配置文件，导入合并为一个

假设，现在项目中有多个人开发，这三个人负责不同的类开发，不同的类需要注册在不同的bean中，我们可以利用import将所有人的beans.xml合并为一个总的！

- 张三
- 李四
- 王五
- applicationContext.xml

```xml
<import resouce="beans.xml"/>
<import resouce="beans2.xml"/>
<import resouce="beans3.xml"/>
```

使用时，直接使用总的配置就可以了

# 6、DI依赖注入

概念

- 依赖注入（Dependency Injection,DI）。
- 依赖 : 指Bean对象的创建依赖于容器 . Bean对象的依赖资源 .
- 注入 : 指Bean对象所依赖的资源 , 由容器来设置和装配 .

## 1、构造器注入

前面“4、IoC创建对象方式” 通过有参构造方法来创建已讲

## 2、set方式注入【重点】

- 依赖注入：set注入
  - 依赖：bean对象创建依赖于容器！
  - 注入：bean对象中所有属性，由容器来注入！

要求被注入的属性 , 必须有set方法 , set方法的方法名由set + 属性首字母大写 , 如果属性是boolean类型 , 没有set方法 , 是 is .

测试pojo类 :

Address.java

```java
public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}
```

Student.java

```java
package com.landon.pojo;

import java.util.*;

public class Student {
    private String name;
    private  Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String, String> card;
    private Set<String> games;
    private String wife;
    private Properties info;

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", address=" + address.toString() +
                ", books=" + Arrays.toString(books) +
                ", hobbys=" + hobbys +
                ", card=" + card +
                ", games=" + games +
                ", wife='" + wife + '\'' +
                ", info=" + info +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String[] getBooks() {
        return books;
    }

    public void setBooks(String[] books) {
        this.books = books;
    }

    public List<String> getHobbys() {
        return hobbys;
    }

    public void setHobbys(List<String> hobbys) {
        this.hobbys = hobbys;
    }

    public Map<String, String> getCard() {
        return card;
    }

    public void setCard(Map<String, String> card) {
        this.card = card;
    }

    public Set<String> getGames() {
        return games;
    }

    public void setGames(Set<String> games) {
        this.games = games;
    }

    public String getWife() {
        return wife;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }

    public Properties getInfo() {
        return info;
    }

    public void setInfo(Properties info) {
        this.info = info;
    }
}
```

各类注入

```xml
<bean id="address" class="com.landon.pojo.Address">
        <property name="address" value="Shanghai"/>
    </bean>
    <bean id="student" class="com.landon.pojo.Student">
        <!--第一种，普通注入，value-->
        <property name="name" value="landon"/>

        <!--第二种，bean注入，ref-->
        <property name="address" ref="address"/>

        <!--数组-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>三国演义</value>
                <value>水浒传</value>
                <value>西游记</value>
            </array>
        </property>

        <!--list-->
        <property name="hobbys">
            <list>
                <value>listen to music</value>
                <value>coding</value>
                <value>movie</value>
            </list>
        </property>

        <!--map-->
        <property name="card">
            <map>
                <entry key="ID card" value="222222222222222222"/>
                <entry key="bank card" value="1111111111111111111"/>
            </map>
        </property>

        <!--set-->
        <property name="games">
            <set>
                <value>lol</value>
                <value>pubg</value>
                <value>apex</value>
            </set>
        </property>

        <!--null-->
        <property name="wife">
            <null/>
        </property>
        
       <!--properties-->
        <property name="info">
            <props>
                <prop key="学号">16058424</prop>
                <prop key="性别">male</prop>
                <prop key="username">root</prop>
                <prop key="password">123456</prop>
            </props>
        </property>
    </bean>
```



## 3、拓展方式注入

User.java

```java
package com.landon.pojo;

public class User {
    private String name;
    private int age;

    public User() {
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

使用

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入，可以直接注入属性的值：property-->
    <bean id="user" class="com.landon.pojo.User" p:name="landon" p:age="18"/>

    <!--c命名空间注入，通过构造器注入：constructor-ref-->
    <bean id="user2" class="com.landon.pojo.User" c:age="18" c:name="Hsu"/>
</beans>
```

测试：

```java
@Test
    public void test2() {
        ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
        User user = context.getBean("user2", User.class);
        System.out.println(user.toString());
    }
```



注意点：p命名和c命名空间不能直接使用，需要导入xml约束！

```xml
xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
```

## 4、bean的作用域

![img](D:\Studyspace\Markdown\Note\Spring.assets\VR[]5EZOHG}1V0TR3L694ZP.png)

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170901622478910.png)

1、单例模式（Spring默认机制）

```xml
<bean id="user2" class="com.landon.pojo.User" c:age="18" c:name="Hsu" scope="singleton"/>
```

2、原型模式：每次从容器中get的时候，都会产生一个新对象

```xml
<bean id="user2" class="com.landon.pojo.User" c:age="18" c:name="Hsu" scope="prototype"/>
```

3、其余的request、session、application、这些个只能在web开发中使用到！

# 7、bean的自动装配

- 自动装配是Spring满足bean依赖一种方式
- spring会在上下文中自动寻找，并自动给bean装配属性！



在Spring中有三种装配方式

1. 在xml中显示配置
2. 在java中显示配置
3. 隐式的自动装配bean【重要】



## 1、测试

环境搭建：一个人有两个宠物



## 2、byName自动装配

```xml
<bean id="dog" class="com.landon.pojo.Dog"/>
    <bean id="cat" class="com.landon.pojo.Cat"/>

    <!--
    byName：会自动在容器上下问中查找，和自己对象set方法后面的值对应的beanid！
    -->
    <bean id="people" class="com.landon.pojo.People" autowire="byName">
        <property name="name" value="landon"/>
    </bean>
```



## 3、byType自动装配

```xml
<bean id="dog" class="com.landon.pojo.Dog"/>
    <bean id="cat" class="com.landon.pojo.Cat"/>

    <!--
    byName：会自动在容器上下问中查找，和自己对象set方法后面的值对应的beanid！
    byType：会自动在容器上下问中查找，和自己对象属性相同的bean！
    -->
    <bean id="people" class="com.landon.pojo.People" autowire="byType">
        <property name="name" value="landon"/>
    </bean>
```

小结：

- byName的时候，要保证所有的bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致！
- byType的时候，要保证所有的bean的class唯一，并且这个bean需要和自动注入的属性的类型一致！



Spring的自动装配需要从两个角度来实现，或者说是两个操作：

组件扫描(component scanning)：spring会自动发现应用上下文中所创建的bean；
自动装配(autowiring)：spring自动满足bean之间的依赖，也就是我们说的IoC/DI；
组件扫描和自动装配组合发挥巨大威力，使得显示的配置降低到最少。

**推荐不使用自动装配xml配置 , 而使用注解 .**

## 4、使用注解实现自动装配

jdk1.5开始支持注解，spring2.5开始全面支持注解。

使用注解须知：

1、导入约束。context约束

2、配置注解的支持：context:annotation-config

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

### @Autowired

- **@Autowired是按类型自动转配的，不支持id匹配。**

直接在属性上使用即可，也可以在set方式上使用！

使用Autowired我们可以不用编写set方法，前提是这个自动装配的属性在Ioc（Spring）容器中存在，且符合类型byType

科普

```java
@Nullable 字段标记了这个注解，说明这个字段可以为null;
```

```java
public @interface Autowired {
    boolean required() default true;
}
```

测试代码：

```java
public class People {
    //显示定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

@Qualifier

- @Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配
- @Qualifier不能单独使用。

如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，可以使用@Qualifier(value="xxx")匹配id去配合@Autowired使用，指定唯一的bean对象注入！

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    @Qualifier(value="dog222")
    private Dog dog;
    private String name;
}
```

#### @Resource

- @Resource如有指定的name属性，先按该属性进行byName方式查找装配；
- 其次再进行默认的byName方式进行装配；
- 如果以上都不成功，则按byType的方式自动装配。
- 都不成功，则报异常。

```java
public class People {
    @Resource(name = "cat2")
    private Cat cat;
    @Resource
    private Dog dog;
    private String name;
}
```

小结：

@Resource和@Autowired区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byType实现，且必须要存在这个对象
- @Resource默认通过byName方式实现，找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错。
- 执行顺序不同：Autowired通过byType实现，@Resource默认通过byName方式实现

# 8、使用注解开发

Spring4之后，使用注解开发，必须要保证aop包导入

使用注解需要导入context约束，增加注解的支持

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
    <!--开启注解的支持-->
    <context:annotation-config/>
</beans>
```



1. bean

2. 属性如何注入

   ```java
   //等价于<bean id="user" class="com.landon.pojo"/>
   //@Component 组件
   @Component
   public class User {
       //相当于<property name="name" value="landon"/>
       @Value("landon")
       public String name;
   }
   ```

   

3. 衍生的注解
   @Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层

   - dao【@Repository】

   - service【@Service】

   - controller【@Controller】

     这四个注解功能都是一样的，都是代表将某个类注册到Spring中，装配Bean

4. 自动装配

   ```
   @Autowired: 自动装配通过类型，名字
   	如果Autowired不能唯一自动装配上属性，则需要通过@Qualifier(value="xxx")
   @Nullable 字段标记了这个注解，说明这个字段可以为null
   @Resource：自动装配通过名字、类型
   ```

   

5. 作用域

   ```java
   @Component
   @Scope("Singleton")
   public class User {
       //相当于<property name="name" value="landon"/>
       @Value("landon")
       public String name;
   }
   ```

   

6. 小结
   xml与注解：

   - xml更加万能，适用于任何瀍河！维护简单方便
   - 注解 不是自己提供类使用不了，维护相对复杂，开发简单

   xml与注解最佳时间

   - xml用来管理bean

   - 注解只负责完成属性的注入

   - 我们使用过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持

     ```xml
      <!--指定要扫描的包，这个包下的注解就会生效-->
         <context:component-scan base-package="com.landon"/>
         <context:annotation-config/>
     ```



# 9、使用Java的方式配置Spring

我们现在完全不使用Spring的xml配置，全权交给Java来做！

JavaConfig是Spring的一个子项目，在Spring4之后，成了核心功能

实体类

```java
//说明这个类被Spring接管，注册到了容器中
@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }

    @Value("landon") //属性注入值
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

配置文件

```java
package com.landon.config;

import com.landon.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

//这个也会被Spring容器托管，注册到容器中，因为它本身就是@Component
//@Configuration代表这是个配置类，就和我们之前的beans.xml
@Configuration
@ComponentScan("com.landon.pojo")
@Import(MyConfig2.class)
public class MyConfig {
    //注册一个bean，就相当于我们之前写的一个bean标签
    //这个方法的名字，就相当于bean标签中的id属性
    //这个方法的返回值，相当于bean表爱你中的class属性
    @Bean
    public User getUser() {
        return new User(); //就是返回要注入到bean的对象
    }
}
```

测试类

```java
public class MyTest {
    public static void main(String[] args) {
        //如果完全使用了配置类方式去做，我们就只能通过AnnotationConfig上下文来获取容器，通过配置类的class对象加载！
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        User getUser = (User) context.getBean("getUser");
        System.out.println(getUser.getName());
    }
}
```

这种纯java的配置方式，在SpringBoot中随处可见！

# 10、代理模式

为什么要学习代理模式？因为这是SpringAOP的底层！【SpringAOP和SpringMVC】

代理模式的分类：

- 静态代理
- 动态代理

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170910698664612.png)

## 1、静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- 客户：访问代理对象的人

代码步骤：

1. 接口

   ```java
   public interface Rent {
       public void rent();
   }
   ```

   

2. 真实角色

   ```java
   //房东
   public class Host implements Rent{
       @Override
       public void rent() {
           System.out.println("房东要出租房子");
       }
   }
   ```

   

3. 代理角色

   ```java
   public class Proxy implements Rent{
       private Host host;
   
       public Proxy() {
       }
   
       public Proxy(Host host) {
           this.host = host;
       }
   
       @Override
       public void rent() {
           seeHouse();
           host.rent();
           contract();
           fare();
       }
   
       //看房
       public void seeHouse() {
           System.out.println("中介带你看房");
       }
   
       //签合同
       public void contract() {
           System.out.println("中介带你看房");
       }
   
       //收中介费
       public void fare() {
           System.out.println("收中介费");
       }
   }
   ```

   

4. 客户端访问代理角色

   ```java
   public class Client {
       public static void main(String[] args) {
           //房东要租房子
           Host host = new Host();
           //代理。中介帮房东租房子，但是，代理角色一般会有一些负数操作！
           Proxy proxy = new Proxy(host);
   
           //你不用面对房东，直接找中介租房即可！
           proxy.rent();
       }
   }
   ```

   

代理模式的好处：

- 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
- 公共也就交给代理角色！实现业务分工
- 公共业务发生扩展，方便集中管理

缺点：

- 一个真实角色就会产生一个代理角色；代码量会翻倍，开发效率低

## 2、加深理解

练习步骤：

1、创建一个抽象角色，比如咋们平时做的用户业务，抽象起来就是增删改查！

```java
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void select();
}
```

2、我们需要一个真实对象来完成这些增删改查操作

```java
public class UserServiceImpl implements UserService{
    @Override
    public void add() {
        System.out.println("增加一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除一个用户");
    }

    @Override
    public void update() {
        System.out.println("更新一个用户");
    }

    @Override
    public void select() {
        System.out.println("查找一个用户");
    }
}
```

3、需求来了，现在我们需要增加一个日志功能，怎么实现！

- 思路1 ：在实现类上增加代码 【麻烦！】
- 思路2：使用代理来做，能够不改变原来的业务情况下，实现此功能就是最好的了！

4、设置一个代理类来处理日志！代理角色

```java
public class UserServiceProxy implements UserService{
    private UserServiceImpl userService;

    public void setUserService(UserServiceImpl userService) {
        this.userService = userService;
    }

    @Override
    public void add() {
        log("add");
        userService.add();
    }

    @Override
    public void delete() {
        log("delete");
        userService.delete();
    }

    @Override
    public void update() {
        log("update");
        userService.update();
    }

    @Override
    public void select() {
        log("select");
        userService.select();
    }

    //日志方法
    public void log(String msg) {
        System.out.println("[debug] 使用了"+msg+"方法");
    }
}
```

5、测试访问类：

```java
public class Client {
    public static void main(String[] args) {
        //真实业务
        UserServiceImpl userService = new UserServiceImpl();
        //代理类
        UserServiceProxy proxy = new UserServiceProxy();
        //使用代理类实现日志功能！
        proxy.setUserService(userService);

        proxy.delete();
    }
}
```

OK，到了现在代理模式大家应该都没有什么问题了，重点大家需要理解其中的思想；

我们在不改变原来的代码的情况下，实现了对原有功能的增强，这是AOP中最核心的思想

聊聊AOP：纵向开发，横向开发

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170910953107314.png)



## 3、动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是我们直接写好的！
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - 基于接口---JDK动态代理
  - 基于类：cglib
  - java字节码实现：JAVAssist



需要了解两个类Proxy，InvocationHandler：调用处理程序

【InvocationHandler：调用处理程序】

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170913087024816.png)

```java
Object invoke(Object proxy, 方法 method, Object[] args)；
//参数
//proxy - 调用该方法的代理实例
//method -所述方法对应于调用代理实例上的接口方法的实例。方法对象的声明类将是该方法声明的接口，它可以是代理类继承该方法的代理接口的超级接口。
//args -包含的方法调用传递代理实例的参数值的对象的阵列，或null如果接口方法没有参数。原始类型的参数包含在适当的原始包装器类的实例中，例如java.lang.Integer或java.lang.Boolean 。

```

【Proxy : 代理】

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170913091730018.png)

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170913092410120.png)

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170913093561622.png)

```java
//生成代理类
public Object getProxy(){
   return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                                 rent.getClass().getInterfaces(),this);
}
```

**代码实现**

抽象角色和真实角色和之前的一样！

Rent . java 即抽象角色

```java
//抽象角色：租房
public interface Rent {
   public void rent();
}
```

Host . java 即真实角色

```java
//真实角色: 房东，房东要出租房子
public class Host implements Rent{
   public void rent() {
       System.out.println("房屋出租");
  }
}
```

ProxyInvocationHandler. java 即代理角色

```java
public class ProxyInvocationHandler implements InvocationHandler {
   private Rent rent;

   public void setRent(Rent rent) {
       this.rent = rent;
  }

   //生成代理类，重点是第二个参数，获取要代理的抽象角色！之前都是一个角色，现在可以代理一类角色
   public Object getProxy(){
       return Proxy.newProxyInstance(this.getClass().getClassLoader(),
               rent.getClass().getInterfaces(),this);
  }

   // proxy : 代理类 method : 代理类的调用处理程序的方法对象.
   // 处理代理实例上的方法调用并返回结果
   @Override
   public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
       seeHouse();
       //核心：本质利用反射实现！
       Object result = method.invoke(rent, args);
       fare();
       return result;
  }

   //看房
   public void seeHouse(){
       System.out.println("带房客看房");
  }
   //收中介费
   public void fare(){
       System.out.println("收中介费");
  }

}
```

Client . java

```java
//租客
public class Client {

   public static void main(String[] args) {
       //真实角色
       Host host = new Host();
       //代理实例的调用处理程序
       ProxyInvocationHandler pih = new ProxyInvocationHandler();
       pih.setRent(host); //将真实角色放置进去！
       Rent proxy = (Rent)pih.getProxy(); //动态生成对应的代理类！
       proxy.rent();
  }

}
```

核心：**一个动态代理 , 一般代理某一类业务 , 一个动态代理可以代理多个类，代理的是接口！**

## 4、深化理解

我们来使用动态代理实现代理我们后面写的UserService！

我们也可以编写一个通用的动态代理实现的类！所有的代理对象设置为Object即可！

```java
public class ProxyInvocationHandler implements InvocationHandler {
   private Object target;

   public void setTarget(Object target) {
       this.target = target;
  }

   //生成代理类
   public Object getProxy(){
       return Proxy.newProxyInstance(this.getClass().getClassLoader(),
               target.getClass().getInterfaces(),this);
  }

   // proxy : 代理类
   // method : 代理类的调用处理程序的方法对象.
   public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
       log(method.getName());
       Object result = method.invoke(target, args);
       return result;
  }

   public void log(String methodName){
       System.out.println("执行了"+methodName+"方法");
  }

}
```

测试！

```java
public class Test {
   public static void main(String[] args) {
       //真实对象
       UserServiceImpl userService = new UserServiceImpl();
       //代理对象的调用处理程序
       ProxyInvocationHandler pih = new ProxyInvocationHandler();
       pih.setTarget(userService); //设置要代理的对象
       UserService proxy = (UserService)pih.getProxy(); //动态生成代理类！
       proxy.delete();
  }
}
```



动态代理的好处：

- 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
- 公共也就交给代理角色！实现业务分工
- 公共业务发生扩展，方便集中管理
- 一个动态代理类代理的是一个接口，一般就是对应的一类业务
- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可

# 11、AOP

## 1、什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170919231825337.png)

## 2、AOP在Spring中的作用

提供声明式事务；允许用户自定义切面

以下名词需要了解下：

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 …
- 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个类。
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。
- 目标（Target）：被通知对象。
- 代理（Proxy）：向目标对象应用通知之后创建的对象。
- 切入点（PointCut）：切面通知 执行的 “地点”的定义。
- 连接点（JointPoint）：与切入点匹配的执行点。

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170919243959539.png)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:

![图片](D:\Studyspace\Markdown\Note\Spring.assets\format,png-170919247764441.png)

即 Aop 在 不改变原有代码的情况下 , 去增加新的功能 .

#### 3、使用Spring实现Aop

【重点】使用AOP织入，需要导入一个依赖包

```xml
<dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
```



方式一：使用Spring的API接口【主要SpringAPI接口实现】

方式二：自定义来实现AOP【主要是切面定义】

方式三：使用注解实现

# 12、整合MyBatis

步骤：

1. 导入相关jar包
   - junit
   - mybatis
   - mysql数据库
   - spring相关
   - aop织入
   - mybatis-spring【new】
2. 编写配置文件
3. 测试

## 1、回忆MyBatis

1、编写实体类

2、编写核心配置文件

3、编写接口

4、编写Mapper.xml

5、测试



## 2、Mybatis-spring

1、编写数据源配置

2、sqlSessionFactory

3、sqlSessionTemplate

4、需要给接口加实现类【】

5、将自己写的实现类注入到Spring中，

6、测试使用

# 13、声明式事务

## 1、回顾事务

- 把一组业务当成一个业务来做；要么都成功，要么都失败
- 事务在项目开发中，十分的重要，涉及到数据的一致性问题
- 确保完整性和一致性



## 2、spring中的事务管理

- 声明式事务：AOP
- 编程式事务：需要再代码中，进行事务的管理



思考：

为什么需要事务？

- 如果不配置事务，可能存在数据提交不一致的情况下；
- 如果我们不再Spring中配置声明式事务，就需要再代码中手动配置事务!
- 事务再项目的开发中十分重要，涉及到数据的一致性和完成性问题，不容马虎