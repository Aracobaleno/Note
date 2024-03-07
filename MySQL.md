

# MySQL

## 1、MySQL

连接数据库

```sql
mysql -u root -p123456
update mysql.user set authentication_string=password('123456') where user = 'root' and Host = 'localhost'; --修改用户密码
flush privilege; --刷新权限

------------------------------------------
--所有语句；结尾
show databases;

use school  --切换数据

show tables; --查看数据库中所有表
describe student; --显示数表的所有信息

create database landon; --建表
exit; --退出连接

-- 单行注释（SQL本来注释）
/* 多行注释
*/
```

**数据库XX语言** crud增删改查！

DDL 定义

DML 操作

DQL 查询

DCL 控制



## 2、操作数据库

操作数据库>表>表中数据

MySQL关键字不区分大小写

### 2.1操作数据库

1、创建数据库

```sql
CREATE DATABASE [IF NOT EXISTS] westos
```

2、删除

```sql
DROP DATABASE [IF EXISTS] westos
```

3、使用

```SQL
--TAB健上面你，如果表明或者字段名是特殊字符，就需要带··
USE `school`
```

4、查看

```SQL
SHOW DATABASES --查看所有数据库
```

### 2.2数据类型

> 数值

- tinyint  1byte
- small   2byte
- mediumint  3byte
- **int      4byte**  **常用**
- bigint  8byte
- float  4byte
- double  8byte
- decimal 字符串形式的浮点数    

> 字符串

- char 字符串固定大小 0-255byte
- **varchar 可变字符串  0-65535  常用    string**
- tinytext 微文本
- **text   文本串  2^8-1  保存大文本**

> 时间日期

java.util.Date

- date  YYYY-MM-DD  日期
- time  HH:mm:ss  时间格式
- **datetime  YYYY-MM-DD HH:mm:ss 最常用的时间格式**
- **timestamp 时间戳 1970.1.1到现在毫秒数！**
- year 年份表示

> null

- 没有值，未知
- <font color='red'>注意，不要用NULL进行运算，结果为NULL</font>



### 2.3、数据库的字段属性（重点）

Unsigned：

- 无符号整数
- 声明了该列不能声明为负数

zerofill：

- 0填充
- 不足位数，0填充 int(3) , 5 … 005

自增：

- 自动在上一条记录的基础上+1（默认）
- 通常用来设计唯一主键~index，必须是整数类型
- 可以自定义主键自增的起始值和步长

非空 null not null

- 设置not null，如果不赋值，报错
- NULL，不写值，默认NULL！

default：

- 设置默认值
- sex，默认男，如果不指定值，会有默认值

拓展：

```sql
/* 每个表必须有以下五个字段！做项目用，表示一个记录存在的意义
id 主键
`VERSION` 乐观锁
is_delete 伪删除
gmt_create 创建时间
gmt_update 修改时间
*/
```

### 2.4、建表

```sql
CREATE TABLE IF NOT EXISTS `student`(
	`id` INT(4) NOT NULL AUTO_INCREMENT COMMNET '学号',
    `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
    `pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
    `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
    `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
    `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
    `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
```



格式

```sql
CREATE TABLE [IF NOT EXISTS] `表名`(
	`字段名` 列类型 [属性] [索引] [注释],
    `字段名` 列类型 [属性] [索引] [注释],
    ...
    `字段名` 列类型 [属性] [索引] [注释]
)[表类型][字符集][注释]
```



查看建库、建表语句

```sql
SHOW CREATE DATABASE school
SHOW CREATE TABLE student
DESC student -- 显示表的结构
```



### 2.5、数据库引擎

```sql
-- 关于数据库引擎
/*
INNODB 默认使用
MYISAM 早些年使用
*/
```

|            | MYISAM | INNODB        |
| ---------- | ------ | ------------- |
| 事务支持   | 不支持 | 支持          |
| 数据行锁定 | 不支持 | 支持          |
| 外键约束   | 不支持 | 支持          |
| 全文索引   | 支持   | 不支持        |
| 表空间大小 | 较小   | 较大，约为2倍 |

常规使用操作

- MYISAM 节约空间，速度较快

- INNODB 安全性高，事务处理，多表多用户操作

> 在物理空间存在的位置

所有的数据库文件都存在data目录下，一个文件夹对应一个数据库

本质还是文件的存储！



