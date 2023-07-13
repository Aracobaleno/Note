# Git

> 学习时的痛苦是暂时的 未学到的痛苦是终生的

[学习链接](https://oschina.gitee.io/learn-git-branching/)

## 版本控制

1、本地版本控制

2、**集中版本控制**（SVN）：所有东西放在服务器上

3、**分布式版本控制**（Git）：所有版本信息仓库 同步到本地的每个用户 

> 常用Linux命令

1）、cd：改变目录

2）、cd ..：返回上级目录

3）、pwd：显示当前路线

4）、ls（ll）：列出目录中的文件，ll列出更详细

5）、touch：新建文件 touch index.js

6）、rm：删除文件 rm index.js

7）、mkdir：创建目录，文件夹

8）、rm -r：删除文件夹

9）、mv：移动文件 mv index.js  test（这是同一目录下）

10）、reset：重新初始化终端/清屏

11）、clear：清屏

12）、history：查看历史命令

13）、help：帮助

14）、exit：退出

15）、#表示注释



## Git环境配置

> Git配置

查看配置 git config -l

系统配置 git config --system --l

本地配置 git config --global --list  全局、当前登录用户配置

> 设置用户名和邮箱（用户表示、必要）

```shell
git config --global user.name "Arcobaleno"  #名称
git config --global user.email zicaijuaner@gmail.com #邮箱
```





## Git基本理论（核心）

>工作区域

Git本地三个工作区：工作目录（Working Directory）、暂存区（Stage/Index）、资源库（Repository或Git Directory）。如果加上远程的Git仓库（Remote Directory）就为四个工作区



## Git项目搭建

1、git init 创建全新仓库

2、git clone [url] 克隆远程仓库 



## Git文件操作

> 查看文件状态

```shell
#查看指定文件状态
git status []

#查看所有文件状态
git status

#添加所有文件到暂存区
git add .

#提交暂存区内容到本地仓库
git commit -m “消息内容”# -m 提交信息
```



> 忽略文件

”.gitignore“文件 

1、（#）注释

2、Linux通配符，（*）代表任意多个字符，（?）代表一个字符，（[abc]）代表可选字符范围，（{string1，string2，...}）代表可选的字符串等。

3、名称前面是（!），表示例外，将不被忽略

4、名称前面路径分隔符是（/），忽略的文件在此目录下，而子目录中的文件不忽略

5、名称前后路径分隔符是（/），忽略此目录下该名称的子目录，而非文件（默认文件或目录都忽略）



