---
title: springboot笔记小结
date: 2025-03-11 19:20:40
tags:
---
### springboot 文档参考连接：

Springboot知识体系详解：https://www.pdai.tech/md/spring/springboot/springboot.html

Springboot官网：https://spring.io/projects/spring-boot

博客：[springboot 开发](https://www.liaoxuefeng.com/wiki/1252599548343744/1282386201411617)

实战开发：[Spring Boot 实战开发](https://learn.lianglianglee.com/专栏/Spring Boot 实战开发)

### 一，什么是SpringBoot

Spring Boot 是一个基于Spring 框架的开源Java开发框架，它用于简化Spring应用的配置和部署。它的目标是让开发者能够过更轻松地创建基于Spring的应用程序，并减少了配置和复杂的开发流程。SpringBoot 通过以下几个特性来实现简化开发。

**1，自动配置：**Spring Boot 会通过配置应用所需的许多组件，开发者不需要手动配置和定义Bean，Spring Boot会根据项目中的依赖自动进行配置。

**2，内嵌的服务器：**Spring Boot 可以直接将应用部署为可执行的JAR文件，内嵌如 Tomcat，Jetty等web服务器，免去外部服务器的配置和依赖。

**3，约定大于配置：**Spring Boot 提供了许多默认的配置，帮助开发者减少了手动配置的复杂性，只需要做出少量修改即可。

**4，开箱即用：**SpringBoot 提供了一系列的开箱即用功能，例如Aucuator（用于监控和管理），Spring Data Spring Security等。

**5，Spring Boot Starter:** 这是一些预配置的模块，涵盖了Web开发，数据访问，消息传递等常见功能，极大的简化了构建应用的过程。

**6，命令行界面(CLI):** Spring Boot 也提供了命令行界面（CLI）,让你能快速的通过脚本或命令行创建应用。



### **二，什么是微服务架构？**

微服务架构（Microservices Architecture）是一种设计和构建软件的方法，它将一个大型的，复杂的应用拆解成一组小的，独立的服务。每个微服务都专注于一组小的，独立的服务。每个微服务都专注于一个特定的功能或者业务逻辑，并且可以独立开发，部署，扩展，和维护。微服务之间通过轻量级的通信机制 (通常是 HTTP/REST 或者消息队列) 进行交互。



### 三，SpringBoot 快速创建

1，修改端口号

resources/application.properties	 server.port = 8081

2,修改bannner

添加 resources/banner.txt  内部存放logo 图片。



### 四，原理初探

#### 1，自动装配

pom.xml

- spring-boot-dependencies：核心依赖在父工程中！
- 我们再写或者映入SpringBoot 的时候不需要指定版本，就是因为有这些版本仓库。



#### 2，启动器

- ```
  <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter</artifactId>
  </dependency>
  ```

- 启动器：就是Springboot 的启动场景

- 比如 spring-boot-starter-web，它会帮我们自动导入所有web环境需要的依赖。

- Spring 会将所有需要的功能场景变成一个一个的启动器。

- 我们需要什么功能，就找到对应的启动器就好了  ‘starter’



#### 3，主程序

```
@SpringBootApplication   //标注这个应用是一个springboot 应用
public class SpringBoot01HelloApplication {

    public static void main(String[] args) {
    
    	//将springboot应用启动
        SpringApplication.run(SpringBoot01HelloApplication.class, args);
    }

}
```

**简单分析一下 SpringApplication 这个类 和它的 run 方法。**

- **SpringApplication主要用于**

    - 启动spring 应用的上下文
    - 配置应用的环境
    - 自动化装配 Spring 配置和组件。
    - 启动嵌入式的 Web 服务器 （如果是Web应用）。
    - 执行Spring Boot 特定的初始化操作。

- **SpringApplication.run() 方法**

  启动SpringBoot 应用常用的方式是通过 springApplication.run（）方法。它通常是在main（）方法中调用：

  ```
  public static void main(String[] args) {
      SpringApplication.run(Application.class, args);
  }
  ```

  这个方法的作用是：

    - 启动Spring 应用上下文（ApplicationContext）。
    - 执行SpringBoot 的自动装配过程。
    - 启动内嵌的Web 服务器 (如果是web应用)

- **SpringApplication.run（） 的工作流程**

1. **创建 SpringApplication 实例：**

   创建一个 springApplication 实例，该实例会根据给定的启动类初始化应用。

2. **准备环境：**

   在初始化Spring 容器之前，SpringApplication 会准备好相应的环境(Environment),包括命令行参数，系统属性，配置文件（application.properties 或 application.yml）等。

3. **创建`ApplicationContext`：**

   根据配置，`SpringApplication` 会决定使用哪种类型的 `ApplicationContext`。默认情况下，如果是Web应用，会使用`AnnotationConfigServletWebApplicationContext`。如果是非 Web 应用，则使用 `AnnotationConfigApplicationContext`。

4. **初始化上下文(prepareContext):**

   在 `ApplicationContext` 被创建之后，`SpringApplication` 会对其进行初始化，加载 Spring 配置、自动配置类以及应用所需的所有 Bean。

5. **启动 `CommandLineRunner` 和 `ApplicationRunner`**：

   ​	`SpringApplication` 会扫描 `CommandLineRunner` 和 `ApplicationRunner` 接口的实现，并在应用启动时执行它们的 `run` 方法。这对于执行一些启动时需要的逻辑（如初始化数据）非常有用。

6. **启动嵌入式 Web 服务器（如果是 Web 应用）**：

   如果你的应用是一个 Web 应用，Spring Boot 会自动启动一个嵌入式的 Web 服务器（如 Tomcat、Jetty 或 Undertow）。如果没有配置为 Web 应用，Spring Boot 就不会启动 Web 服务器。

7. **应用启动完成**：

   最后，`SpringApplication` 会完成所有初始化工作并启动应用。应用启动后，Spring 容器开始正常工作，处理请求等。



### 五，Springboot配置

#### 	1，ymal语法

​		特别注意空格的配置，有层级关系。

```
person
	name: 秦疆
	age: 18
	
统一格式为:
实体类:
	属性名:空格 属性值
注意：这里的空格是不可少的，不然会报错。
```

配置类要和配置文件绑定加 @ConfigurationProperties 注解



#### **2，JSR303校验**

JSR303（Java Specification Request 303）是 Java 的一个规范，定义了一组注解（Annotations）来验证 Java Bean 对象的属性是否符合特定的规则。这个规范为 Java 提供了标准的 Bean 验证方式，通常用于输入数据验证（如表单输入验证、API 请求体的验证等）。

常见的 JSR 303 注解有：

1. **@NotNull**：字段不能为空。
2. **@Size**：限制字符串、集合、数组等的大小。
3. **@Min** 和 **@Max**：限制数字的最小值和最大值。
4. **@Pattern**：正则表达式验证字符串格式。
5. **@Email**：验证电子邮件地址格式。
6. **@DecimalMin** 和 **@DecimalMax**：限制十进制数字的范围。
7. **@Future** 和 **@Past**：验证日期时间是否是将来或过去。
8. **@AssertTrue** 和 **@AssertFalse**：验证布尔值是否为真或假。
9. **@NotBlank**：验证字符串不能为空白字符。

使用这些注解时，通常需要配合一个验证框架来进行验证。最常用的实现是 **Hibernate Validator**，它是 JSR 303 的参考实现，也支持 JSR 380（Bean Validation 2.0）。



#### 3，SpringBoot多环境配置

在 Spring Boot 中，多环境配置可以通过使用 **profiles** 来实现，不同的环境配置可以通过不同的 **application-{profile}.properties** 或 **application-{profile}.yml** 文件来定义。常见的环境有开发环境、测试环境和生产环境等。

1），**使用 `application.properties` 或 `application.yml`**

Spring Boot 通过 `application.properties` 或 `application.yml` 文件来配置应用。为了支持多环境配置，我们可以在这些文件中根据不同的 profile 定义不同的配置。

a. **配置不同环境的配置文件**

例如：

- `application.properties`：默认配置，通常是通用配置
- `application-dev.properties`：开发环境的配置
- `application-prod.properties`：生产环境的配置

b. **配置多个环境的配置文件示例**

**`application.properties`**（默认配置）

```
spring.datasource.url=jdbc:mysql://localhost:3306/default_db
server.port=8080
```

**`application-dev.properties`**（开发环境配置）

```
spring.datasource.url=jdbc:mysql://localhost:3306/dev_db
server.port=8081
```

**`application-prod.properties`**（生产环境配置）

```
spring.datasource.url=jdbc:mysql://prod-db-server:3306/prod_db
server.port=8080
```

推荐使用ymal语法配置，

使用分隔符的方式配置多个端口，实例如下。

```
# 默认配置
server:
  port: 8081
spring:
  profiles:
    active: dev

---
# 开发环境的配置
server:
  port: 8082
spring:
  profiles:
    active: dev

---
# 测试环境的配置
server:
  port: 8083
spring:
  profiles:
    active: test

```



### 六，Spring Web 开发

jar： webapp！

自动装配

springboot 到底帮我们呢配置了什么？我们能不能进行修改？能修改那些东西？能不能扩展？

- xxxAutoConfiguration...向容器中自动装配组件

- xxxProperties:自动装配类，装配配置文件中自定义的一些内容！

要解决的问题：

- 导入静态资源...
- 首页
- jsp，模板引擎下载 Thymeleaf
- 装配扩展 springMVC
- 增删改查
- 拦截器
- 国际化！



#### 1，静态资源的导入：

在 Spring Boot 中，静态资源的导入很简单，Spring Boot 会自动识别并提供访问静态资源的能力。静态资源通常包括 HTML、CSS、JavaScript 文件、图片等，它们一般放在 `src/main/resources/static` 或 `src/main/resources/public` 目录中。

**1）. 静态资源放置目录**

- **`src/main/resources/static`**：这是 Spring Boot 默认的静态资源目录，任何放在这个目录下的文件都可以通过 `/` 路径访问。
- **`src/main/resources/public`**：同样，Spring Boot 也会自动识别此目录并将其中的静态资源暴露出来。
- **`src/main/resources/resources`**：如果你放在这个目录下，Spring Boot 不会默认处理这些静态资源，需要通过额外的配置来指定访问路径。
- **`src/main/resources/META-INF/resources`**：Spring Boot 也会识别此目录。

**2）. 访问静态资源**

假设你将静态资源放在 `src/main/resources/static` 目录下，如下结构：

```
src/
 └── main/
      └── resources/
           └── static/
                ├── css/
                │    └── style.css
                ├── js/
                │    └── script.js
                └── images/
                     └── logo.png
```

那么，你可以通过以下 URL 访问这些资源：

- **CSS**：`http://localhost:8080/css/style.css`
- **JavaScript**：`http://localhost:8080/js/script.js`
- **图片**：`http://localhost:8080/images/logo.png`

**3）， 配置 Spring Boot 托管静态资源（可选）**

虽然 Spring Boot 默认会处理 `static` 和 `public` 目录中的静态资源，但如果需要自定义静态资源路径，可以通过配置来指定。例如，修改 `application.properties` 文件来更改默认路径：

```
# 设置静态资源路径
spring.resources.static-locations=classpath:/my-static/
```

这样，Spring Boot 将从 `src/main/resources/my-static` 目录中加载静态资源，而不是默认的 `static` 目录。

**4. 启用 WebMvcConfigurer 来定制静态资源**

如果你需要进一步定制静态资源的访问路径或行为，可以实现 `WebMvcConfigurer` 接口并重写 `addResourceHandlers` 方法。

```
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        // 映射静态资源路径
        registry.addResourceHandler("/assets/**")
                .addResourceLocations("classpath:/static/assets/");
    }
}
```

在这个例子中，静态资源目录 `src/main/resources/static/assets` 将会映射到 `http://localhost:8080/assets/` 路径下。

**5. 其他注意事项**

- 静态资源的访问是通过路径 `/static/` 公开的，但可以在 `application.properties` 或 `application.yml` 文件中通过配置更改路径。
- 静态资源通常不需要特别的控制器来处理，Spring Boot 会自动处理。
- 你也可以将静态文件打包到 JAR 或 WAR 中，Spring Boot 会自动解析并提供静态文件服务。



#### 2，首页和图标

**1），首页**

你可以通过控制器映射自定义 `/` 路径，返回 HTML 页面或简单的文本。

**2），图标**

将 `favicon.ico` 文件放在 `src/main/resources/static` 目录下，Spring Boot 会自动识别并暴露它。你还可以在 HTML 中通过 `<link>` 标签来定义 favicon。



#### **3，thymeleaf模版引擎**

结论：只要需要使用 thymleaf 模板引擎，只要导入对应的依赖就可以了，我们将 html 页面放在templates目录下即可。

```
<dependencies>
    <!-- Spring Boot Web Starter -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot Thymeleaf Starter -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <!-- 其他可能需要的依赖 -->
    
	<dependency>
  		<groupId>org.thymeleaf</groupId>
  		<artifactId>thymeleaf</artifactId>
  		<version>3.1.3.RELEASE</version>
  	</dependency>

</dependencies>
```

1>，thymleaf 官网：[https://www.thymeleaf.org/](https://www.thymeleaf.org/)

2>，thymleaf 在Github 的主页 ：[https://github.com/thymeleaf/thymeleaf](https://github.com/thymeleaf/thymeleaf)



#### 4，装配扩展springMVC

**1，springboot 扩展视图解析器：**

**`ViewResolver` 自定义配置**

你可以创建一个 `@Configuration` 类来进一步自定义视图解析器：

```
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
import org.springframework.web.servlet.view.UrlBasedViewResolver;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver;
import org.springframework.web.servlet.view.thymeleaf.ThymeleafViewResolver;

@Configuration
public class ViewResolverConfig {

    // 配置 Thymeleaf 视图解析器
    @Bean
    public ViewResolver thymeleafViewResolver(org.thymeleaf.spring6.SpringTemplateEngine templateEngine) {
        ThymeleafViewResolver resolver = new ThymeleafViewResolver();
        resolver.setTemplateEngine(templateEngine);
        resolver.setOrder(1);  // 设置解析顺序，优先级越高越先解析
        resolver.setCharacterEncoding("UTF-8");
        return resolver;
    }

    // 配置 FreeMarker 视图解析器
    @Bean
    public FreeMarkerViewResolver freeMarkerViewResolver() {
        FreeMarkerViewResolver resolver = new FreeMarkerViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".ftl");
        resolver.setCache(true);
        resolver.setOrder(2);
        return resolver;
    }

    // 配置 JSP 视图解析器
    @Bean
    public InternalResourceViewResolver internalResourceViewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/jsp/");
        resolver.setSuffix(".jsp");
        resolver.setOrder(3);
        return resolver;
    }
}
```



### 七，简单聊一下如何快速的搭建一个网站

​	#1、前端的设计：页面张什么样子：该怎么给数据

​	#2、设计数据库	(数据库设计难点。)

​	#3、前端让他能够自己独立运行，独立化工程。

​	#4、数据接口如何对接：JSON 对象 all in one！

​	#5、前后端联调测试！

1，有一套自己熟悉的后台模板：工作必要！例如 x-admin

2，前端页面：至少自己能通过前端框架，组合出来一个网站页面。一个简单的网站页面至少需要以下几部分构成。

​	-index

​	-about

​	-blog

​	-post

​	-user

3，让这个网站能独立运行！



### 八，Springboot 整合jdbc使用

Spring Boot 中整合 JDBC 相对简单，Spring Boot 提供了对JDBC的良好支持。

**1，添加依赖**

确保你的 pom.xml 文件中添加了和数据库相关的依赖。示例如下：

```
 <dependency>
 		<groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter-jdbc</artifactId>
 </dependency>
```

**2，配置数据源**

在 application.properties 或者 application.yml 中配置数据库连接。示例如下

```
spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC 解决时区的报错时需要加的错误
    url: jdbc:mysql://localhost:3306/mybatis?								       
    serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    #自定义数据源
      
     
```

**3，创建JdbcTemplate**

Spring Boot 已经为你配置好了 JdbcTemplate，你可以在你的sevice类中注入并使用它。JdbcTemplate 是Spring 提供的用于操作数据库的核心类，能够简化JDBC的操作。

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    public void insertUser(String username, String password) {
        String sql = "INSERT INTO users (username, password) VALUES (?, ?)";
        jdbcTemplate.update(sql, username, password);
    }

    public List<User> getAllUsers() {
        String sql = "SELECT * FROM users";
        return jdbcTemplate.query(sql, (rs, rowNum) -> {
            User user = new User();
            user.setId(rs.getLong("id"));
            user.setUsername(rs.getString("username"));
            user.setPassword(rs.getString("password"));
            return user;
        });
    }
}

```

**4，创建User 实体类**

你可以根据数据库中的表结构，创建对应的实体类 User。

```
public class User {
    private Long id;
    private String username;
    private String password;

    // Getters and Setters
}
```

**5,使用 Service 进行操作**

最后，你可以在controller 中调用 Service 进行数据库操作。

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    @PostMapping
    public String createUser(@RequestParam String username, @RequestParam String password) {
        userService.insertUser(username, password);
        return "User created successfully!";
    }

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
}
```

6，启动项目

确保数据库已经出创建并且可以连接spring boot 应用程序，运行Spring Boot 项目后，应该可以通过/Users 路径进行基本的用户操作。



九，整合Druid数据源

Druid 数据源是一个性能高，功能强大的数据数据库连接池，常用来处理数据库丽娜姐的管理。以下是Springboot如何整合Druid数据源的步骤。

1、添加依赖

首先，你需要在 pom.xml 中添加相关的依赖。

```
<!-- Druid 数据源 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.1.22</version> <!-- 版本可以根据需要调整 -->
    </dependency>
```

2、配置Druid 数据源

在 application.properties 或 application.yml 中配置Druid 数据源。以下是 application.properties 的示例配置：

```
# 配置 Druid 数据源
spring.datasource.druid.url=jdbc:mysql://localhost:3306/your_database?useSSL=false&serverTimezone=UTC
spring.datasource.druid.username=root
spring.datasource.druid.password=yourpassword
spring.datasource.druid.driver-class-name=com.mysql.cj.jdbc.Driver

# Druid 配置
spring.datasource.druid.initial-size=5
spring.datasource.druid.min-idle=5
spring.datasource.druid.max-active=20
spring.datasource.druid.max-wait=60000
spring.datasource.druid.validation-query=SELECT 1 FROM DUAL
spring.datasource.druid.test-on-borrow=true
spring.datasource.druid.test-while-idle=true
spring.datasource.druid.time-between-eviction-runs-millis=60000
spring.datasource.druid.min-evictable-idle-time-millis=300000
spring.datasource.druid.pool-prepared-statements=true
spring.datasource.druid.max-open-prepared-statements=20
```

3、配置Druid 数据源Bean

spring Boot 的druid-spring-boot-starter 会自动配置Druid 数据源，但你可以过呢据手动配置跟多细节，或者扩展，例如在你的`@Configuration` 类中，配置 Druid 数据源：如下

```
import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import javax.sql.DataSource;

@Configuration
public class DruidDataSourceConfig {

    @Value("${spring.datasource.druid.url}")
    private String url;

    @Value("${spring.datasource.druid.username}")
    private String username;

    @Value("${spring.datasource.druid.password}")
    private String password;

    @Value("${spring.datasource.druid.driver-class-name}")
    private String driverClassName;

    @Value("${spring.datasource.druid.initial-size}")
    private int initialSize;

    @Value("${spring.datasource.druid.min-idle}")
    private int minIdle;

    @Value("${spring.datasource.druid.max-active}")
    private int maxActive;

    @Value("${spring.datasource.druid.max-wait}")
    private long maxWait;

    @Value("${spring.datasource.druid.validation-query}")
    private String validationQuery;

    @Bean
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        dataSource.setDriverClassName(driverClassName);
        dataSource.setInitialSize(initialSize);
        dataSource.setMinIdle(minIdle);
        dataSource.setMaxActive(maxActive);
        dataSource.setMaxWait(maxWait);
        dataSource.setValidationQuery(validationQuery);
        return dataSource;
    }
}
```



### 九，springboot 整合 mybatis 框架

在sping boot 中整合mybatis 框架，可以实现数据库访问层。轻松的实现数据库操作。

#### 1、创建 springboot 项目，选择以下依赖。

- Spring Web
- MyBatis Framework
- MySQL Driver（如果你使用的是 MySQL 数据库）

或者如果你是使用 Maven 构建，可以手动添加相关依赖。

#### 2. 添加依赖

在 `pom.xml` 文件中，加入如下依赖：

```
<dependencies>
    <!-- Spring Boot Starter Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- MyBatis Spring Boot Starter -->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.2.0</version> <!-- 根据实际情况选择版本 -->
    </dependency>

    <!-- MySQL JDBC Driver -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>

    <!-- Spring Boot Starter Data JPA (Optional) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- Spring Boot Starter Test (Optional for testing) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

#### 3、配置数据源和MyBatis

在 `application.properties` 或 `application.yml` 中配置数据库连接：

```
spring.datasource.url=jdbc:mysql://localhost:3306/your_database_name?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# MyBatis配置
mybatis.mapper-locations=classpath:/mappers/*.xml
mybatis.type-aliases-package=com.example.demo.model
```

- `mybatis.mapper-locations`：指定 MyBatis 映射文件的位置。

- `mybatis.type-aliases-package`：指定 MyBatis 映射对象的包位

#### 4、创建 Model 类（实体类）

```
package com.example.demo.model;

public class User {
    private int id;
    private String name;
    private int age;

    // getters and setters
}
```

#### 5、创建 Mapper 接口

MyBatis 使用接口来定义数据库操作：

```
package com.example.demo.mapper;

import com.example.demo.model.User;
import org.apache.ibatis.annotations.Mapper;
import java.util.List;

@Mapper
public interface UserMapper {
    List<User> findAll();
    User findById(int id);
    void insertUser(User user);
}
```

使用 `@Mapper` 注解标记该接口是一个 MyBatis 映射器。

#### 6、创建 Mapper XML 文件（可选）

你可以使用 XML 文件来定义 SQL 语句（也可以使用注解代替）：

```
<!-- resources/mappers/UserMapper.xml -->
<mapper namespace="com.example.demo.mapper.UserMapper">

    <select id="findAll" resultType="com.example.demo.model.User">
        SELECT * FROM users;
    </select>

    <select id="findById" parameterType="int" resultType="com.example.demo.model.User">
        SELECT * FROM users WHERE id = #{id};
    </select>

    <insert id="insertUser" parameterType="com.example.demo.model.User">
        INSERT INTO users (name, age) VALUES (#{name}, #{age});
    </insert>

</mapper>
```

#### 7. 服务层调用 Mapper

在业务层，你可以通过注入 `UserMapper` 接口来调用数据库操作：

```
package com.example.demo.service;

import com.example.demo.mapper.UserMapper;
import com.example.demo.model.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class UserService {
    
    @Autowired
    private UserMapper userMapper;

    public List<User> getAllUsers() {
        return userMapper.findAll();
    }

    public User getUserById(int id) {
        return userMapper.findById(id);
    }

    public void createUser(User user) {
        userMapper.insertUser(user);
    }
}
```

#### 8.、使用控制器

最后，你可以通过 Spring MVC 创建一个控制器来访问这些服务：

```
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @GetMapping("/{id}")
    public User getUserById(@PathVariable int id) {
        return userService.getUserById(id);
    }

    @PostMapping
    public void createUser(@RequestBody User user) {
        userService.createUser(user);
    }
}
```

#### 9. 运行应用

启动 Spring Boot 应用程序，你可以通过浏览器或者 Postman 访问接口，验证 MyBatis 与 Spring Boot 的集成是否成功。



### 十、spring security 环境搭建

spring security 可以帮助你实现应用的身份认证和授权。spring security 是一个安全框架，提供身份验证，访问控制和攻击防护等功能。搭建步骤如下：

#### 1、创建 spring boot 项目，

首先，使用 [Spring Initializr](https://start.spring.io/) 创建一个 Spring Boot 项目，并选择以下依赖：

- Spring Web
- Spring Security

如果你使用的是 Maven，可以在 `pom.xml` 中手动添加如下依赖：

```
<dependencies>
    <!-- Spring Boot Starter Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Security -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <!-- Spring Boot Starter Thymeleaf (如果需要模板引擎) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <!-- Spring Boot Starter Data JPA (如果需要数据库支持) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- H2 Database (可选，仅用于测试) -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- Spring Boot Starter Test -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

#### 2. 配置 Spring Security

Spring Security 默认会为你的应用提供基础的安全保护，如基于 HTTP 的认证（表单登录）和简单的权限管理。

在 `application.properties` 或 `application.yml` 中，你可以配置一些基本的设置，比如自定义登录页面、用户名和密码等。

```
# 自定义登录页面
spring.security.user.name=user
spring.security.user.password=password
```

#### 3. 配置 SecurityConfig 类

你可以创建一个配置类来定制 Spring Security 的行为，允许你控制身份验证方式、授权规则等。

```
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 定制安全策略
        http
            .authorizeRequests()
                .antMatchers("/", "/home").permitAll()  // 允许访问根目录和/home
                .anyRequest().authenticated()          // 其他请求需要认证
            .and()
            .formLogin()
                .loginPage("/login")                   // 自定义登录页面
                .permitAll()
            .and()
            .logout()
                .permitAll();
    }
}
```

#### 4. 创建登录页面（可选）

如果你想自定义登录页面，可以创建一个 `login.html` 文件，例如使用 Thymeleaf 模板引擎来生成页面。

```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
</head>
<body>
    <h1>Please log in</h1>
    <form action="#" th:action="@{/login}" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" /><br />
        
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" /><br />
        
        <button type="submit">Login</button>
    </form>
</body>
</html>
```

#### 5. 创建控制器

创建一个控制器来处理登录后的请求或未认证的请求。

```
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home() {
        return "home";  // 返回主页
    }

    @GetMapping("/login")
    public String login() {
        return "login";  // 返回登录页面
    }

    @GetMapping("/admin")
    public String admin() {
        return "admin";  // 需要权限才能访问的页面
    }
}
```

#### 6. 配置用户存储（内存用户）

如果你希望使用内存中的用户进行认证（适合开发和测试），可以在 `SecurityConfig` 中配置用户存储：

```
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/", "/home").permitAll()
                .anyRequest().authenticated()
            .and()
            .formLogin()
                .permitAll()
            .and()
            .logout()
                .permitAll();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
            .withUser("user").password("{noop}password").roles("USER")
            .and()
            .withUser("admin").password("{noop}admin").roles("ADMIN");
    }
}
```

#### 7. 使用数据库存储用户（可选）

如果需要使用数据库存储用户信息，可以配置 `JdbcUserDetailsManager` 或 `UserDetailsService` 来连接数据库，并自定义认证过程。

```
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.context.annotation.Bean;
import org.springframework.security.core.userdetails.jdbc.JdbcDaoImpl;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/").permitAll()
                .anyRequest().authenticated()
            .and()
            .formLogin()
                .permitAll()
            .and()
            .logout()
                .permitAll();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService());
    }

    @Bean
    public UserDetailsService userDetailsService() {
        JdbcDaoImpl jdbcDao = new JdbcDaoImpl();
        jdbcDao.setDataSource(dataSource);  // 配置数据源
        return jdbcDao;
    }
}
```

#### 8. 启动应用

启动应用程序后，你可以访问应用的 URL，例如 `http://localhost:8080/`，默认会要求你登录。如果使用内存认证用户，则可以尝试使用 `user/password` 或 `admin/admin` 登录。



### 十一，shiro

Shiro 是一个功能强大的 Java 安全框架，主要用于提供认证、授权、加密、会话管理等安全服务。它使得开发者可以轻松地对 Java 应用程序进行访问控制、身份验证等操作。Shiro 的设计简单且灵活，可以非常方便地集成到各种 Java 应用中，尤其适合需要高度安全性的 web 和企业应用。

#### 主要功能：

1. **认证(Authentication)：**验证用户身份，通常通过用户名和密码。
2. **授权(Authorization)**：控制用户对特定资源的访问权限，例如角色和权限管理。
3. **会话管理(Session Management)：**管理用户会话，在无状态的 Web 环境中保持用户状态。
4. **加密(Cryptography)**:提供密码和数据加密等安全功能。
5. **Web 支持**: 提供了 Web 环境下的特定支持，能简化 Web 安全控制。

#### **使用shiro 的基本步骤**

**1、添加依赖**

如果使用Maven，就可以添加以下依赖：

```
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.10.1</version> <!-- 请检查最新版本 -->
</dependency>
```

**2、配置 SecurityManager**

在 Shiro 中，Security 是中心组件，负责管理安全操作。通常你需要创建一个 SecurityManager 配置类。

```
@Configuration
public class ShiroConfig {

    @Bean
    public SecurityManager securityManager() {
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        securityManager.setRealm(myRealm());  // 设置自定义 Realm
        return securityManager;
    }

    @Bean
    public Realm myRealm() {
        MyCustomRealm realm = new MyCustomRealm();
        return realm;
    }
}
```

**3、创建自定义 Realm**

Shiro 通过 `Realm` 来完成身份验证和授权。你可以继承 `AuthorizingRealm` 来创建自定义的 Realm。

```
public class MyCustomRealm extends AuthorizingRealm {
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        // 返回用户的权限和角色
        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
        info.addRole("admin");
        info.addStringPermission("user:create");
        return info;
    }

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        // 通过用户名密码进行身份验证
        String username = (String) token.getPrincipal();
        // 假设从数据库中查询用户密码
        String password = "password123"; // 假数据

        return new SimpleAuthenticationInfo(username, password, getName());
    }
}
```

**4、登录与授权**

在 Shiro 中，登录通常通过 `Subject` 来进行，使用 `SecurityManager` 来管理。

```
Subject currentUser = SecurityUtils.getSubject();
UsernamePasswordToken token = new UsernamePasswordToken("username", "password");

try {
    currentUser.login(token);
    // 登录成功
} catch (AuthenticationException e) {
    // 登录失败
}
```

**5、配置Shiro filter**

在 Web 应用中，你通常会使用 `ShiroFilter` 来处理 URL 权限控制。可以在 `web.xml` 或者 Spring 配置类中配置。

```
<filter>
    <filter-name>shiroFilter</filter-name>
    <filter-class>org.apache.shiro.web.filter.authc.FormAuthenticationFilter</filter-class>
</filter>
```

**6、使用 Shiro 标签**

如果你是在 JSP 页面中使用，可以通过 Shiro 标签来控制页面访问权限。

```
<shiro:authenticating>
    <p>用户已经登录</p>
</shiro:authenticating>
```

**总结**

Shiro 提供了丰富的安全功能，但配置起来相对简单，适用于各种类型的 Java 应用。你可以根据需求定制自己的 Realm、认证和授权逻辑，也可以通过简单的配置和注解来快速集成安全功能。



### 十二、swagger 介绍及集成

**Swagger** 是一个用于生成、描述、调用和可视化 RESTful API 的工具集。它通过开放API规范（OpenAPI Specification, OAS）提供了一种标准的方式来描述API，允许开发者、测试人员和用户理解、使用和测试 API，而无需阅读大量文档。

Swagger 包括多个工具和库，如：

- **Swagger UI**：提供一个交互式的用户界面，用于浏览和测试 API。
- **Swagger Codegen**：用于生成服务器端和客户端代码。
- **Swagger Editor**：基于浏览器的工具，用于编辑 API 文档并生成 OpenAPI 规范。
- **Swagger UI**：可以在浏览器中查看并交互式地使用 API。
- **Swagger Annotations**：用于在 Java 代码中嵌入 API 的元数据。

#### Swagger 的优点：

1. **标准化 API 文档**：Swagger 提供了开放的标准（OpenAPI Specification），可以有效地描述 API。
2. **交互式文档**：通过 Swagger UI，开发人员和用户可以直接在浏览器中测试 API，减少了文档和代码之间的差距。
3. **代码生成**：Swagger Codegen 可以生成服务器端、客户端的代码，减少了重复的工作。
4. **易于集成**：Swagger 提供了多种语言和框架的支持，方便与现有的项目集成。

#### swagger 集成步骤

**1、添加依赖**

```
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version> <!-- 可以根据实际需要更新版本 -->
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version> <!-- 可以根据实际需要更新版本 -->
</dependency>
```

**2、配置 Swagger**

在 Spring Boot 项目中配置 Swagger，可以创建一个配置类来启用 Swagger。

```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;

@Configuration
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.controller")) // 指定扫描的包路径
                .paths(PathSelectors.any())  // 扫描所有的API接口
                .build();
    }
}
```

- `RequestHandlerSelectors.basePackage("com.example.controller")`: 用于指定扫描控制器的包路径。

- `PathSelectors.any()`: 表示扫描所有的 API 路径。

**3、启用Swagger 注解**

在控制器类和方法中，可以使用Swagger 提供的注解来描述API，增强文档的可读性。常见的Swagger注解:

- `@Api`：用于描述类，通常用于控制器类。

- `@ApiOperation`：描述一个操作方法（API）。

- `@ApiParam`：描述方法参数。

- `@ApiResponse`：描述 API 的响应信息。

```
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import io.swagger.annotations.ApiParam;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@Api(value = "User Controller", tags = "用户管理相关API")
@RestController
public class UserController {

    @ApiOperation(value = "获取用户列表", notes = "返回所有用户的详细信息")
    @GetMapping("/users")
    public List<User> getUsers() {
        return userService.getAllUsers();
    }

    @ApiOperation(value = "根据ID获取用户", notes = "根据用户ID查询用户")
    @GetMapping("/users/{id}")
    public User getUserById(@ApiParam(value = "用户ID", required = true) @PathVariable Long id) {
        return userService.getUserById(id);
    }
}
```

**4、访问 Swagger UI**

启动 Spring Boot 应用后，Swagger UI 将默认部署在 `http://localhost:8080/swagger-ui.html` 路径下。在该页面上，可以查看 API 文档，并进行交互式的测试。



### 十三、springboot 集成 redis

在Spring boot 项目中集成Redis, 可以利用 Spring Data Redis 提供的功能，简化Redis 的操作。Spring Data Redis 是一个用于与 Redis 交互的框架，他提供了对 Redis 的自动配置i支持。

以下是Redis 集成到 Spring Boot 项目的步骤：

#### 1、添加 Redis 相关依赖

先，在 `pom.xml` 文件中添加 Spring Boot 和 Redis 相关的依赖。

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-logging</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>2.8.0</version> <!-- 可以根据需要选择版本 -->
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.8</version> <!-- 如果需要，修改为合适的版本 -->
</dependency>
```

#### **2、配置Redis 连接**

在 `application.properties` 或 `application.yml` 中配置 Redis 的连接信息。

**使用** `application.properties` **配置：**

```
spring.redis.host=localhost
spring.redis.port=6379
spring.redis.password=your_password  # 如果有密码的话
spring.redis.database=0  # 使用 Redis 的哪个数据库，默认为 0
spring.redis.timeout=2000  # 设置连接超时时间，单位毫秒
```

**使用 `application.yml` 配置：**

```
spring:
  redis:
    host: localhost
    port: 6379
    password: your_password  # 如果有密码的话
    database: 0  # 默认数据库
    timeout: 2000  # 连接超时时间（毫秒）
```

#### 3、配置 RedisTemplate（可选）

`RedisTemplate` 是 Spring Data Redis 提供的一个类，允许你通过 Java 类与 Redis 进行交互。你可以通过它进行操作 Redis 中的各种数据类型（如 String、List、Hash、Set 等）。

创建一个配置类，定义 `RedisTemplate` 和 `StringRedisTemplate`。

```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {

    @Bean
    public JedisConnectionFactory jedisConnectionFactory() {
        JedisConnectionFactory factory = new JedisConnectionFactory();
        factory.setHostName("localhost");
        factory.setPort(6379);
        return factory;
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(jedisConnectionFactory());
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        return template;
    }

    @Bean
    public StringRedisTemplate stringRedisTemplate() {
        StringRedisTemplate template = new StringRedisTemplate();
        template.setConnectionFactory(jedisConnectionFactory());
        return template;
    }
}
```

- `RedisTemplate`：用于处理各种类型的 Redis 数据结构。

- `StringRedisTemplate`：主要用于操作字符串类型的数据，通常用于简化操作。

#### 4、使用 Redis

一旦配置完成，你就可以在 Spring Boot 中通过 `RedisTemplate` 或 `StringRedisTemplate` 来进行 Redis 操作了

**1）、使用 `StringRedisTemplate` 进行字符串操作：**

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Service;

@Service
public class RedisService {

    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    public void setValue(String key, String value) {
        stringRedisTemplate.opsForValue().set(key, value);
    }

    public String getValue(String key) {
        return stringRedisTemplate.opsForValue().get(key);
    }
}
```

**2）、使用 `RedisTemplate` 进行更复杂的数据类型操作：**

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;

@Service
public class RedisService {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    // 设置 Hash 数据
    public void setHash(String hashKey, String key, String value) {
        redisTemplate.opsForHash().put(hashKey, key, value);
    }

    // 获取 Hash 数据
    public Object getHash(String hashKey, String key) {
        return redisTemplate.opsForHash().get(hashKey, key);
    }

    // 设置 List 数据
    public void setList(String listKey, Object value) {
        redisTemplate.opsForList().leftPush(listKey, value);
    }

    // 获取 List 数据
    public Object getList(String listKey) {
        return redisTemplate.opsForList().leftPop(listKey);
    }
}
```

#### 5、使用Redis缓存

Spring Boot 可以与 Redis 集成，作为缓存解决方案。你可以使用 `@Cacheable` 和 `@CachePut` 注解来实现缓存功能。

首先，启用缓存支持，在启动类或者配置类中添加 `@EnableCaching` 注解。

```
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@EnableCaching  // 启用缓存
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

然后，你可以在方法上使用 `@Cacheable` 和 `@CachePut` 注解：

```
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Cacheable(value = "users", key = "#id")
    public User getUserById(Long id) {
        // 模拟从数据库中查询用户
        return new User(id, "John Doe");
    }
}
```

- `@Cacheable(value = "users", key = "#id")`：缓存 `getUserById` 方法的结果，缓存的键为 `id`，缓存的值是返回的 `User` 对象。

- `value`：缓存名称。

- `key`：缓存的键，可以是方法参数。

#### 6、使用 Redis 作为消息队列（可选）

你还可以将 Redis 用作消息队列，通过 Redis 发布/订阅功能实现异步消息处理。

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.listener.ChannelTopic;
import org.springframework.data.redis.listener.MessageListener;
import org.springframework.data.redis.listener.MessageListenerContainer;
import org.springframework.data.redis.listener.RedisMessageListenerContainer;
import org.springframework.data.redis.connection.Message;

@Configuration
public class RedisMessageConfig {

    @Autowired
    private RedisTemplate<String, String> redisTemplate;

    @Bean
    public MessageListenerContainer messageListenerContainer(RedisConnectionFactory redisConnectionFactory, MessageListener messageListener) {
        RedisMessageListenerContainer container = new RedisMessageListenerContainer();
        container.setConnectionFactory(redisConnectionFactory);
        container.addMessageListener(messageListener, new ChannelTopic("myChannel"));
        return container;
    }

    @Bean
    public MessageListener messageListener() {
        return new MessageListener() {
            @Override
            public void onMessage(Message message, byte[] pattern) {
                System.out.println("Received: " + message.toString());
            }
        };
    }
}
```

#### 7、启动Redis 服务

确保 Redis 服务已启动并正在监听默认的 6379 端口。你可以使用以下命令启动 Redis：

```
# 启动 Redis 服务
redis-server
```

**总结**

通过以上步骤，你可以轻松地将 Redis 集成到 Spring Boot 项目中，并利用 Spring Data Redis 提供的功能进行 Redis 操作。你可以使用 Redis 来存储缓存数据、消息队列、会话数据，甚至用来管理复杂的数据结构。