# Java基础



## 初识Java

> 复杂应用三高问题：高可用、高性能、高并发 ；中间件将程序员从事务、安全、权限中解脱出来。

### JDK、JRE、JVM

- JDK: Java Development Kit
- JRE: Java Runtime Environment
- JVM: Java Virtual Machine(write once, run anywhere. 不同操作系统下，模拟CPU，处理Java程序。虚拟机屏蔽了底层差别)



### Java程序运行机制

- 编译型：一开始就翻译给他，更新了要重新编译

  操作系统 C、C++……

- 解释型：读什么，解释什么（从头，又要重新翻译）

  网页（速度要求不高）Java

  Java两者都有

程序运行机制：源程序（\*.java文件)→<font color=DarkSeaGreen>Java编译器</font>→字节码(\*.class文件)→<font color=DarkSeaGreen>类装载器</font>→<font color=DarkSeaGreen>字节码校验器</font>→<font color=DarkSeaGreen>解释器</font>→<font color=Green>操作系统平台</font>



<font color = red>*注意点：*</font>*Java文件名 和 类名必须一致，且首字母大写*

### Java基础语法

Java所有组成部分都需要名字。类名、变量名以及方法名都被称为标识符。

强类型语言：所有变量使用要严格符合规范，变量一定要先定义再使用



#### 基本数据类型

boolean 占一位

long类型要在数字后面加L

float类型要在数字后面加F

String不是关键字，类



#### 类型转换

低----------------------------------->高

byte，short，char，int，long，float，double

运算中，不同类型的数据先转换为同一类型，然后进行运算



#### 变量

变量作用域：JavaSE\Demo04

#### 常量

初始化（initialize）后不能再改变的值！不会变动的值。

```java
fianl 常量名=值;
fianl double PI=3.14;
```

常量名一般用大写



#### 变量命名规范

- 所有变量、方法、类名：<font color = "red">见名如意</font>
- 类成员变量：首字母小写和驼峰原则：monthSalary 除了第一个单词以外，后面的单词首字母大写 lastName
- 局部变量：首字母小写和驼峰原则
- 常量：大写字母和下划线：MAX_VALUE
- 类名：首字母大写和驼峰原则：Man，GoodMan
- 方法名：首字母小写和驼峰原则：run()，runRun()



#### 运算符

##### +-*/

short，byte算数运算（加减乘除）后为int，算数运算里有long则结果为long，有double则double，有float则float，自动向上转换。



#### JavaDoc

命令行：javadoc 参数 Java文件

javadoc -encoding UTF-8 -charset UTF-8 Java文件

idea生成JavaDoc文档：



### 流程控制

#### Scanner对象

用Scanner类来获取用户的输入

基本语法：

```java
Scanner s = new Scanner(System.in);
```

通过Scanner类的next()与nextLine()方法获取输入的字符串，在读取前我们一般需要使用 hasNext()与hasNextLine()判断是否还有输入数据。

**next():**

1. 一定要读到有效字符后才可以结束输入。
2. 对输入有效字符之前输入的空格，next()方法会自动将其去掉。
3. 只有输入有效字符后才将其后面输入的空白作为分隔符或结束符。
4. <font color=red>next()不能得到带有空格的字符串。</font>



**nextLine():**

1. 以Enter为结束符，也就是说nextLine()方法返回的是输入回车之前的所有字符。
2. 可以获得空白。



#### switch多选择结构

switch匹配一个具体的值，不加break会有case穿透

switch语句中的变量类型可以是：

- byte、short、int或char
- <font color = red>从Java SE 7开始 支持String类型</font>
- 同时case便签必须为字符串常量或字面量



#### goto

Java用<font color=red>Label:</font>代替goto，用在break，continue后面。用来跳转到指定地方。



### 方法

方法设计原则：原子性，一个方法只完成一个功能，便于后期扩展

值传递（Java）和引用传递

#### 方法重载

一个类中，有相同的函数名称，但形参不同的函数。

- 方法名必须相同
- 参数列表必须不同（个数不同、或类型不同、参数排列顺序不同等）
- 方法返回类型可以相同也可以不相同
- 仅仅返回类型不同不足以成为方法的重载。



#### 命令行参数

