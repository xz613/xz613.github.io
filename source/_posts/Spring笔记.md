---
title: Spring笔记
date: 2025-03-11 19:12:33
tags:
---
## Spring笔记

### IOC对象创建的方式：



### AOP实现方式

**AOP**:面向切面编程：spring 框架用动态代理技术来支持面像切面编程，spring的动态代理有两种方式：

**1、JDK动态代理：**基于接口的代理

**2、CGLB代理：**基于类的代理

需要了解两个类：

- proxy ：代理
- InvocationHandler ：调用处理程序

​	Invocation Handler内部只有一个方法

object invoke(objecT proxy,method, object[] args)throws throwable



1、JDK动态代理

​	JDK动态代理是 Java 本身提供的代理机制，要求目标类必须实现接口。Spring 会创建一个目标接口的代理对象，在该对象上执行方法时，实际回委托给InvocationHandler处理。



### 回顾Servlet

Servlet 是 Java 编程语言中的一种服务器端技术，用于扩展服务器的功能，尤其是在 Web 应用程序中。它们可以响应客户端（通常是浏览器）发出的请求，并生成动态的 Web 内容。Servlets 是 Java EE（现 Jakarta EE）平台的一部分，广泛用于构建 Web 应用程序。

**基本概念**：

Servlet 是一种运行在服务器上的 Java 类，它接收并处理来自客户端的请求，生成相应的响应。Servlet 通常用于动态生成 HTML 内容、处理表单提交、与数据库交互等操作。

**工作原理：**

- **客户端请求：**客户端（如Web浏览器）像服务器发送请求，通常是HTTP请求。
- **Servlet容器接受请求：**Web服务器（如Tomcat，Jetty）接受HTTP请求后，将其发送给像应得Servlet。
- **Servlet处理请求：**Servlet容器通过Servlet得service()方法来处理请求。在service()方法中，Servlet根据请的内容（如GET、POST请求）调用 doGet() 或 dopost()方法来生成响应。
- **Servlet生成相应：**Servlet 将响应内容( 如 HTML、JSON、XML等发送回客户端)。



### SpringMVC

**初识SpringMVC**

Spring MVC 是一个基于 **Java** 的 Web 应用框架，是 **Spring Framework** 的一部分，旨在通过 **Model-View-Controller (MVC)** 设计模式，帮助开发者构建松耦合、易维护的 Web 应用程序。它提供了一个清晰的请求处理流程，支持面向对象的开发，并能与各种视图技术（如 JSP、Thymeleaf）结合使用。

**基本概念：**

**Spring MVC** 是一个轻量级的 Web 框架，它使用 **DispatcherServlet** 作为前端控制器来统一处理 HTTP 请求，并将请求分发给不同的处理器（Controller）。它遵循经典的 **MVC（Model-View-Controller）模式**，使得应用程序的不同部分职责分明：

- **model:**	表示应用程序的核心数据和业务逻辑。通常是 POJO（Plain Old Java Object）类，用来封装数据。
- **View:**            表示数据的显示(通常是HTML页面)。spring MVC支持多中视图技术，如JSP、Thymeleaf等。
- **Controller:**   处理用户的请求，协调Model 和  View  之间的交互，最终返回视图。



**Spring MVC执行原理概述**

Spring MVC 的执行原理可以简化为以下几个步骤：

1. **客户端发送请求**：浏览器向服务器发送 HTTP 请求。
2. **DispatcherServlet 拦截请求**：所有的请求都经过 **DispatcherServlet**，它是 Spring MVC 的前端控制器，负责统一处理请求。
3. **HandlerMapping 查找处理器（Controller）**：DispatcherServlet 根据请求的 URL 查找对应的 Controller（通过 HandlerMapping）。
4. **Controller 处理请求**：找到对应的 Controller 后，调用该 Controller 中的方法来处理请求。
5. **返回 ModelAndView**：Controller 执行完逻辑后，返回一个 **ModelAndView** 对象，其中包含了模型数据和视图名称。
6. **ViewResolver 解析视图**：DispatcherServlet 使用 **ViewResolver** 来解析视图名称，找到对应的视图资源（例如 JSP 页面）。
7. **渲染视图并响应客户端**：最终，Spring MVC 渲染视图并将生成的 HTML 内容返回给客户端。

**1. 请求到达 DispatcherServlet**

- **DispatcherServlet** 是 Spring MVC 的前端控制器，所有的 HTTP 请求都会首先被它接收和处理。它是一个 Servlet，在 `web.xml` 中配置。
- 通过配置 `DispatcherServlet` 的 `url-pattern`，通常设置为 `/`，意味着所有的请求都会通过它进入 Spring MVC 的处理流程。

```
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

**2. DispatcherServlet 查找 HandlerMapping**

- DispatcherServlet 负责将请求转发给合适的 **HandlerMapping**。
- **HandlerMapping** 根据请求的 URL 查找相应的 **Controller**，并将请求映射到相应的处理方法。
- Spring MVC 支持多种 **HandlerMapping**，最常用的是 **RequestMappingHandlerMapping**，它通过分析请求的 URL 和 Controller 中 `@RequestMapping` 注解的映射关系，来找到对应的处理器方法。

**3. 调用 Controller 方法**

- 一旦 **HandlerMapping** 确定了要执行的 Controller 和方法，它会把请求交给对应的 **Controller**。
- **Controller** 方法通过业务逻辑处理请求，并将结果封装到 `Model` 中。方法可以返回一个 **ModelAndView** 对象，其中包含了视图名称和模型数据。

例如：

```
@Controller
public class HelloController {

