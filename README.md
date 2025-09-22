# 今日任务

- 了解项目
- 搭建开发环境
- SpringBoot快速入门
- Thymeleaf快速入门
- 导入静态资源



# 1, 项目介绍

实现一个小型的在线商城系统。
每天完成阶段性练习。
最终提交一个作品。

## 1.1 功能概述

### 用户角色

- 后台管理员
- 普通用户

### 后台管理员的功能

- 管理商品
- 管理类别
- 管理用户

### 普通用户的功能

- 注册 / 登录
- 浏览商品
- 购买商品
- 购物车
- 订单管理
- 管理用户信息

## 1.2 项目演示

项目演示。

## 1.3 技术栈

- 前端 HTML, CSS, JavaScript, jQuery
- 后端使用 Java SpringBoot 3 框架。SpringBoot已经成为Java企业级开发的基础框架。尤其是在微服务架构中。
- 数据库框架：MyBatis Plus
- 数据库服务器：MySQL 8
- MVC 模式
- 页面模板：Thymeleaf
- 为降低复杂度，不采用前后端分离模式

## 1.4 架构

- **controller** 控制器

- **entity** 实体类

- **service** 服务接口

- **service.impl** 服务接口实现

- **mapper** Mybatis 映射接口

- **util** 工具类

- Mybatis mapper xml 文件

  

# 2, 环境搭建

- 项目分组。可个人单独完成；也可分组完成（3人以内）
- JDK17 以上
- Maven 3.9（修改镜像）

```xml
<mirrors>
    <mirror>
        <id>aliyun</id>
        <name>aliyun</name>
        <mirrorOf>central</mirrorOf>
        <!-- 国内推荐阿里云的Maven镜像 -->
        <url>https://maven.aliyun.com/repository/central</url>
    </mirror>
</mirrors>
```

- Idea专业版（试用或破解）或社区版（免费），并配置maven为安装的maven

- MySQL8 及客户端工具



# 3, springboot 快速入门

使用 Idea 创建 maven 工程，工程名 mall

项目包名可取为 org.example

## 3.1 pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>mall</artifactId>
    <version>1.0-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.3</version>
    </parent>
    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    <dependencies>        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
		<!-- springboot测试工具 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- 打包插件 -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

快速创建一个 springboot web 应用程序

## 3.2 编写主启动类

创建 org.example.Application

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## 3.3 创建控制器

创建类：org.example.TestController

```java
@Controller
@RequestMapping("/test")
public class TestController {

    @GetMapping("/t1")
    @ResponseBody
    public String test1(){
        return "Hello";
    }
}
```

以上以 JSON 格式返回，对于实体类对象也是如此（请实验）。

### 重定向

```java
@RequestMapping("/test1")
public String test1(){
    return "redirect:/test2";
}

@RequestMapping("/test2")
@ResponseBody
public String test2(){
    return "I am test2";
}
```



## 3.4 Controller 传参

- RequestParam 方式

```java
public String foo(@RequestParam("phone_number") String user)
```

- PathVariable 方式

```java
@RequestMapping("/user/{id}")
public String foo(@PathVariable("id") String userId)
```



## 3.5 引入 Service

一般不在 Controller 中编写大量复杂的业务逻辑，而是使用 service

- service 接口：org.example.service.UserService

```java
public interface UserService {
    int login();
}
```

- service 实现：org.example.service.impl.UserServiceImpl

**注意：** **@Service** 注解

```java
@Service
public class UserServiceImpl implements UserService {
    @Override
    public int login() {
        return 100;
    }
}
```

在控制器中自动装配 Service

```java
@Resource
UserService userService;
```

## 3.6 打包应用

```shell
# 打包
mvn clean package
# 运行
java -jar [jar包]
```



# 4, 使用 Thymeleaf

由于本项目并不采用前后端分离模式，因此，为了编写页面方便，引入模板引擎。

## 4.1 快速使用

1, 引入依赖

```xml
<!-- Thymeleaf模板引擎 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

2, 编写控制器

```java
@Controller
@RequestMapping("/test")
public class TestController {

    @GetMapping("/t2")
    public String test2(){
        return "index";      // 不用写 .html
    }
}
```

3, 编写模板文件，放在 **resources/templates/** 下

resources/templates/index.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        
    </head>
    <body>
        Index
    </body>
</html>
```

4，测试

5，访问静态资源

静态资源存放在 **resources/static** 目录下，可以存放 css，js，image 等文件，例如：

```html
<img th:src="@{/img/1.png}">
<link rel="stylesheet" th:href="@{/css/a.css}" />
```

**注意**：使用绝对路径



## 4.2 Thymeleaf 语法

### 在控制器中传递变量

```java
@GetMapping("/")
public String index(Model model){
    model.addAttribute("name", "张三");
    return "index";
}
```



### thymeleaf 对于变量的操作主要有$\*\#@四种方式：

变量表达式： ${...}，是获取容器上下文变量的值.
选择变量表达式： *{...}，获取指定的对象中的变量值。如果是单独的对象，则等价于${}。
消息表达式： #{...}表达式与th:text一起使用，加载数据源中的消息，用于国际化
链接网址表达式： @{...}，获取网址链接

```html
文本值：
<h1 th:text="${name}"></h1>
<span>[[${name}]]</span>
<td th:text="${person.age >= 18 ? 'adult' : 'teenage'}"></td>

拼接字符串：
<h1 th:text="${'prefix' + ${name} + 'suffix'}"></h1>
<h1 th:text="|prefix ${name} suffix|"></h1>
<td th:text="| ${person.age} / ${person.age >= 18 ? '成年' : '未成年'} |">
    
属性：
<img th:src="@{image}">
<a th:href="@{/image}">链接</a>

假设有一个用户对象user，包含name、sex、age等属性
<div th:object="${user}">
    <p th:text="*{name}"></p>
    <p th:text="*{sex}"></p>
    <p th:text="*{age}"></p>
</div>

```

### 循环渲染

```html
<li th:each="goods : ${goodsList}">
  <span th:text="${goods.name}"></span>
</li>
```

### switch

```html
<div th:switch="${role}">
  <p th:case="admin">User is admin</p>
  ...
</div>
```

### if
```html
<div th:if="${age>18}"> Hello </div>
<div th:if="${flag}" th:text="true"> Hello </div>
```

### 模板布局

```html
模板布局
  1,定义模板 th:fragment
  2,引用模板 ~{templatename::selector}
  3,插入模板 th:insert, th:replace

1, common.html
<html>....
<body>
 <nav th:fragment="nav"></nav>


2, index.html
<html>...
<body>
...
<div th:replace="~{common :: nav}"></div>
```

# 5，国际化

SpringBoot 默认国际化属性文件是 **messages.properties**，放在类路径下（resources/）

例如：

- messages.properties 默认
- messages.en_US.properties 美国英语
- messages.zh_CN.properties 中文简体

```properties
login=登录
```
> 注意：将文件编码设置为 UTF-8

在 Thymeleaf 模板中引用国际化属性：

```html
<span th:text="#{login}"></span>
```

在 Java 代码中引用国际化属性：

```java
@Autowired
MessageSource messageSource;

...
String str = messageSource.getMessage("login", null, Locale);
```



# 6, 作业：导入静态资源

1, 后台管理页静态资源

2, 普通用户页静态资源



静态资源放在 **resources/static** 目录下

模板文件放在 **resources/templates** 目录下