```sh
G:\StudySpace\JavaSE\BasicGrammar\src\com\landon\method>javac Demo01.java

G:\StudySpace\JavaSE\BasicGrammar\src\com\landon\method>cd ../../../

G:\StudySpace\JavaSE\BasicGrammar\src>java com.landon.method.Demo01 this is landon
args[0]: this
args[1]: is
args[2]: landon
```

#### 可变参数

JDK1.5开始支持船体同类型的可变参数给一个方法 

在方法申明中，在指定参数类型后加一个省略号(...)。

一个方法中只能指定一个可变参数，它必须是方法的最后一个参数。任何普通参数必须在它之前申明。



#### 递归

结构包括两部分：

- 递归头：是时候不调用自身方法。若果没有头，将陷入死循环
- 递归体：什么时候需要调用自己方法。



### 数组

数组声明

```java
dataType[] arrayRefVar = new dataType[arraySiz]
```



#### Java内存分析

堆：

- 存放new的对象和数组
- 可以被所有线程共享，不会存放别的对象引用

栈：

- 存放基本变量类型（会包含这个基本变量的具体数值）
- 引用对象的变量（会存放这个引用在堆里面的具体地址）

方法区：

- 可以被所有线程共享
- 包含了所有class和static变量



数组的默认初始化：

数组是引用类型，他的元素相当于类的实例变量，因此数组一经分配空间，其中的每个元素也被按照实例变量同样的方式被隐式初始化。



#### 数组的四个基本特点

- 其长度是确定的，数组一旦被创建，它的大小就是不可以被改变的
- 其元素必须是相同类型，不允许出现混合类型
- 数组中的元素可以使任何数据类型，包括基本类型和引用类型
- 数组变量属于引用类型，数组也可以看成是对象，数组中的每个元素相当于该对象的成员变量。数组本身就是对象，Java中对象是在堆中的，因此数组无论保存原始类型还是其他对象类型，<font color=red>数组对象本身是在堆中的</font>

#### 数组小结

- 数组是相同数据类型（数据类型可以为任意类型）的有序集合
- 数组也是对象。数组元素相当于对象的成员变量
- 数组长度是确定的，不可变的。如果越界，报<font color=red>ArrayIndexOutofBounds。</font>



### 面向对象（OOP）

面向对象编程的本质就是：<font color=red>以类的方式组织代码，以对象的形式组织（封装）数据</font>

三大特性：封装、继承、多态

static 静态，和类一起加载，无需实例化

#### 创建与初始化对象

使用new关键字创建对象

使用new关键字创建的时候，除了分配内存空间之外，还会给创建好的对象进行默认的初始化以及对类中构造器的调用。

类中的构造器也称为构造方法，是在进行创建对象的时候必须要调用的。并且构造器有以下俩个特点:

1. 必须和类的名字相同
2. 必须没有返回类型,也不能写void

<font color=red>构造器必须要掌握</font>

一旦定义了有参构造，无参构造必须显示定义。

1. 使用new关键字，本质是在调用构造器
2. 构造器用来初始化值

#### 封装

该露的露，该藏的藏：

​	我们程序设计要追求“<font color=red>高内聚，低耦合</font>”。高内聚就是类的内部数据操作细节自己完成，不允许外部干涉;低耦合:仅暴露少量的方法给外部使用。

封装(数据的隐藏)：

​	通常，应禁止直接访问一个对象中数据的实际表示，而应通过操作接口来访问，这称为信息隐藏。

记住这句话就够了:<font color=red>属性私有,get/set</font>

意义：

1. 提高程序的安全性，保护数据。
2. 隐藏代码的实现细节。
3. 统一接口。
4. 提高系统的可维护性。



#### 继承

继承的本质是对某一批类的抽象，从而实现对现实世界更好的建模。

extends的意思是“扩展”。子类是父类的扩展。

JAVA中类只有单继承，没有多继承!

继承是类和类之间的一种关系。除此之外,类和类之间的关系还有依赖、组合、聚合等。

继承关系的俩个类，一个为子类(派生类),一个为父类(基类)。子类继承父类,使用关键字extends来表示。子类和父类之间.从意义上讲应该具有"is a"的关系.

Java中所有类，都默认直接或间接继承Object

##### super注意点：

1. super调用父类的构造方法，必须在构造方法的第一个
2. super必须只能处在子类的方法或构造方法中
3. super 和 this 不能同时调用构造方法！

VS this：

代表的对象不同：

​	this：本身调用者这个对象

​	super：父类对象的引用

前提：