    @RequestMapping("/hello")
    public String hello(Model model) {
        model.addAttribute("message", "Hello, Spring MVC!");
        return "hello";  // 返回视图名称
    }
}
```

**4. 返回 ModelAndView**

- Controller 方法返回的是一个 **ModelAndView** 对象，它包含了视图的名称和需要传递给视图的数据模型。
- 例如，`"hello"` 是视图名称，而 `Model` 则是包含数据的对象。

**5. 视图解析（ViewResolver）**

- DispatcherServlet 使用 **ViewResolver** 来解析视图名称（如 `"hello"`），并找到对应的视图实现（通常是 JSP 或 Thymeleaf 模板）。
- **ViewResolver** 会根据视图名称（例如 `hello`）拼接出具体的视图文件路径（例如 `/WEB-INF/views/hello.jsp`）。

配置视图解析器（`InternalResourceViewResolver`）示例：

```
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/views/" />
    <property name="suffix" value=".jsp" />
</bean>
```

- 然后，Spring MVC 会通过 **ViewResolver** 查找实际的视图文件，并将模型数据填充到视图中。

**6. 渲染视图并返回响应**

- 一旦视图被解析，Spring MVC 会通过 **View** 渲染最终的 HTML 页面，最终将 HTML 内容返回给客户端浏览器。
- 视图通常是一个 JSP 文件、Thymeleaf 模板、Freemarker 模板等。

**7. 请求响应完成**

- 最终，客户端接收到响应，浏览器展示渲染后的 HTML 页面。

**Spring MVC 执行流程图**

```
客户端请求  --->  DispatcherServlet  --->  HandlerMapping  --->  Controller  --->  返回 ModelAndView
                                |
                           ViewResolver
                                |
                           视图解析
                                |
                           渲染视图 (JSP, Thymeleaf)
                                |
                           返回 HTTP 响应
```



### RequestMapping注解

@RequestMapping 可以作用于类和方法级别。

**类级别映射**

通常用于定义一个公共的基础路径，所有方法会继承这个路径。

```
@Controller
@RequestMapping("/user")
public class UserController {

    // 处理请求 /user/list
    @RequestMapping("/list")
    public String listUsers() {
        return "userList";
    }

    // 处理请求 /user/add
    @RequestMapping("/add")
    public String addUser() {
        return "addUser";
    }
}
```

**方法级别映射**

在方法级别，`@RequestMapping` 注解可以通过设置不同的属性来匹配特定的请求。

```
@Controller
public class UserController {

    // 处理 GET 请求
    @RequestMapping(value = "/user", method = RequestMethod.GET)
    public String getUser() {
        return "userDetails";
    }

    // 处理 POST 请求
    @RequestMapping(value = "/user", method = RequestMethod.POST)
    public String addUser(@RequestBody User user) {
        // 保存用户
        return "userAdded";
    }
}
```

### RESTFul 风格

Spring 中的 RESTful 风格 Web 服务设计非常灵活且易于实现。通过使用 `@RestController` 和相关的注解，开发者可以快速构建符合 REST 架构风格的 API。通过合理设计 URL 和 HTTP 方法，开发出的 Web 服务可以清晰、简洁地实现资源的 CRUD 操作。

常用的操作：

- `@GetMapping`：获取资源。
- `@PostMapping`：创建资源。
- `@PutMapping`：更新资源。
- `@DeleteMapping`：删除资源。

通过 Spring Boot 和 Spring MVC，开发 RESTful 风格的 Web 服务变得更加高效，同时还可以通过集成工具（如 Swagger）提供更好的开发体验。



### **重定向和转发**
q
**重定向（Redirect）：**是指服务器告诉客户端 (通常是浏览器) 重新发起一个新的请求到另一个URL,浏览器回请求一个新的URL并显示新的页面。与转发不同，重定向会导致URL发生变化。

**转发:**	转发是指服务器内部将请求转发到另一个资源 (例如一个视图或控制器方法)，客户端不知道这个转发过程，浏览器的地址栏也不会发生变化。

**转发和重定向的区别：**

| 特性         | 转发（Forward）                                      | 重定向（Redirect）                            |
| ------------ | ---------------------------------------------------- | --------------------------------------------- |
| URL 是否变化 | 不变化                                               | 变化（浏览器地址栏 URL 会改变）               |
| 请求次数     | 只有一次请求，浏览器与服务器之间不发生新的 HTTP 请求 | 有两个 HTTP 请求（第一次请求 + 重定向的请求） |
| 适用场景     | 内部逻辑处理后转发到另一个页面或视图                 | 页面跳转、避免重复提交表单等                  |
| 性能开销     | 低，只有一次请求                                     | 高，包含两次 HTTP 请求                        |
| 用户感知     | 用户不感知发生了转发，地址栏 URL 不变                | 用户会看到新的 URL                            |
| 用途         | 内部服务器处理请求后转发到另一个资源                 | 发送指令给浏览器重新发起请求，常用于页面跳转  |