MySQL引擎在物理文件上的区别

- InnoDB 在数据库表中只有一个*.frm文件，以及上级目录下的ibdata1文件
- MyISAM 对应文件
  - *.frm - 表结构定义文件
  - *.MYD 数据文件（data）
  - *.MYI  索引文件  （index）

> 设置数据表的字符集编码

```sql
CHARSET = utf8
```

不设置，会是MySQL默认字符集编码（不支持中文）

默认编码Latin1

在my.ini中配置默认编码

```sql
character-set-server=utf8
```

### 2.6、修改删除表

> 修改

```sql
-- 修改表名： ALTER TABLE 表名 RENAME AS 新表名
ALTER TABLE teacher RENAME AS teacher1
-- 增加字段：ALTER TABLE 表名 ADD 字段名 列属性
ALTER TABLE teacher1 ADD age INT(11)

-- 修改表字段（重命名，修改约束）
-- ALTER TABLE 表名 MODIFY 字段名 列属性[]
ALTER TABLE teacher1 MODIFY age VARCHAR(11) --修改约束
-- ALTER TABLE 表名 CHANGE 旧名 新名 列属性[]
ALTER TABLE teacher1 CHANGE age age1 INT(1) --字段重命名

-- 删除表的字段： ALTER TABLE 表名 DROP 字段名
ALTER TABLE teacher1 DROP age1
```



> 删除

```sql
-- 删除表（如果存在再删除）
DROP TABLE IF EXSITS teacher
```



注意点：

- ``字段名，用这个包裹
- 注释 -- /**/
- sql关键字大小写不敏感

## 3、MySQL数据管理

### 3.1、外键

是数据库中用于建立两个表之间关系的一种约束。如果一个实体的某个字段指向另一个实体的主键，则该字段被称为外键。外键表示了两个表之间的相关联系，并确保数据的一致性和完整性。

<font color='red'>**外键并不一定是另一张表的主键，但一定是唯一性索引；主键约束和唯一性约束都是唯一性索引**</font>

```sql
CREATE TABLE `grade`(
	`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
    `gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
    PRIMARY KEY (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

-- 学生表的gradeid字段 要去引用年级表的gradeid
-- 定义外键key
-- 给这个外键添加约束（执行引用） references 引用
CREATE TABLE IF NOT EXISTS `student`(
    `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
    `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
    `pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
    `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
    `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
    `gradeid` INT(10) NOT NULL COMMENT '学生年级'
    `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
    `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY(`id`),
    KEY `FK_gradeid` (`gradeid`),
    CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`(`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