​	this：没有继承也可以使用

​	super：只能在继承条件下使用

构造方法：

​	this(): 本类的构造

​	super(): 父类的构造



##### 重写

需要有继承关系，子类重写父类的方法！

1. 方法名必须相同
2. 参数列表必须相同
3. 修饰符：范围可以扩大：public>protected>default>private
4. 抛出的异常：范围，可以被缩小，但不能扩大：ClassNotFoundException--->Exception(大)

重写，子类的方法和父类必须要一致：方法体不同！

为什么要重写：

1. 父类的功能，子类不一定需要，或者不一定满足！

子类重写父类方法，父类不一定要public，只要子类重写方法的权限修饰符不比父类更严格就行。

重写的方法返回类型可以改变，但返回类型必须是父类的子类型

#### 多态

动态编译

即同一方法可以根据发送对象的不同而采用多种不同的行为方式。

一个对象的实际类型是确定的，但可以指向对象的引用的类型有很多(父类，有关系的类)

多态注意事项： 

1. 多态是方法的多态，属性没有多态
2. 父类和子类，有联系， 类型转换异常 ClassCastException!
3. 存在的条件：继承关系，方法需要重写，父类引用指向子对象Father f1 = new Son( );

不能重写：

1. Static 方法，属于类，不属于实例
2. final 常量
3. private方法



#### 类型转换

1. 父类引用指向子类对象
2. 把子类转换为父类，向上转型；（会丢失自身独有的方法）
3. 把父类转换为子类，向下转型；强制转换
4. 方便方法的调用，减少重复的代码！

子转父: 向上转型, 直接转, 丢失子类中原本可直接调用的特有方法；  父转子: 向下转型, 强制转, 丢失父类被子类所重写掉的方法。

**一般需要使用子类独有的方法才会向下转型**

抽象：封装、继承、多态



#### 抽象类

1. 不能new 这个抽象类，只能靠子类去实现它；约束！
2. 抽象类中可写些普通方法（可以没有抽象方法）
3. 抽象方法必须在抽象类中

子类继承抽象类，就必须实现抽象类中没有实现的抽象方法，否则该子类也要声明为抽象类。

抽象的抽象

抽象类存在构造器嘛？

抽象类存在构造器。

存在的意义？

抽象出来~ 提高开发效率 可扩展性

#### 接口

- 普通类:只有具体实现
- 抽象类:具体实现和规范(抽象方法)都有!
- 接口:只有规范!自己无法写方法~专业的约束!约束和实现分离:面向接口编程~
- 接口就是规范，定义的是一组规则，体现了现实世界中“如果你是.….则必须能...”的思想。如果你是天使，则必须能飞。如果你是汽车，则必须能跑。如果你好人，则必须千掉坏人;如果你是坏人，则必须欺负好人。
- 接口的本质是契约，就像我们人间的法律一样。制定好后大家都遵守。
- OO的精髓，是对对象的抽象，最能体现这一点的就是接口。为什么我们讨论设计模式都只针对具备了抽象能力的语言（比如c++、java、c#等)，就是因为设计模式所研究的，实际上就是如何合理的去抽象。

##### 作用

1. 约束
2. 定义一些方法，让不同的人实现~ 10 --->  1
3. 方法都是 public abstract
4. 变量都是 public static final
5. 接口不能被实例化~，接口中没有构造方法
6. implements可以实现多个接口
7. 必须要重写接口中的方法 



#### 内部类

内部类就是在一个类的内部在定义一个类，比如，A类中定义一个B类，那么B类相对A类来说就称为内部类，而A类相对B类来说就是外部类了。

1. 成员内部类

2. 静态内部类

3. 局部内部类

4. 匿名内部类



### 异常

自定义异常：

使用Java内置的异常类可以描述在编程时出现的大部分异常情况。除此之外，用户还可以自定义异常。用户自定义异常类，只需继承Exception类即可。
在程序中使用自定义异常类,大体可分为以下几个步骤:

1. 创建自定义异常类。
2. 在方法中通过throw关键字抛出异常对象。
3. 如果在当前抛出异常的方法中处理异常,可以使用try-catch语句捕获并处理;否则在方法的声明处通过throws关键字指明要抛出给方法调用者的异常，继续进行下一步操作。
4. 在出现异常方法的调用者中捕获并处理异常。





实际应用中：

