---
title: Mybatis黑马笔记
date: 2025-03-11 19:06:54
tags:
---

## MyBatis  黑马课程对应资料



## 1 Mybatis概述

### 1.1 Mybatis概念

> - MyBatis 是一款优秀的持久层框架，用于简化 JDBC 开发
> - MyBatis 本是 Apache 的一个[开源](https://edu.csdn.net/cloud/pm_summit?utm_source=blogglc&spm=1001.2101.3001.7020)项目[iBatis](https://so.csdn.net/so/search?q=iBatis&spm=1001.2101.3001.7020), 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。2013年11月迁移到Github
> - 官网：https://mybatis.org/mybatis-3/zh/index.html

**持久层：**

- 负责将数据到保存到数据库的那一层代码。

  以后开发我们会将操作数据库的Java代码作为[持久层](https://so.csdn.net/so/search?q=持久层&spm=1001.2101.3001.7020)。而Mybatis就是对jdbc代码进行了封装。

- JavaEE三层架构：表现层、业务层、持久层

  三层架构在后期会给大家进行讲解，今天先简单的了解下即可。

**框架：**

- 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
- 在框架的基础之上构建软件编写更加高效、规范、通用、可扩展

举例给大家简单的解释一下什么是半成品[软件](https://marketing.csdn.net/p/3127db09a98e0723b83b2914d9256174?pId=2782&utm_source=glcblog&spm=1001.2101.3001.7020)。大家小时候应该在公园见过给石膏娃娃涂鸦

![image-20230309162158161](https://i-blog.csdnimg.cn/blog_migrate/af413bb490d48a311d45788aa14d037e.png)

如下图所示有一个石膏娃娃，这个就是一个半成品。你可以在这个半成品的基础上进行不同颜色的涂鸦

![image-20230309162259236](https://i-blog.csdnimg.cn/blog_migrate/0741a309a828f1ba54221551cf66078d.png)

了解了什么是Mybatis后，接下来说说以前 `JDBC代码` 的缺点以及Mybatis又是如何解决的。

### 1.2 JDBC 缺点

下面是 JDBC 代码，我们通过该代码分析都存在什么缺点：

![image-20230309162305057](https://i-blog.csdnimg.cn/blog_migrate/fafd07aecf2c1252f945731d7deb31bf.png)

- 硬编码

    - 注册驱动、获取连接

      上图标1的代码有很多字符串，而这些是连接数据库的四个基本信息，以后如果要将Mysql数据库换成其他的关系型数据库的话，这四个地方都需要修改，如果放在此处就意味着要修改我们的源代码。

    - SQL语句

      上图标2的代码。如果表结构发生变化，SQL语句就要进行更改。这也不方便后期的维护。

- 操作繁琐

    - 手动设置参数

    - 手动封装结果集

      上图标4的代码是对查询到的数据进行封装，而这部分代码是没有什么技术含量，而且特别耗费时间的。

### 1.3 Mybatis 优化

- 硬编码可以配置到配置文件
- 操作繁琐的地方mybatis都自动完成

如图所示

![image-20230309195035122](https://i-blog.csdnimg.cn/blog_migrate/9a45d8dba75fcde4d11427d0fec1e467.png)

下图是持久层框架的使用占比。

![image-20230309195041713](https://i-blog.csdnimg.cn/blog_migrate/23fd2d0864d8f82420302c96e0c08100.png)

## 2 Mybatis快速入门

**需求：查询user表中所有的数据**

- 创建user表，添加数据

  ```sql
  create database mybatis;
  use mybatis;
  
  drop table if exists tb_user;
  
  create table tb_user(
  	id int primary key auto_increment,
  	username varchar(20),
  	password varchar(20),
  	gender char(1),
  	addr varchar(30)
  );
  
  INSERT INTO tb_user VALUES (1, 'zhangsan', '123', '男', '北京');
  INSERT INTO tb_user VALUES (2, '李四', '234', '女', '天津');
  INSERT INTO tb_user VALUES (3, '王五', '11', '男', '西安');
  12345678910111213141516
  ```

- 创建模块，导入坐标

  在创建好的模块中的 pom.xml 配置文件中添加依赖的坐标

  ```xml
  <dependencies>
      <!--mybatis 依赖-->
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.5</version>
      </dependency>
  
      <!--mysql 驱动-->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.46</version>
      </dependency>
  
      <!--junit 单元测试-->
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.13</version>
          <scope>test</scope>
      </dependency>
  
      <!-- 添加slf4j日志api -->
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-api</artifactId>
          <version>1.7.20</version>
      </dependency>
      <!-- 添加logback-classic依赖 -->
      <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-classic</artifactId>
          <version>1.2.3</version>
      </dependency>
      <!-- 添加logback-core依赖 -->
      <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-core</artifactId>
          <version>1.2.3</version>
      </dependency>
  </dependencies>
  123456789101112131415161718192021222324252627282930313233343536373839404142
  ```

  注意：需要在项目的 resources 目录下创建logback的配置文件

- 编写 MyBatis 核心配置文件 – > 替换连接信息 解决硬编码问题

  在模块下的 resources 目录下创建mybatis的配置文件 `mybatis-config.xml`，内容如下：

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
  
      <typeAliases>
          <package name="com.itheima.pojo"/>
      </typeAliases>
      
      <!--
      environments：配置数据库连接环境信息。可以配置多个environment，通过default属性切换不同的environment
      -->
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <!--数据库连接信息-->
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql:///mybatis?useSSL=false"/>
                  <property name="username" value="root"/>
                  <property name="password" value="1234"/>
              </dataSource>
          </environment>
  
          <environment id="test">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <!--数据库连接信息-->
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql:///mybatis?useSSL=false"/>
                  <property name="username" value="root"/>
                  <property name="password" value="1234"/>
              </dataSource>
          </environment>
      </environments>
      <mappers>
         <!--加载sql映射文件-->
         <mapper resource="UserMapper.xml"/>
      </mappers>
  </configuration>
  1234567891011121314151617181920212223242526272829303132333435363738394041
  ```

- 编写 SQL 映射文件 --> 统一管理sql语句，解决硬编码问题

  在模块的 `resources` 目录下创建映射配置文件 `UserMapper.xml`，内容如下：

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="test">
      <select id="selectAll" resultType="com.itheima.pojo.User">
          select * from tb_user;
      </select>
  </mapper>
  1234567
  ```

- 编码

    - 在 `com.itheima.pojo` 包下创建 User类

      ```java
      public class User {
          private int id;
          private String username;
          private String password;
          private String gender;
          private String addr;
          
          //省略了 setter 和 getter
      }
      123456789
      ```

    - 在 `com.itheima` 包下编写 MybatisDemo 测试类

      ```java
      public class MyBatisDemo {
      
          public static void main(String[] args) throws IOException {
              //1. 加载mybatis的核心配置文件，获取 SqlSessionFactory
              String resource = "mybatis-config.xml";
              InputStream inputStream = Resources.getResourceAsStream(resource);
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
      
              //2. 获取SqlSession对象，用它来执行sql
              SqlSession sqlSession = sqlSessionFactory.openSession();
              //3. 执行sql
              List<User> users = sqlSession.selectList("test.selectAll"); //参数是一个字符串，该字符串必须是映射配置文件的namespace.id
              System.out.println(users);
              //4. 释放资源
              sqlSession.close();
          }
      }
      1234567891011121314151617
      ```

**解决SQL映射文件的警告提示：**

在入门案例映射配置文件中存在报红的情况。问题如下：

![image-20230309195053881](https://i-blog.csdnimg.cn/blog_migrate/a3e11d23cff7b2e057f379060068b347.png)

- 产生的原因：Idea和数据库没有建立连接，不识别表信息。但是大家一定要记住，它并不影响程序的执行。
- 解决方式：在Idea中配置MySQL数据库连接。

IDEA中配置MySQL数据库连接

- 点击IDEA右边框的 `Database` ，在展开的界面点击 `+` 选择 `Data Source` ，再选择 `MySQL`

![img](https://i-blog.csdnimg.cn/blog_migrate/01698e805a56ef538d2a1c712118f579.png)

- 在弹出的界面进行基本信息的填写

  ![image-20230309195122696](https://i-blog.csdnimg.cn/blog_migrate/da5146195ba7813f7a401a06774f16c1.png)

- 点击完成后就能看到如下界面

![img](https://i-blog.csdnimg.cn/blog_migrate/1c68d536d89450bd49cf9082bc684d21.png)

而此界面就和 `navicat` 工具一样可以进行数据库的操作。也可以编写SQL语句

![image-20230309195141378](https://i-blog.csdnimg.cn/blog_migrate/72d766e731838b90b0fb049126f26810.png)

## 3 Mapper代理开发

### 3.1 Mapper代理开发概述

之前我们写的代码是基本使用方式，它也存在硬编码的问题，如下：

![image-20230309195212513](https://i-blog.csdnimg.cn/blog_migrate/ff77431f2282e63d12c97391ddb16a75.png)

这里调用 `selectList()` 方法传递的参数是映射配置文件中的 namespace.id值。这样写也不便于后期的维护。如果使用 Mapper 代理方式（如下图）则不存在硬编码问题。

![image-20230309195239248](https://i-blog.csdnimg.cn/blog_migrate/3e0bf955c582d5681e95c815adb0f352.png)

通过上面的描述可以看出 Mapper 代理方式的目的：

- 解决原生方式中的硬编码
- 简化后期执行SQL

Mybatis 官网也是推荐使用 Mapper 代理的方式。下图是截止官网的图片

![image-20210726215339568](https://i-blog.csdnimg.cn/blog_migrate/b91ded925b305bb5515cc037229d3277.png)

### 3.2 使用Mapper代理要求

使用Mapper代理方式，必须满足以下要求：

- 定义与SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下。如下图：

  ![img](https://i-blog.csdnimg.cn/blog_migrate/40fc9f8f9d37bf099e1fffe40ff11591.png)

- 设置SQL映射文件的namespace属性为Mapper接口全限定名

  ![image-20230309195515116](https://i-blog.csdnimg.cn/blog_migrate/45ba5c1db15f9917479fdf165423bfda.png)

- 在 Mapper 接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致

  ![img](https://i-blog.csdnimg.cn/blog_migrate/cba3652e42bb0c456bd4a8839bb518d9.png)

### 3.3 案例代码实现

- 在 `com.itheima.mapper` 包下创建 UserMapper接口，代码如下：

  ```java
  public interface UserMapper {
      List<User> selectAll();
      User selectById(int id);
  }
  1234
  ```

- 在 `resources` 下创建 `com/itheima/mapper` 目录，并在该目录下创建 UserMapper.xml 映射配置文件

  ```xml
  <!--
      namespace:名称空间。必须是对应接口的全限定名
  -->
  <mapper namespace="com.itheima.mapper.UserMapper">
      <select id="selectAll" resultType="com.itheima.pojo.User">
          select *
          from tb_user;
      </select>
  </mapper>
  123456789
  ```

- 在 `com.itheima` 包下创建 MybatisDemo2 测试类，代码如下：

  ```java
  /**
   * Mybatis 代理开发
   */
  public class MyBatisDemo2 {
  
      public static void main(String[] args) throws IOException {
  
          //1. 加载mybatis的核心配置文件，获取 SqlSessionFactory
          String resource = "mybatis-config.xml";
          InputStream inputStream = Resources.getResourceAsStream(resource);
          SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
  
          //2. 获取SqlSession对象，用它来执行sql
          SqlSession sqlSession = sqlSessionFactory.openSession();
          //3. 执行sql
          //3.1 获取UserMapper接口的代理对象
          UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
          List<User> users = userMapper.selectAll();
  
          System.out.println(users);
          //4. 释放资源
          sqlSession.close();
      }
  }
  ```

注意：

如果Mapper接口名称和SQL映射文件名称相同，并在同一目录下，则可以使用包扫描的方式简化SQL映射文件的加载。也就是将核心配置文件的加载映射配置文件的配置修改为

```xml
<mappers>
    <!--加载sql映射文件-->
    <!-- <mapper resource="com/itheima/mapper/UserMapper.xml"/>-->
    <!--Mapper代理方式-->
    <package name="com.itheima.mapper"/>
</mappers>
```

## 4 核心配置文件

核心配置文件中现有的配置之前已经给大家进行了解释，而核心配置文件中还可以配置很多内容。我们可以通过查询官网看可以配置的内容

![img](https://i-blog.csdnimg.cn/blog_migrate/196c807e5d8a75af97e2cf3c26e343f5.png)

接下来我们先对里面的一些配置进行讲解。

### 4.1 多环境配置

在核心配置文件的 `environments` 标签中其实是可以配置多个 `environment` ，使用 `id` 给每段环境起名，在 `environments` 中使用 `default='环境id'` 来指定使用哪儿段配置。我们一般就配置一个 `environment` 即可。

```xml
<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <!--数据库连接信息-->
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql:///mybatis?useSSL=false"/>
            <property name="username" value="root"/>
            <property name="password" value="1234"/>
        </dataSource>
    </environment>

    <environment id="test">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <!--数据库连接信息-->
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql:///mybatis?useSSL=false"/>
            <property name="username" value="root"/>
            <property name="password" value="1234"/>
        </dataSource>
    </environment>
</environments>
```

### 4.2 类型别名

在映射配置文件中的 `resultType` 属性需要配置数据封装的类型（类的全限定名）。而每次这样写是特别麻烦的，Mybatis 提供了 `类型别名`(typeAliases) 可以简化这部分的书写。

首先需要现在核心配置文件中配置类型别名，也就意味着给pojo包下所有的类起了别名（别名就是类名），不区分大小写。内容如下：

```xml
<typeAliases>
    <!--name属性的值是实体类所在包-->
    <package name="com.itheima.pojo"/> 
</typeAliases>
```

通过上述的配置，我们就可以简化映射配置文件中 `resultType` 属性值的编写

```xml
<mapper namespace="com.itheima.mapper.UserMapper">
    <select id="selectAll" resultType="user">
        select * from tb_user;
    </select>
</mapper>
```