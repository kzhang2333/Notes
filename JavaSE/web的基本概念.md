## **JavaWeb**

# 1. 基础

## 1.1 基本概念

web开发
- web，网页的意思
- 静态web
  - html，css
  - 所有人看都一样
- 动态web
  - 不同人，不同时间，不同地点看到的信息个不相同
  - 如淘宝
  - 技术栈：Servlet/JSP, ASP, PHP
在java中，动态web资源开发的技术统称为Javaweb

## 1.2 web应用程序
web应用程序：可以提供浏览器访问的程序
- a.html, b.html....多个web资源，这些web资源可以被外界访问
- 你们能访问到的任何一个页面或者资源，都存在于这个世界的某一个角落的计算机上
- URL
- 这个同意的web资源会被放在同一个文件夹下，web应用程序
- 一个web应用分为多个部分（静态web，动态web）
  - html，css，js
  - jsp， servelet
  - java程序
  - jar包
  - 配置文件（properties）
web应用程序编写完毕后，若想提供给外界访问：需要一个服务器来统一管理

## 1.3 静态web
- *.html, *.html, 这些都是网页的后缀，如果拂去其上一直存在这些东西，我们就可以直接进行读取
- 一次请求，一次相应：request & response
- 静态web的缺点
  - web页面无法动态更新，所有的用户看到都是同一个页面
    - 轮播图，点击特效：伪动态
    - JavaScript
    - VBScript
  - 无法和数据库交互（数据无法持久化，用户无法交互）

## 1.4 动态web
页面会动态展示：“web的页面展示效果因人而异”
缺点
- 假如服务器的动态web资源出现了错误，我们需要重新编写我们的**后台程序**，重新发布：
  - 停机维护

优点
- web页面可以动态更新，所有用户看到都是不同页面
- 可以与数据库交互（数据持久化：注册等等）

# 2. web服务器
## 2.1 技术讲解
ASP
- 微软：国内最早流行的就是asp
- 在HTML中嵌入了VB的脚本，ASP + COM
- 在ASP开发中，基本一个页面都有几千航的业务代码，页面及其混乱
- 维护成本及其高
- C#
- IIS

```html
<h1>
  <h2>
  <% System.out.println("hello") %>
  </h2>
</h2>
```

PHP
- PHP开发速度很快，功能很强大，跨平台，代码很简单
- 无法承载大访问量的情况

JSP/servelet:
B/S: 浏览和服务器
C/S：客户端和服务器
  - sun公司主推的B/S架构
  - 基于Java语言的
  - 可以承载三高问题带来的影响
  - 语法像ASP， 可接盘ASP程序员

## 2.2 web服务器
服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息

IIS
微软的；用来跑ASP,windows中自带

Tomcat
Tomcat是apache软件基金会的Jakarta项目中的一个核心项目，最新的Servelet和JSP规范可在其中体现。
Tomcat技术先进，性能稳定，**免费**， 是比较流行的web应用服务器。

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用服务器，在中小型系统和
并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。

> 如何学习一个新知识
1. 下载安装
2. 了解配置文件以及目录结构
3. 这个东西的作用


# 3. Tomcat
## 3.1 下载

注意：Tomcat没有所谓的安装，只有解压

解压至Environment文件夹(与jdk在一起)

## 3.2 文件夹作用
1. bin - 启动, 关闭的脚本文件
2. conf - 配置
3. lib - 依赖的jar包
4. logs - 日志
5. webapps - 存放网站的

## 3.3 启动关闭

启动: bin/startup.bat

关闭: bin/shutdown.bat OR 关闭对话框

访问测试: http://localhost:8080/

conf\server.xml - 服务器核心配置文件

可以配置启动的端口号

- tomcat的默认端口号:8080
- mysql: 3306
- http: 80
- https: 443

```xml
<Connector port="8081" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```
可以配置主机的名称

- 默认的主机名:  localhost 对应 127.0.0.1
- 默认的网站应用存放位置: webapps 
- 注意这些都可以改, 但是

```xml
<Host name="localhost"  appBase="webapps"
      unpackWARs="true" autoDeploy="true">	
```
### 请你谈谈网站是如何访问的, 高难度面试题

请你谈谈网站是如何进行访问的:

1. 输入一个域名; 回车

2. 检查本机的C:\Windows\System32\drivers\etc\hosts配置文件下有没有这个域名映射

   1. 有: 直接返回对应的ip地址, 这个地址中, 有我们需要访问的web程序, 可以直接访问

      ```txt
      127.0.0.1       localhost
      ```

   2. 没有: 去dns服务器找, 找到的话就返回, 找不到的话就返回找不到

      **此处应该有图**

## 3.4 发布一个web网站

**不会就先模仿!**

- 将自己写的网站, 放到服务器(tomcat)中制定的web应用文件夹(webapps)下, 启动服务器, 就可以访问了

```
--webapps: Tomcat服务器的web目录
	-ROOT
	-myapp: 网站的目录名
		- WEB-INF
			-classes: java程序
			-lib: web应用依赖的jar包
			-web.xml: 网站的配置文件
		- index.html 默认的首页
		- static
			-css
				-style.css
			-js
			-img
		...
```

> 问题: 直接用浏览器打开html和用tomcat server有什么区别?

# 4. HTTP

## 4.1 什么是HTTP

HTTP(超文本传输协议)是一个简单的请求-相应协议,它通常运行在TCP之上

- 文本: html, 字符串....
- 超文本: 图片, 音乐, 视频, 定位, 地图......
- 默认端口: 80

Https: 安全的

- 默认端口: 443

## 4.2 两个时代