- 处理运行时异常时，采用逻辑去合理规避同时辅助try-catch处理
- 在多重catch块后面，可以加一个catch (Exception）来处理可能会被遗漏的异常
- 对于不确定的代码，也可以加上try-catch，处理潜在的异常
- 尽量去处理异常，切忌只是简单地调用printStackTrace()去打印输出
- 具体如何处理异常，要根据不同的业务需求和异常类型去决定
- 尽量添加finally语句块去释放占用的资源





## 多线程

### 继承Thread类

1. 自定义线程类继承Thread类
2. 重写run()方法，编写线程执行体
3. 创建线程对象，调用start()方法启动线程

线程不一定立即执行，CPU安排调度

### 实现Runnable方法

1. 定义MyRunnable类实现Runnable接口
2. 实现run()方法，编写线程执行体
3. 创建线程对象，（丢入Thread对象）调用start()方法启动线程

继承Thread类

- 子类继承Thread类具备多线程能力
- 启动线程:子类对象. start()
- 不建议使用:避免OOP单继承局限性

实现Runnable接口

- 实现接口Runnable具有多线程能力
- 启动线程:传入目标对象+Thread对象.start()
- 推荐使用:避免单继承局限性，灵活方便，方便同一个对象被多个线程使用



### 实现Callable接口

1. 实现Callable接口，需要返回值类型
2. 重写call方法，需要抛出异常
3. 创建目标对象
4. 创建执行服务:ExecutorService ser = Executors.newFixedThreadPool(1);
5. 提交执行:Future<Boolean> result1 = ser.submit(t1);
6. 获取结果: boolean r1 = result1.get()
7. 关闭服务: ser.shutdownNow();

1.可以定义返回值
2.可以抛出异常



### Lambda表达式

为什么要用Lambda表达式

- 避免匿名内部类定义过多
- 可以让你的代码看起来很简洁
- 去掉了一堆没有意义的代码，只留下核心逻辑



#### 函数式接口

任何接口，如果只包含唯一一个抽象方法，那么他就是函数式接口

```java
public interface Runnable {
    public abstract void run();
}
```

对于函数式接口，我们可以通过lambda表达式来创建该接口的对象



### 线程状态

1、新生状态：new Thread();

2、就绪状态：调用start();

3、运行状态：调度

4、阻塞状态：sleep，wait，同步锁，阻塞借出，重回就绪

5、死亡状态：线程中断或结束，一旦死亡不能再次启动

Thread.State

NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING,  TERMINATED



#### 线程方法

 方法                           | 说明                       
 ------------------------------ | -------------------------- 
 setPriority(int newPriority)   | 更改优先级                 
 static void sleep(long millis) | 线程休眠毫秒               
 void join()                    | 等待该线程终止             
 static void yield()            | 暂停当前线程，执行其他线程 
 void interrupt()               | 中断线程（别用）           
 boolean isAlive()              | 是否处于活动状态           

#### 停止线程

不推荐JDK提供的stop()、destroy()方法

<font color='red'>推荐线程自己停止</font>

建议使用一个标志位进行终止，当flag = false，则线程终止

#### 线程休眠

sleep()

模拟延时，倒计时

<font color='red'>每个对象都有一个锁，sleep不会释放锁</font>

#### 线程礼让

yield()

线程暂停，但不阻塞；运行态变为就绪态；礼让不一定成功，看CPU调度

#### join()

join合并线程，待此线程执行完，再执行其他线程，其他线程阻塞（可以想象成插队）

#### 线程优先级

优先级的设定建议在start()调度之前

优先级低只意味着获得调度的概率低，并不是优先级低就不会被调度，还是看CPU调度

性能倒置（低的线程被先执行）

#### 守护（daemon）线程

线程分为用户线程和守护线程

虚拟机必须确保用户线程执行完毕 main()

虚拟机不用等待守护线程执行完毕 gc

如，后台记录操作日志，监控内存，垃圾回收等待



### 线程同步

线程有自己的工作内存，互不影响。

线程同步其实就是一种等待机制，多个需要同事访问此对象的线程进入这个对象的等待池形成队列，前面线程用完，后面再用。形成条件：<font color='red'>队列+锁</font>

synchronized

在访问时加入锁机制synchronized，当一个线程获得对象的排它锁，独占资源，其他线程必须等待，使用后释放锁即可。存在以下问题:

一个线程持有锁会导致其他所有需要此锁的线程挂起;
在多线程竞争下,加锁,释放锁会导致比较多的上下文切换和调度延时,引起性能问题;
如果一个优先级高的线程等待一个优先级低的线程释放锁会导致优先级倒置﹐引起性能问题．

synchronized默认锁this

同步方法：public synchronized void run(){}

同步块：synchronized (obj) {}锁需要增删改的对象

### 死锁

多个线程互相抱着对方需要的资源，然后形成僵持.

#### 产生死锁的四个必要条件:

1. 互斥条件:一个资源每次只能被一个进程使用。

2. 请求与保持条件:一个进程因请求资源而阻塞时，对已获得的资源保持不放。

3. 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。

### synchronized 与Lock的对比

1. Lock是显式锁（手动开启和关闭锁，别忘记关闭锁)synchronized是隐式锁，出了作用域自动释放