```



删除有外键关系的表，必须先删除引用别的表（从表），再删除被引用的表（主表）



>方式二：建表后，添加外键约束

```sql
CREATE TABLE `grade`(
	`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
    `gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
    PRIMARY KEY (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
-- 学生表的gradeid字段 要去引用年级表的gradeid
-- 定义外键key
-- 给这个外键添加约束（执行引用） references 引用
CREATE TABLE IF NOT EXISTS `student`(
    `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
    `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
    `pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
    `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
    `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
    `gradeid` INT(10) NOT NULL COMMENT '学生年级',
    `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
    `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

-- 创建表的时候没有外键关系
ALTER TABLE `student`
ADD CONSTRAINT 'FK_gradeid' FOREIGN KEY(`gradeid`) REFERENCES `grade`(`gradeid`)
--ALTER TABLE 表名 ADD CONSTRAINT 约束名 FOREIGN KEY(作为外键的列) REFERENCES 哪个表(哪个字段)
```

以上的操作都是物理外键，数据库级别的外键，不建议使用（避免数据库过多造成困扰）

最佳实践

- 数据库就是单纯的表，只用来存数据，只有行（数据）和列（字段）
- 我们想使用多张表的数据，想使用外键（程序去实现）

### 3.2、DML语言

数据库：存储，管理数据

DML语言：数据操作语言

- insert
- update
- delete

### 3.3、添加

> insert

```sql
-- 插入语句（添加）
-- INSERT INTO 表名([字段名1],[字段名2],[字段名3]) VALUES(值1,值2,值3)
INSERT INTO `grade`(`gradename`) VALUES('大四')

-- 由于主键自增我们可以省略（如果不写字段，它就会一一匹配）
INSERT INTO `grade` VALUES('大三')

-- 一般写插入语句，数据和字段一定要一一对应

-- 插入多个字段
INSERT INTO `grade`(`gradename`)
VALUES('大一'),('大二')

INSERT INTO `student`(`name`) VALUES ('张三')

INSERT INTO `student`(`name`,`pwd`,`sex`) VALUES ('张三','AAAAAA','男')

INSERT INTO `student`(`name`,`pwd`,`sex`) VALUES ('李四','aaaaaa','男'),('王五','aaaaaa','男')
```

语法：INSERT INTO 表名([字段名1],[字段名2],[字段名3]) VALUES(值1,值2,值3)

注意事项：

​	1、字段可以省略，后面的值要一一对应

​	2、可以插入多条数据，VALUES后面的值，用 , 隔开VALUES(),()

### 3.4、修改

> update 修改谁 （条件） set原来的值=新值

```sql
-- 修改学员名字，带条件
UPDATE `student` SET `name` = 'Landon' WHERE `id` = 1

-- 不带条件 会改动所有表项
UPDATE `student` SET `name` = 'Landon'

-- 修改多个属性，逗号隔开
UPDATE `student` SET `name` = 'Landon',`email` = '123456@qq.com' WHERE `id` = 1

-- 语法
-- UPDATE 表名 SET column = value,[] WHERE [条件]
```

语法：UPDATE 表名 SET column = value,[column = value] WHERE [条件]

注意：

- 没指定筛选条件，会修改所有列

- value，可以具体值，也可以是一个变量

```sql
UPDATE `student` SET `birthday` = CURRENT_TIME WHERE `id` = 1
```



### 3.5、删除

> DELETE

语法：DELETE FROM 表名 [WHERE 条件]

```SQL
-- 删除数据（避免这样，全部删除）
DELETE FROM `student`

-- 删除指定数据
DELETE FROM `student` WHERE id = 1
```



> TRUNCATE      -- DDL 不能回滚

TRUNCATE 表名

作用：完全清空一个表，表结构和索引约束不会变！

```sql
-- 清空student表
TRUNCATE `student`
```



> DELETE 和 TRUNCATE 的区别

- 相同点：都能删除数据，不会删除表结构
- 不同点：
  - TRUNCATE 重新设置自增列 计数器会归零
  - TRUNCATE 不会影响事务

```sql
-- 测试DELETE 和 TRUNCATE 的区别
CREATE TABLE test(
	`id` INT(4) NOT NULL AUTO_INCREMENT,
    `col` VARCHAR(20) NOT NULL,
    PRIMARY KEY (`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO `test`(`col`) VALUES(`1`),(`2`),(`3`)

DELETE FROM `test` -- 不会影响自增

TRUNCATE TABLE `test`  -- 自增会归零
```

DELETE删除问题。重启数据库，现象

- InnoDB 自增列会从1开始（存在内存中，断电及失）
- MyISAM 继续从上一个自增量开始（存在文件中，不会丢失）



## 4、DQL查询数据（最重点）

### 4.1、DQL

(date query language: 数据查询语言)

- 数据库中最核心的语言，使用频率最高

```SQL
SELECT [ALL | DISTINCT]
{* | TABLE.* |[TABLE.FIELD1[AS ALIAS1][,TABLE.FIELD2[AS ALIAS2]][,...]]}
FROM TABLE_NAME [AS TABLE_ALIAS]
	[LEFT|RIGHT|INNER JOIN TABLE_NAME2]
	[WHERE ...]
	[GROUP BY ...]
	[HAVING ...] -- 过滤分组后的条件
	[ORDER BY ...]
	[LIMIT {[OFFSET,]ROW_COUNT|ROW_COUNTOFFSET OFFSET}]
```



### 4.2、查询指定字段

```sql
-- 函数
CONCAT(a,b) --拼接字符串
```



> 去重 distinct

去除 select 查询出来数据中重复数据



> 数据库的列（表达式）

```sql
SELECT VERSION()
SELECT 100*3-1 AS 计算结果
SELECT @@auto_increment_increment  --查询自增步长（变量）

-- 成绩 +1分查看
SELECT StudentNo, StudentResult+1 as 提分后 from result
```

数据库中表达式：文本值，列，NULL，函数，计算表达式，系统变量……

select 表达式 from 表

### 4.3、where条件子句

搜索条件由一个或多个表达式组成！结果为布尔值

> 逻辑运算符 AND  OR  NOT

- AND &&

- OR  ||
- NOT  !



> 模糊查询：比较运算符

```sql
IS NULL
IS NOT NULL
BETWEEN AND
LIKE  --SQL匹配 % _
in
```



### 4.4、联表查询

join on 连接查询

where 等值查询



> 自连接

核心：一张表拆为两张一样的表

```sql
select c1.categoryname, c2.categoryname
FROM category c1
JOIN category C2
ON c1.categoryid = c2.pid


select c1.categoryname, c2.categoryname
FROM category c1
,category C2
WHERE c1.categoryid = c2.pid
```



### 4.5、分页和排序

> 排序 order by

升序 ASC

降序 DESC

> 分页 limit

缓解数据库压力，更好体验    瀑布流（无限往下刷）

语法：limit 起始值，页面大小

网页应用：当前，总的页数，页面大小

limit 0, 5   1~5

limit 1, 5   2~6

第N页  limit  (n-1)pagesize*pagesize  pagesize

### 4.6、分组和过滤

### 4.7、子查询

在where语句中嵌套一个子查询



## 5、MySQL函数

### 5.1、常用函数

```sql
select abs(-8)
select ceiling(9.4) -- 向上取整
select floor(9.4) -- 向下取整 
select rand() -- 0~1随机数
select sign(10) -- 判断正负

-- 字符串函数
select char_length()
select concat --拼接字符串
select insert()  -- 从某个位置替换某个长度
select lower() -- 小写
select upper() -- 大写
select instr() -- 返回第一次出现子串的位置
select replace() -- 替换
select substr() -- 返回指定的字符串（string,pos,len）
reverse() -- 反转

-- 时间、日期
select current_date() --当前日期
curdate()
now() -- 当前时间
localtime() -- 本地时间
sysdate() -- 系统时间

year(now())
month(now())
day(now())
minute(now())
second(now())

-- 系统
select system_user()
select user()
select version()
```

### 5.2、聚合函数(常用)

函数名|描述
---|---
count()|计数
sum()|求和
avg()|
max()|
min()|

<font color= 'red'>count(col), count(*), count(1)区别：</font>

count(col),会忽略所有null值

*，1不会忽略null值，本质计算行数

### 5.3、数据库级别的MD5加密

增强复杂性和不可逆性

不可逆，具体值MD5一样

MD5破解网站原理，存了一个字典，加密后的值，加密前的值

md5()



## 事务

### 6.1、事务

要么都能成功，要么都失败

---

1、SQL执行 A给B转账 A 1000 --->200  B 200

2、SQL执行 B收到A的钱  A 800 ---> B 400

---

将一组SQL放在一个批次中去执行

> 事物原则：ACID  	隔离性---（脏读，不可重复读，幻读）

参考链接：https://www.cnblogs.com/wiseleer/p/16898444.html

**原子性（ Atomicity ）**

要么都成功，要么都失败

**一致性（ Consistency ）**

事务前后的数据完整性要保证一致，1000

**持久性（ Durability ）**

事务一旦提交则不可逆，被持久化到数据库中！

**隔离性（Isolation）**

事务的隔离性是多个用户并发访问数据库，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离

> 隔离所导致的问题

**脏读:**
指一个事务读取了另外一个事务未提交的数据。

**不可重复读:**
在一个事务内读取表中的某一行数据，多次读取结果不同。(这个不一定是错误，只是某些场合不对)

**虚读(幻读)**
是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致。



```sql
-- mysql 默认开始事务自动提交的
SET autocommit = 0  /*关闭*/
SET autocommit = 1  /*开启（默认的）*/
-- 手动处理事务
-- 事务开启
START TRANSACTION -- 标记一个事务的开始，在这个时候的sql都在同一个事务内

INSERT xx
INSERT xx

-- 提交：持久化（成功）
COMMIT
-- 回滚：回到原来的样子（失败）
ROLLBACK

-- 事务结束
SET autocommit = 1   -- 开启自动提交

-- 了解
SAVEPOINT 保存点名 -- 设置一个事务的保存点
ROLLBACK TO SAVEPOINT 保存点名  -- 回滚到保存点
RELEASE SAVEPOINT 保存点名  -- 撤销保存点
```

模拟事务

```sql
CREATE DATABASE BANK CHARACTER SET UTF8 COLLATE UTF8_GENERAL_CI

USE BANK;

CREATE TABLE ACCOUNT(
	ID INT(3) NOT NULL AUTO_INCREMENT,
	`NAME` VARCHAR(20) NOT NULL,
	MONEY DECIMAL(9,2) NOT NULL,
	PRIMARY KEY (ID)
)ENGINE = INNODB DEFAULT CHARSET = UTF8

INSERT INTO ACCOUNT(`NAME`, MONEY)
VALUES ('A', 2000),('B', 10000)

-- 模拟事务
SET AUTOCOMMIT = 0; -- 关闭自动提交
START TRANSACTION;

UPDATE ACCOUNT SET MONEY = MONEY - 500 WHERE `NAME` = 'B';
UPDATE ACCOUNT SET MONEY = MONEY + 500 WHERE `NAME` = 'A';

COMMIT;  -- 提交事务，持久化了
ROLLBACK;

SET AUTOCOMMIT = 1; -- 恢复默认值
```

## 7、索引

> 索引是数据结构

### 7.1、索引的分类

> 在一个表中，主键索引只能有一个，唯一索引可以多个

- 主键索引（primary key）
  - 唯一的表示，主键不可重复
- 唯一索引 （unique key）
  - 避免重复列出现，唯一索引可以重复，多个列都可以标识为唯一索引
- 常规索引（KEY/INDEX）
  - 默认的
- 全文索引（FULLTEXT）
  - 特定数据库才有，MyISAM，快速定位数据

```sql
SHOW INDEX FROM `subject`
-- 添加全文索引
ALTER TABLE `subject` ADD FULLTEXT INDEX `SubjectName`(`SubjectName`);

-- explain 分析sql执行的状况
EXPLAIN SELECT *FROM student; -- 非全文索引

EXPLAIN SELECT * FROM student WHERE MATCH(studentName) AGAINST('刘'); 
```

### 7.2、测试索引

建表

```sql
-- 一百万条数据检验索引的效果。
use school;-- 数据库school
DROP TABLE IF EXISTS `app_user`
CREATE TABLE `app_user` (
	`id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
	`name` varchar(50) DEFAULT '' COMMENT '用户昵称',
	`email` varchar(50) NOT NULL COMMENT '用户邮箱',
	`phone` varchar(20) DEFAULT '' COMMENT '手机号',
	`gender` tinyint(4) unsigned DEFAULT '0' COMMENT '性别（0:男；1：女）',
	`password` varchar(100) NOT NULL COMMENT '密码',
	`age` tinyint(4) DEFAULT '0' COMMENT '年龄', 
	`create_time`  timestamp NULL DEFAULT NULL COMMENT '创建时间',
    `update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
    -- Mysql 5.5版本，需要注意：`create_time`和`update_time` 插入数据时，针对创建时间字段：在sql里now()  或者在代码里new date()更改后的sql，把默认值给个空，否则报错。
	PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='app用户表';
```

造数

```sql
DROP FUNCTION IF EXISTS mock_data;
SET GLOBAL log_bin_trust_function_creators = 1;  
-- 写函数之前必须要写，标志:$$
DELIMITER $$
CREATE FUNCTION mock_data()
RETURNS INT
-- 注意returns，否则报错。
BEGIN
DECLARE num INT DEFAULT 1000000;
-- num 作为截止数字，定义为百万，
DECLARE i INT DEFAULT 0;
WHILE i < num DO
   INSERT INTO app_user(`name`, `email`, `phone`, `gender`, `password`, `age`)
   VALUES(CONCAT('用户', i), CONCAT('100',i,'@qq.com'), CONCAT('13', FLOOR(RAND()*(999999999-100000000)+100000000)),FLOOR(RAND()*2),UUID(), FLOOR(RAND()*100));
   SET i = i + 1;
END WHILE;
RETURN i;
END;
```



最后

```sql
SELECT mock_data();
```



查看效果：

```sql
EXPLAIN SELECT * FROM app_user WHERE `name` = '用户9999'
-- 耗时 0.408

-- 常规索引名命名方式  id_表名_字段名
-- create index 索引名 on 表(字段)
CREATE INDEX id_app_user_name ON app_user(`name`);

EXPLAIN SELECT * FROM app_user WHERE `name` = '用户9999'  
-- 建立索引后速度贼快 0.002sec
```

### 7.3、索引原则

- 不是越多越好
- 不要对经常变动的数据加索引
- 小数据量不需要加索引
- 索引一般加载常用的查询字段上

> 索引数据结构

Hash类型的索引

Btree：InnoDB默认数据结构

索引参考文献：http://blog.codinglabs.org/articles/theory-of-mysql-index.html

## 8、权限管理和备份

### 8.1、用户管理

> SQL命令操作

```sql
-- 新建用户
CREATE USER landon IDENTIFIED BY '123456'
 -- 修改当前密码
SET PASSWORD = PASSWORD('123456')
-- 修改用户密码
SET PASSWORD FOR landon = PASSWORD('123456')

-- 查看权限
SHOW GRANTS FOR landon 
SHOW GRANTS FOR root@localhost

RENAME USER landon TO landon2  -- 重命名

GRANT ALL PRIVILEGES ON *.* TO landon2 -- 设置所有权限

-- 撤销所有权限
REVOKE ALL PRIVILEGES ON *.* FROM landon2

-- 删除用户
DROP USER landon2
```

### 8.2、MySQL备份

- 保证重要数据不丢失
- 数据转移

MySQL数据库备份方式

- 拷贝物理文件
- 可视化工具中手动导出
- 命令行导出 mysqldump 命令行使用

```bash
# mysqldump -h 主机 -u 用户名 -p 密码 数据库 表名  >  物理磁盘名 

# mysqldump -h 主机 -u 用户名 -p 密码 数据库 表1 表2 表3  >  物理磁盘名 

# 入
#法二：先登录，#source 备份文件
	source D://a.sql
#法一：mysql -u用户名 -p密码 库名 < 备份文件
```



## 9、规范数据库设计

### 9.1、为什么要设计

当数据库复杂

### 9.2、三大范式

**第一范式（1NF）**

保证每一列不可再分，原子性

**第二范式（2NF）**

前提：满足第一范式，每张表只描述一件事情

确保数据库表中每一列和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）

**第三范式（3NF）**

满足第二范式，数据表中每一列数据都和主键直接相关，而不能间接相关



**规范性和性能问题：**

关联查询不得超过三张表

- 考虑商业化的需求和目标，（成本，用户体验）数据库性能更重要
- 规范性能的问题的时候，需要适当考虑一下规范性
- 会故意给某些表加一些冗余的字段（多表查询变成单表查询）
- 故意增加一些计算列

## 10、JDBC

## 10.1、数据库驱动

## 10.2、JDBC

sun公司为了简化开发人员的对数据库的统一的操作，提供了一个java操作数据库的规范，称为JDBC。

对于开发人员来说，我们只需要掌握JDBC接口的操作即可

java.sql

javax.sql

### 10.3、第一个JDBC程序

步骤：

1、加载驱动

2、连接数据库 DriverManager

3、获得执行SQL对象 Statement

4、获得返回结果集

5、释放连接

测试代码

```java
package com.landon.jdbc;

import java.sql.*;

public class JdbcDemo01 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1、加载驱动
        Class.forName("com.mysql.jdbc.Driver");

        //2、用户信息和url
        String url = "jdbc:mysql://localhost:3306/jdbcstudy??useUnicode=true&characterEncoding=utf8&useSSL=false"; //SSL=true要配置证书和密钥
        String username = "root";
        String password = "123456";
        //3、连接成功，数据库对象 connection代表数据库
        Connection connection = DriverManager.getConnection(url, username, password);
        //4、执行SQL的对象 statement执行SQL的对象
        Statement statement = connection.createStatement();
        //5、被执行的SQL，可能有结果，查看返回结果
        String sql = "select * from users";

        //返回结果集
        ResultSet resultSet = statement.executeQuery(sql);

        while (resultSet.next()) {
            System.out.println("id="+resultSet.getObject("id"));
            System.out.println("name="+resultSet.getObject("name"));
            System.out.println("pwd="+resultSet.getObject("PASSWORD"));
            System.out.println("email="+resultSet.getObject("email"));
            System.out.println("birth="+resultSet.getObject("birthday"));
            System.out.println("=================================");
        }
        //6、释放连接
        resultSet.close();
        statement.close();
        connection.close();
    }
}

```

>DriverManager

```java
//DriverManager.registerDriver(new com.mysql.jdbc.Driver) 
Class.forName("com.mysql.jdbc.Driver"); 
```



### 10.4、Statement对象

> Statement 执行sql的对象

1、提取工具类

```java
package com.landon.jdbc.utils;

import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class JdbcUtils {
    private static String driver = null;
    private static String url = null;
    private static String username = null;
    private static String password = null;

    static {
        try {
            InputStream in = JdbcUtils.class.getClassLoader().getResourceAsStream("db.properties");
            Properties properties = new Properties();
            properties.load(in);
            driver = properties.getProperty("driver");
            url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");

            //1、驱动只加载一次
            Class.forName(driver);
        } catch (IOException | ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }
    //获取连接
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, username, password);
    }
    //释放连接
    public static void release(Connection conn, Statement st, ResultSet rs) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        if (st != null) {
            try {
                st.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}

```

2、编写增删改的方法，executeUpdate

```java
package com.landon.jdbc;

import com.landon.jdbc.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestInsert {
    public static void main(String[] args) {
        Connection conn = null;
        Statement st = null;
        ResultSet rs = null;

        try {
            conn = JdbcUtils.getConnection();
            st = conn.createStatement();
            String sql = "INSERT INTO USERS(ID,NAME,PASSWORD,EMAIL,BIRTHDAY) VALUES(4,'landon','123456','123@qq.com','2020-01-01')";
            int i = st.executeUpdate(sql);
            if (i > 0) {
                System.out.println("insert success");
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JdbcUtils.release(conn, st, rs);
        }
    }
}

```



3、查询 executeQuery

```java
package com.landon.jdbc;

import com.landon.jdbc.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestSelect {
    public static void main(String[] args) {
        Connection conn = null;
        Statement st = null;
        ResultSet rs = null;

        try {
            conn = JdbcUtils.getConnection();
            st = conn.createStatement();
            String sql = "select * from users where id = 1";
            rs = st.executeQuery(sql);
            while (rs.next()) {
                System.out.println(rs.getString("name"));
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JdbcUtils.release(conn, st, rs);
        }
    }
}

```



> SQL注入问题

```java
package com.landon.jdbc;

import com.landon.jdbc.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class SqlInjection {
    public static void main(String[] args) {
        //login("Landon","123456");
        login("' or '1=1", "' or '1=1");
    }

    public static void login(String username, String password) {
        Connection conn = null;
        Statement st = null;
        ResultSet rs = null;

        try {
            conn = JdbcUtils.getConnection();
            st = conn.createStatement();
            //select * from users where name = 'Landon' and password = '123456';

            //select * from users where name = '' or '1=1' and password = '' or '1=1';
            String sql = "select * from users where name = '"+username+"' and password = '"+password+"'";
            rs = st.executeQuery(sql);
            while (rs.next()) {
                System.out.println(rs.getString("name"));
                System.out.println(rs.getString("password"));
                System.out.println("===========================");
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JdbcUtils.release(conn, st, rs);
        }
    }
}

```

### 10.5、PrepareStatement对象

防止SQL注入，效率更好

```java
//PrepareStatement防止SQL注入的本质，把传递进来的参数当做字符
//假设其中存在转义字符，就直接忽略，比如说' 会被直接转义
```



### 10.6、事务

代码实现：

1、开启事务 conn.setAutoCommit(false);

2、一组事务执行完毕，提交事务

3、可以再catch语句中显示定义回滚语句，但默认失败就会回滚



### 10.7、数据库连接池

数据库连接---执行完毕---释放

连接---释放 十分浪费系统资源

**池化技术：准备一些预先资源，过来就连接预先准备好的**

--开门---业务员：等待--服务--

常用连接数 10

最小连接数 10

最大连接数 100 业务最高承载上限

排队等待

等待超时 100ms



编写连接池：实现接口 DataSource

> 开放数据源实现 (拿来即用)

DBCP

C3P0

Druid：阿里巴巴

使用这些连接池后，项目中不需要自己写连接数据库代码

> DBCP

需要的jar包

commons-dbcp-1.4.jar  	commons-pool-1.6.jar

> C3P0

需要的jar包

c3p0-0.9.5.5.jar 	mchange-commons-java-0.2.19.jar

> 结论

无论使用什么数据源，本质一样的，DataSource接口不变，方法就不会变