- http1.0
  - HTTP/1.0: 客户端与服务器连接后, 只能获得一个web资源, 断开连接
- http2.0
  - HTTP/1.1 : 客户端与服务器连接后, 可以获得一个web资源

## 4.3 Http请求 - request

- 客户端 -- 发请求 ---> 服务器

1. 请求行

```java
Request URL: https://www.google.com/    // 请求地址
Request Method: GET          			// get方法
Status Code: 200 						// status
Remote Address: [2607:f8b0:4005:805::2004]:443	// 远程
Referrer Policy: no-referrer-when-downgrade
```
- 请求行中的请求方式: GET
- 请求方式: Get, Post, HEAD, DELETE....
  - get: 能携带的参数比较少, 大小限制,会在浏览器的url地址栏显示数据内容, 不安全, 到那时高效
  - post: 能携带的参数没有限制, 大小没有限制, 不会再浏览器的RUL地址栏显示数据内容, 安全, 但不如get 高效
2. 消息头 Header

```java
accept: text/html
accept-encoding: gzip, deflate, br
accept-language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7,sl;q=0.6,it;q=0.5
cache-control: max-age=0
```
## 4.4 Http相应 - Response

- 服务器 -- 相应 ---> 客户端

谷歌 Response Header

```java
cache-control: private, max-age=0		// 缓存控制
content-encoding: br					// 编码
content-length: 63601
content-type: text/html; charset=UTF-8	// 类型
```

### 1. Request Body

```java
accept: text/html
accept-encoding: gzip, deflate, br
accept-language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7,sl;q=0.6,it;q=0.5
cache-control: max-age=0　 // 缓存控制
Refresh: 告诉客户端, 什么时候刷新一次
Location:让网页重新定位
```

### 2. Status code

200: 成功

3xx: 请求重新定向

		- redirection, 重新给你到新位置去

4xx: 找不到资源, 如404

	- 资源不存在

5xx: 服务器代码错误, 如 500, 502: 网关错误



**常见面试题**:

当你浏览器中的地址栏输入地址并回车的一瞬间到页面能够展示回来, 经历了什么? 



# 5. Maven

## 5.0 WHY?

1. 在javaweb开发中, 需要使用大量的jar包(如tomcat的lib中就是jar包), 我们需要手动去导入
2. 如何能够让一个东西自动帮我导入和配置jar包(就像npm)

## 5.1 Maven项目架构管理工具

Maven的核心思想：**约定大于配置**

- 有约束, 不要去违反

Maven会规定好你改如何去编写我们的java代码, 必须按照这个规范来

## 5.2 Download and Installation/Unzip

- bin - mvn在此

## 5.3 配置环境变量

在系统环境变量中 , 配置如下配置:

- M2_HOME 	maven目录下的bin
- MAVEN_HOME     maven目录
- 为什么用"M2_HOME" 和"MAVEN_HOME": 因为之后用的工具, 如springboot默认如此

在系统的path中, 配置MAVE_HOME

- %MAVE_HOME%\bin

在cmd中测试

- mvn -version

## 5.4 阿里云mirror(optional)

- 镜像: mirrors, 在setting.xml内, 在mirrors tag内增加

```
look for it when you need it
```



- 国内可以使用

## 5.5 本地仓库

1. 在本地environment\maven中新建仓库文件夹
2. 在setting文件中, 设置localRepository位置为此文件夹位置

```xml
<localRepository>C:\Environment\apache-maven-3.6.3\maven-repo </localRepository>
```

## 5.6 在IDEA中创建一个MAVEN(使用模板)

1. 启动IDEA
2. 创建一个Maven项目
   1. 选择Maven
   2. 检查java版本
   3. 勾选Create from 模板, 点选webapp
3. Maven - GAV
   1. G - 
   2. A - 
   3. V - 
4. 项目名字和位置
5. 选择Maven 版本, setting, local-respo
6. 等待downloading
   1.  BUILD SUCCESS 代表build完成
7. 观察maven仓库中多了什么, local-repo
8. 检查IDEA中的Maven使用的是否是自定义的版本(而非idea自带版本), 以及正确使用local-repository

## 5.7 在IDEA中创建一个MAVEN(不使用模板)

不勾选create from 模板, 即可获得干净Maven项目



## 5.8 Mark 文件夹属性

两种方法

1. 新建dir, 右键, mark directory as
2. file, project structure, 选中dir, 上方点选/反选directory属性

## 5.9 在IDEA中配置Tomcat

配置tomcat, 选择网站入口

## 5.10 pom文件

pom.xml 是maven的核心配置文件, 就像npm 项目里的 package.json一样

**Maven Respository** 就像npm仓库一样, 去这里搜索需要的包, 然后将其include至pom文件中

## 5.11 web.xml 文件

web.xml文件是servlet项目核心文件, 在这里可以注册servlet并设置servlet mapping

# 6. Servlet

## 6.1 What is Servlet?

- Servlet是sun公司开发动态web的一门技术
- Sun在这些API中提供一个接口: Servlet. 如果你想开发一个Servlet程序, 只需要完成两个小步骤:
  - 编写一个类, 实现Servlet接口
  - 把开发好的Java类不熟到web服务器中

**把实现了Servlet接口的Java程序叫做, Servlet**

## 6.2 Hello Servlet

1. 建立一个空(不选择模板)的Maven项目作为主目录, 删除src目录, 专门用来存放modules, 尽量多放一些依赖

2. Maven父子工程

   子项目可以继承主项目的内容, 主项目不能用子项目的内容. 类似继承

   ```java
   son extends father
   ```

3. Maven环境优化