2. Lock只有代码块锁，synchronized有代码块锁和方法锁
3. 使用Lock锁，JVM将花费较少的时间来调度线程，性能更好。并且具有更好的扩展性(提供更多的子类)
4. 优先使用顺序:
   Lock >同步代码块（已经进入了方法体，分配了相应资源)>同步方法（在方法体之外)

### 线程通信

方法名|作用
--|--
wait()|线程等待，直到其他线程通知，与sleep不同，会释放锁
wait(long time)|制定等待毫秒数
notify()|唤醒一个处于等待的线程
notifyAll()|唤醒同一个对象上所有调用wait()方法的线程，优先级别搞得线程优先调度

### 使用线程池

- 背景:经常创建和销毁、使用量特别大的资源，比如并发情况下的线程，对性能影响很大。
- 思路:提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用。类似生活中的公共交通工具。
- 好处:
  - 提高响应速度（减少了创建新线程的时间)
  - 降低资源消耗（重复利用线程池中线程，不需要每次都创建)
  - 便于线程管理(....)
    - corePoolSize:核心池的大小
    - maximumPoolSize:最大线程数
    - keepAliveTime:线程没有任务时最多保持多长时间后会终止

- JDK 5.0起提供了线程池相关API: ExecutorService和Executors
- ExecutorService:真正的线程池接口。常见子类ThreadPoolExecutor
  - void execute(Runnable command)∶执行任务/命令，没有返回值，一般用来执行Runnable
  - <T> Future<T> submit(Callable<T> task):执行任务，有返回值，一般又来执行Callable
  - void shutdown()∶关闭连接池
- Executors:工具类、线程池的工厂类，用于创建并返回不同类型的线程池



## 注解

### 内置注解

@Override

@Deprecated

@SuppressWarnings

### 元注解（meta-annotation)

注解其他注解

@Target：注解适用范围

@Retention：什么级别保存注解，描述注解生命周期（SOURCE<CLASS<RUNTIME）

@Document：注解包含在javadoc

@Inherited：子类可继承父类注解



## 反射

Class类

由系统创建，一个加载的类在jvm中只有一个Class实例，一个Class对象对应一个加载到JVM中的一个Class文件，每个类的实例会记得是有那个Class实例生成的，通过Class可以完整获得一个类中所有被加载的结构



### Java内存

堆：存放new的对象和数组

​		可以被所有线程共享，不会存放别的对象的引用

栈：存放基本变量类型（包含这个基本类型的数值）

​		引用对象的变量（存放这个引用在堆里面的具体地址）

方法区（特殊的堆）：可以被所有线程共享

​		包含所有的class和static变量



![image-20240228235144756](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240228235144756-170913550638024.png)

![image-20240229000957026](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229000957026-170913659825225.png)



![image-20240229101431193](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229101431193-170917287267126.png)

![image-20240229101832076](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229101832076-170917311327327.png)



![image-20240229102353990](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229102353990-170917343518428.png)

![image-20240229102452165](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229102452165-170917349362929.png)

![image-20240229103049504](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229103049504-170917385218330.png)

![image-20240229103339415](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229103339415-170917402151731.png)

![image-20240229103528516](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229103528516-170917413066332.png)

![image-20240229105528302](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229105528302-170917532948533.png)

![image-20240229105626791](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229105626791-170917538789834.png)

![image-20240229105734851](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229105734851-170917545595035.png)

![image-20240229105843315](D:\Studyspace\Markdown\Note\Java基础.assets\image-20240229105843315-170917552455436.png)
