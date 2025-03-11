---
title: 狂神spring5
date: 2025-03-11 19:14:04
tags:
---


### 目录标题

1. Spring
   1.1 简介
   1.2 优点
   1.3 组成
   1.4 拓展
2. IOC理论推导
3. HelloSpring
4. IOC创建对象的方式
5. Spring配置
   5.1 取别名
   5.2 Bean的配置
   5.3 import
6. 依赖注入
   6.1 构造器注入
   6.2 Set方式注入【重点】
   6.3 拓展方式注入
   6.4 Bean作用域
7. Bean的自动装配
   7.1 测试
   7.2 ByName自动装配
   7.3 ByType自动装配
   7.4 使用注解实现自动装配
8. 使用注解开发
9. 用JAVA的方式配置Spring
10. 代理模式
    10.1 静态代理
    10.2 加深理解
    10.3 动态代理
11. AOP
    11.1 什么是AOP
    11.2 AOP在Spring中的作用
    11.3 使用Spring实现AOP
12. 整合Mybatis
    12.1 回忆Mybatis
    12.2 Mybatis-Spring
13. 声明式事务
    13.1 回顾事务
    13.2 Spring中的事务管理
14. 总结



## 1.Spring

### **1.1 简介**

SSH：Struct2+Spring+Hibernate
SSM：SpringMVC+Spring+Mybatis
官网：https://spring.io/projects/spring-framework#overview
官方下载地址：https://repo.spring.io/release/org/springframework/spring/
GitHub：https://github.com/spring-projects/spring-framework

```
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>
```

### 1.2 优点

- Spring是一个开源的免费的框架（容器）。
- Spring是一个轻量级、非侵入式的框架。
- 控制反转（IOC）、面向切面编程（AOP)。
- 支持事务的处理，对框架整合的支持。

**总结：Spring是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架。**

### 1.3 组成

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d45b676acb3840fe4c62e82e6e5901c4.png)

### 1.4 拓展

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d52256d9af48158ba33e5ec7b04895ea.png)
**Spring Boot：**

- 一个快速开发的脚手架。
- 基于SpringBoot可以快速的开发单个微服务。
- 约定大于配置。

**Spring Cloud：**

- SpringCloud是基于SpringBoot实现。

## 2. IOC理论推导

在之前的业务中，用户的需求可能会影响原来的代码，需要根据需求修改相应的代码。如果程序代码量很大，修改的成本十分昂贵。

**可使用set接口实现：**

```
public class UserServiceImpl implements UserService{

    private UserDao userDao ;

    //利用set进行动态实现值的注入
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void getUser(){
        userDao.getUser();
    }
}
```

- 之前，程序是主动创建对象，控制权在程序员的手上。
- 使用了set注入后，程序不再具有主动性，变成了被动的接受对象。

```
public class MyTest {
    public static void main(String[] args) {
        //用户实际调用的是业务层，dao层他们不需要接触。
        UserService userService = new UserServiceImpl();
        
        System.out.println($END$);
		//((UserServiceImpl) userService).setUserDao(new UserDaoImpl());

        userService.getUser();
    }
}
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/aee73111fd5a9699ca5484ea71aa1a83.png)
**IOC本质：**

**控制反转IoC（Inversion of Control）**，是一种设计思想，DI（依赖注入）是实现IoC的一种方法，也有人认为DI只是IoC的另一种说法。没有IoC的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection，DI）。**

## 3.HelloSpring

1、新建一个maven项目，编写实体类

```
public class Hello {
    private String str;

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }

    @Override
    public String toString() {
        return "Hello{" +
                "str='" + str + '\'' +
                '}';
    }
}
```

2、编写bean.xml配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--使用Spring来创建对象，在Spring这些都称为Bean
    类型 变量名 = new 类型();
    Hello hello = new Hello();

    id = 变量名
    class = new的对象
    property 相当于给对象中的属性设置一个值！
        -->
    <bean id="hello" class="com.kuang.pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>
</beans>
```

3、测试

```
public class MyTest {
    public static void main(String[] args) {
        //获取Spring的上下文对象
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象现在都在Spring中的管理了，我们需要使用，直接去里面取出来就可以！
        Hello hello = (Hello) context.getBean("hello");
        System.out.println(hello.toString());
    }
}
```

思考问题：

- Hello对象是谁创建的？


Hello对象是由Spring创建的。

- Hello对象的属性是怎么设置的？

Hello对象的属性是由Spring容器设置的。

**控制：** 谁来控制对象的创建，传统应用程序的对象是由程序本身控制创建的，使用Spring后，对象是由Spring来创建的。

**反转：** 程序本身不创建对象，而变成被动的接收对象。

**依赖注入：** 就是利用set方法来进行注入的。

**IOC是一种编程思想，由主动的编程变成被动的接收。**

到了现在，我们彻底不用在程序中去改动了，要实现不同的操作，只需要在xml配置文件中进行修改，所谓的IOC，一句话搞定：对象由Spring来创建，管理，装配！

## 4. IOC创建对象的方式

1、使用无参构造创建对象，默认。

```
    <bean id="user" class="com.kuang.pojo.User">
        <property name="name" value="houmou"/>
    </bean>
```

2、假设要使用有参构造创建对象。

方式一：下标赋值

```
<!--第一种方式：下标赋值    -->
<bean id="user" class="com.kuang.pojo.User">
    <constructor-arg index="0" value="狂神说Java"/>
</bean>
```

方式二：类型

```
<!--第二种方式：通过类型的创建，不建议使用    -->
<bean id="user" class="com.kuang.pojo.User">
    <constructor-arg type="java.lang.String" value="lifa"/>
</bean>
```

方式三：参数名

```
   <bean id="user" class="com.kuang.pojo.User">
        <constructor-arg name="name" value="wangfang"/>
   </bean>
```

**总结：在配置文件加载的时候，容器中管理的对象就已经初始化了！**



## 5. [Spring配置](https://so.csdn.net/so/search?q=Spring配置&spm=1001.2101.3001.7020)

### 5.1 取别名

```
    <!--别名，如果添加了别名，我们也可以使用别名获取到这个对象-->
    <alias name="user" alias="userNew"/>
```

### 5.2 Bean的配置

    <!--
    id：bean的唯一标识符，也就是相当于我们学的对象名
    class：bean对象所对应的全限定名：包名+类名
    name：也是别名，而且name可以同时取多个别名
        -->
    <bean id="userT" class="com.kuang.pojo.UserT" name="user2 u2,u3;u4">
        <property name="name" value="狂神"/>
    </bean>

5.3 import
这个import。一般用于团队开发使用，它可以将多个配置文件，导入合并为一个。
假设，现在项目中有多个人开发，这三个人负责不同的类开发，不同的类需要注册在不同的bean中，我们可以利用import将所有人的beans.xml合并为一个总的！

**applicationContext.xml**

```xml
<import resource="bean.xml"/>
<import resource="bean2.xml"/>
<import resource="bean3.xml"/>
```

使用的时候，直接使用总的配置就可以了。


## 6. 依赖注入

依赖注入简单来说就是给类中的属性赋值。

### 6.1 构造器注入

**前面已经介绍过，参考4、IOC创建对象的方式**

### 6.2 Set方式注入【重点】

**依赖注入：Set注入**

- 依赖：bean对象的创建依赖于容器。
- 注入：bean对象中的所有属性，由容器来注入。

【环境搭建】

1、编写实体类

```
package com.kuang.pojo;

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

```
package com.kuang.pojo;

import java.util.*;

@Date
public class Student {

    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> games;
    private Properties info;
    private String wife;
    
}
```

2、配置beans.xml文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:jdbc="http://www.springframework.org/schema/jdbc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc.xsd">

    <bean id="address" class="com.kuang.pojo.Address">
        <property name="address" value="河南"/>
    </bean>
    
    <bean id="student" class="com.kuang.pojo.Student">
        <!--    第一种注入：普通值注入，value-->
        <property name="name" value="houshuaixin"/>

        <!--    第二种注入：bean注入，ref-->
        <property name="address" ref="address"/>

        <!--    第三种注入：数组的注入-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>

        <!--    list注入-->
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>看电影</value>
            </list>
        </property>

        <!--    Map注入-->
        <property name="card">
            <map>
                <entry key="身份证" value="1111111"/>
                <entry key="银行卡" value="2222222"/>
            </map>
        </property>


        <!--    Set注入-->
        <property name="games">
            <set>
                <value>CF</value>
                <value>LOL</value>
                <value>BOB</value>
            </set>
        </property>

        <!--    null注入-->
        <property name="wife">
            <null></null>
        </property>

        <!--    Properties注入-->
        <property name="info">
            <props>
                <prop key="学号">215151</prop>
                <prop key="性别">男</prop>
            </props>
        </property>

    </bean>
    
</beans>
```

3、测试

```
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student)context.getBean("student");
        System.out.println(student.toString());
    }
```

### 6.3 拓展方式注入

可以使用p命名空间和c命名空间进行注入，但需要导入xml约束：

       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"

beans.xml配置文件：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:jdbc="http://www.springframework.org/schema/jdbc"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc.xsd">

    <!--P命名空间注入，可以直接注入属性的值:property-->
    <bean id="user" class="com.kuang.pojo.User" p:name="狂神" p:age="32"/>

    <!--C命名空间注入，通过构造器注入:construct-args-->
    <bean id="user2" class="com.kuang.pojo.User" c:age="18" c:name="狂神"/>
    
</beans>
```

测试：

```
    @Test
    public void test1(){
        ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
        User user = context.getBean("user2", User.class); //此方式不用再强转
        System.out.println(user);
    }
```

### 6.4 Bean作用域

1、单例模式（Spring默认机制）

```
<bean id="user2" class="com.kuang.pojo.User" c:age="18" c:name="狂神" 
scope="singleton"/>
```

```
        ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
        User user1 = context.getBean("user2", User.class);
        User user2 = context.getBean("user2", User.class);
        System.out.println(user1 == user2); //ture
```

2、原型模式：每次从容器中get的时候，都会产生一个新的对象。

```xml
<bean id="user2" class="com.kuang.pojo.User" c:age="18" c:name="狂神" 
scope="prototype"/>
```

```
        ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
        User user1 = context.getBean("user2", User.class);
        User user2 = context.getBean("user2", User.class);
        System.out.println(user1 == user2); //false
```

3、其余的request、session、application，这些只能在web开发中使用到。



## 7. Bean的[自动装配](https://so.csdn.net/so/search?q=自动装配&spm=1001.2101.3001.7020)

- 自动装配是Spring满足Bean依赖的一种方式。
- Spring会在上下文中自动寻找，并自动给bean装配属性。

在Spring中有三种装配的方式：

1. 在xml中显式的配置。
2. 在java中显示配置。

**3. 隐式的自动装配bean【重要】。**

### 7.1 测试

环境搭建：一个人有两个宠物。

```
public class Cat {
    public void shout(){
        System.out.println("猫叫");
    }
}
```

```
public class Dog {
    public void shout(){
        System.out.println("狗叫");
    }
}
```

```
public class People {
    private Cat cat;
    private Dog dog;
    private String name;
}
```

```
	        <bean id="cat" class="com.kuang.pojo.Cat"/>
	        <bean id="dog" class="com.kuang.pojo.Dog"/>
	    
	    <bean id="people" class="com.kuang.pojo.People" >
	        <property name="name" value="狂神"/>
	        <property name="cat" ref="cat"/>
	        <property name="dog" ref="dog"/>
	    </bean>
```

### 7.2 ByName自动装配

byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid。

```
<!--   byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid -->
        <bean id="people" class="com.kuang.pojo.People" autowire="byName">
            <property name="name" value="狂神"/>
<!--            <property name="cat" ref="cat"/>-->
<!--            <property name="dog" ref="dog"/>-->
        </bean>
```

### 7.3 ByType自动装配

byType:会自动在容器上下文中查找，和自己对象属性类型相同的bean，需要保证属性类型全局唯一 。

```
        <bean id="cat" class="com.kuang.pojo.Cat"/>
        <bean id="dog" class="com.kuang.pojo.Dog"/>  
<!--   byType:会自动在容器上下文中查找，和自己对象属性类型相同的bean，需要保证属性类型全局唯一 -->
        <bean id="people" class="com.kuang.pojo.People" autowire="byType">
            <property name="name" value="狂神"/>
<!--            <property name="cat" ref="cat"/>-->
<!--            <property name="dog" ref="dog"/>-->
        </bean>

```

**小结：**

- ByName的时候，需要保证所有的bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致。
- ByType的时候，需要保证所有的bean的class唯一，并且这个bean需要和自动注入的属性类型一致。



### 7.4 使用注解实现自动装配

要使用注解须知：

1. 导入约束： context约束。
2. 配置注解的支持：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	        https://www.springframework.org/schema/beans/spring-beans.xsd
	        http://www.springframework.org/schema/context
	        https://www.springframework.org/schema/context/spring-context.xsd">
		
		<!--开启注解的支持    -->
        <context:annotation-config/>
</beans>
```

**@Autowired**

直接在属性上使用即可，也可以在set方法上使用。

    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;

使用Autowired，可以不用再编写set方法，前提是自动装配的属性再IOC容器中存在，且符合名字byname。

**科普：**

**@Nullable** 字段标记了这个注解，表明该字段可以为null

```
 public People(@Nullable String name) {
    this.name = name;
 }
```

```
public @interface Autowired {
    boolean required() default true;
}
```

测试代码

```
//    如果显式的定义了AutoWired的required属性为false，说明该对象可以为null，否则不能为空。
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
```

如果@Autowired自动装配的环境比较复杂，自动装配无法通过@Autowired完成时，可以使用@Qualifier（value=”***“）去配置@Autowired的使用，指定一个唯一的bean对象注入。

```
    <bean id="cat" class="com.kuang.pojo.Cat"/>
    <bean id="cat222" class="com.kuang.pojo.Cat"/>
    <bean id="cat333" class="com.kuang.pojo.Cat"/>
```

```
    @Autowired
    @Qualifier(value = "cat222")
    private Cat cat;
```

**@Resource注解**

```xml
    <bean id="dog111" class="com.kuang.pojo.Dog"/>
    <bean id="dog222" class="com.kuang.pojo.Dog"/>
```

```
    @Resource(name = "dog222")
    private Dog dog;
    private String name;
```

小结：

@Resource和@Autowired的区别：

都是用来自动装配的，都可以放在属性字段上。
@Autowired通过byType的方式实现，而且必须要求这个对象存在。【常用】
@Resource默认通过byName的方式实现，如果找不到名字，则通过byType实现，两种都找不到，报错。
执行顺序：@Autowired通过byType的方式实现。@Resource使用byName的方式实现。

## 8. 使用注解开发

在Spring4之后，要使用注解开发，需要导入aop的包
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/371b1115d4abc46f7f35a1ff20b4597c.png)
使用注解开发需要导入context约束，增加注解的支持。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	        https://www.springframework.org/schema/beans/spring-beans.xsd
	        http://www.springframework.org/schema/context
	        https://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/aop
            https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--指定要扫描的包，这个包下的注解就会生效-->
    <context:component-scan base-package="com.kuang.pojo"/>

    <!--开启注解的支持    -->
    <context:annotation-config/>
    
</beans>
```

**1、bean**

**2、属性如何注入：@Component组件**

```
//等价于<bean id="user" class="com.kuang.pojo.User"/>
//@Component组件
@Component
public class User {
    //相当于<property name="name" value="kuangshen"/>
    @Value("kuangshen")
    public String name ;

    @Value("kuangshen")
    public void setName(String name) {
        this.name = name;
    }
}
```

**3、衍生的注解**
@Component有几个衍生的注解，在web开发中，会按照mvc三层架构分层。
dao层 【@Repository】
service层 【@Service】
controller层 【@Controller】
这四个注解的功能相同，都是代表将某个类注册到Spring中，装配Bean。

**4、自动装配**
@Autowired – @Qualifier
@Nullable
@Resource

**5. 作用域**

```java
@Scope("prototype") //原型模式
@Scope("singleton") //单例模式
public class User{
}
```

**6、小结**

**xml与注解**

- xml更加万能，适用于任何场合，维护简单。

- 注解不是自己类使用不了，维护相对复杂。

**xml与注解最佳实践：**

- xml用来管理bean。
- 注解只负责完成属性的注入。
- 在使用过程中，只需要注意：必须要让注解生效，需开启注解的支持。

```
    <!--指定要扫描的包，这个包下的注解就会生效-->
    <context:component-scan base-package="com.kuang"/>
    <!--开启注解的支持    -->
    <context:annotation-config/>
```

## 9. 用JAVA的方式配置Spring

完全不使用Spring的xml配置，交给Java来做。
JavaConfig是Spring的一个子项目，在Spring4之后，成为一个核心功能。

**实体类**

```
package com.kuang.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {
    public String name;

    public String getName() {
        return name;
    }

    @Value("qinjiang")
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

**配置文件**

```
package com.kuang.config;

import com.kuang.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

//@Configuration也会被Spring容器托管，注册到容器中，其本身就是@Component。
//@Configuration代表一个配置类，和之前的beans.xml相同。
@Configuration
@ComponentScan("com.kuang.pojo") //扫描包
@Import(MyConfig2.class)
public class MyConfig {
    //注册一个bean，就相当于之前写的bean标签。
    //方法的名字，相当于bean标签的id属性。
    //方法的返回值，相当于bean标签的class属性。
    @Bean
    public User getUser(){
        return new User(); //返回要注入bean的对象
    }
}
```

**测试类**

```
import com.kuang.config.MyConfig;
import com.kuang.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        //如果完全使用了配置类的方式做，仅能通过AnnotationConfig上下文来获取容器，通过配置类的class对象加载。
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        User user = (User) context.getBean("getUser");
        System.out.println(user.getName());
    }
}
```

这种纯Java 的配置，在SpringBoot中随处可见。



## 10. 代理模式

SpringAoP的底层为代理模式。

代理模式分类：

- 静态代理
- 动态代理
  ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/42be9242b135753adf6419addc8d2cc0.png)

### 10.1 静态代理

**角色分析：**

- 抽象角色：一般会使用接口或者抽象类解决。
- 真实角色：被代理的角色。
- 代理角色：代理真实角色，一般会做一些附属操作。
- 客户：访问代理对象的人。

**要求：真实角色和代理角色都要实现同一个接口。**

代码步骤：

1、接口

```
//租房
public interface Rent {
    public void rent();
}
```

2、真实角色

```
//房东
public class Host implements Rent{
    @Override
    public void rent() {
        System.out.println("房东要出租房子");
    }
}
```

3、代理角色

```
package com.kuang.demo01;

public class Proxy implements Rent{
    private Host host;

    public Proxy(Host host) {
        this.host = host;
    }

    public Proxy() {
    }

    @Override
    public void rent() {
        seeHouse();
        host.rent();
        hetong();
        fare();
    }

    //看房
    public void seeHouse(){
        System.out.println("中介带你看房");
    }

    public void hetong(){
        System.out.println("中介带你签合同");
    }

    //收中介费
    public void fare(){
        System.out.println("收中介费");
    }
}
```

4、客户端访问代理角色

```
public class Client {
    public static void main(String[] args) {
        //房东要出租房子
        Host host = new Host();
        //代理，中介帮房东租房子，一般会有附属操作（看房，签合同等）
        Proxy proxy = new Proxy(host);
        //你不用面对房东，直接找中介租房
        proxy.rent();
    }
}
```

**代理模式的好处：**

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务。
- 公共业务交给代理角色，实现了业务的分工。
- 公共业务发生扩展的时候，方便集中管理。

**缺点：**

- 一个真实角色就会产生一个代理角色，代码量会翻倍，开发效率会变低。



### 10.2 加深理解

1、接口

```
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();
}
```

2、真实角色

```
//真实对象
public class UserServiceImpl implements UserService{
    @Override
    public void add() {
        System.out.println("增加了一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个用户");
    }

    @Override
    public void update() {
        System.out.println("修改了一个用户");
    }

    @Override
    public void query() {
        System.out.println("查询了一个用户");
    }
}
```

3、代理角色

```
package com.kuang.demo02;

public class UserServiceProxy implements UserService{

    private UserServiceImpl userService;

    public void setUserService(UserServiceImpl userService) {
        this.userService = userService;
    }

    @Override
    public void add() {
        log("add");userService.add();
    }

    @Override
    public void delete() {
        log("delete");userService.delete();
    }

    @Override
    public void update() {
        log("update");userService.update();
    }

    @Override
    public void query() {
        log("query");userService.query();
    }

    //日志方法
    public void log(String msg){
        System.out.println("使用了"+msg+"方法");
    }
}
```

4、客户端

```
public class Client {
    public static void main(String[] args) {
        UserServiceImpl userService = new UserServiceImpl();
        UserServiceProxy userServiceProxy = new UserServiceProxy();
        userServiceProxy.setUserService(userService);

        userServiceProxy.add();
    }
}
```

通过代理模式，不需修改真实角色的原有代码，既可实现一些附属操作。

### 10.3 动态代理

- 动态代理和静态代理角色一样。
- 动态代理类是动态生成的，不是直接写好的。
- 动态代理分类：基于接口 的动态代理、基于类的动态代理。
  基于接口 ----- JDK动态代理。
  基于类 ------ cglib
  java字节码实现 ------ javasist

**两个类：**

- Proxy：代理
- InvocationHandler：调用处理程序

1、接口

```java
//租房
public interface Rent {
    public void rent();
}
1234
```

2、真实角色

```
//房东
public class Host implements Rent {
    @Override
    public void rent() {
        System.out.println("房东要出租房子");
    }
}
```

3、动态代理类

```
//使用该类自动生成代理模式
public class ProxyInvocationHandler implements InvocationHandler {

    //被代理的接口
    private Rent rent;

    public void setRent(Rent rent) {
        this.rent = rent;
    }

	//生成代理类
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),
        							rent.getClass().getInterfaces(),this);
    }

    //处理代理实例，并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        //动态代理的本质就是使用反射机制。
        seeHouse();
        Object result = method.invoke(rent, args);
        fare();
        return result;
    }

    public void seeHouse(){
        System.out.println("中介带看房子");
    }
    public void fare(){
        System.out.println("收中介费");
    }
}
```

4、客户端

```
public class Client {
    public static void main(String[] args) {
        //真实角色
        Host host = new Host();
        //代理角色：现在没有
        ProxyInvocationHandler pih = new ProxyInvocationHandler();
        //通过调用程序处理角色来处理我们要调用的接口对象
        pih.setRent(host); //要代理的角色
        Rent proxy = (Rent) pih.getProxy();//proxy为动态生成，并没写
        proxy.rent();
    }
}
```

**动态代理工具类模板：**

```
//使用该类自动生成代理模式
public class ProxyInvocationHandler implements InvocationHandler {

    //被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    //生成代理类
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),target.getClass().getInterfaces(),this);
    }

    //处理代理实例，并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //动态代理的本质就是使用反射机制。
        log(method.getName());
        Object result = method.invoke(target, args);
        return result;
    }

    //日志
    public void log(String msg){
        System.out.println("执行了："+msg+"方法");
    }

}
```

客户端

```
public class Client {
    public static void main(String[] args) {
        //真实角色
        UserServiceImpl userService = new UserServiceImpl();
        //代理角色，不存在
        ProxyInvocationHandler pih = new ProxyInvocationHandler();
        //设置要代理的对象
        pih.setTarget(userService);
        //动态生成代理类
        UserService proxy = (UserService) pih.getProxy();
        proxy.delete();
    }
}
```

**动态代理的好处：**

- 静态代理的所有好处。
- 一个动态代理类代理的是一个接口，一般就是一个对应的业务。
- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可。



## 11. AOP

### 11.1 什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程， 通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/977114aa6feb60ec215377290c0010b8.png)

### 11.2 AOP在Spring中的作用

**提供声明式事务；允许用户自定义切面**

**横切关注点：** 跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等…
**切面（ASPECT）：**横切关注点被模块化的特殊对象。即，它是一个类。
**通知（Advice）：**切面必须要完成的工作。即，它是类中的一个方法。
**目标（Target）：**被通知对象。
**代理（Proxy）：**向目标对象应用通知之后创建的对象。
**切入点（PointCut）：**切面通知执行的“地点”的定义。
**连接点（JointPoint）：**与切入点匹配的执行点。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ad74f1595b1d97d1bc9e0ce268653677.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f0ffb9e92a44edee092420088f257900.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/280d34f18c32e203594d81bd2fd19f6a.png)
**即AOP在不改变原有代码的情况下，去增加新的功能。**



### 11.3 使用Spring实现AOP

【重点】 使用AOP植入，需要导入一个依赖包。

```
    <dependencies>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.9</version>
        </dependency>
    </dependencies>
```

**方式一：使用Spring的API接口 【主要SpringAPI接口实现】**
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/721d0c72eacf16bd5e9aa3c052a2edd5.png)

**UserService：**

```
package com.kuang.service;

public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void select();
}
```

**UserServiceImpl：**

```
package com.kuang.service;

public class UserServiceImpl implements UserService{
    @Override
    public void add() {
        System.out.println("增加了一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个用户");
    }

    @Override
    public void update() {
        System.out.println("修改了一个用户");
    }

    @Override
    public void select() {
        System.out.println("选择了一个用户");
    }
}
```

**Log：**

```
package com.kuang.log;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class Log implements MethodBeforeAdvice {
    @Override
    //method:要执行的目标对象的方法
    //args：参数
    //target：目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
    }
}
```

**afterLog：**

```
package com.kuang.log;

import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class AfterLog implements AfterReturningAdvice {

    //returnValue：返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了"+method.getName()+"方法， 返回结果为："+returnValue);
    }
}
```

**applicationContext：**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">


	<!--注册bean-->
    <bean id="userService" class="com.kuang.service.UserServiceImpl"/>
    <bean id="log" class="com.kuang.log.Log"/>
    <bean id="afterLog" class="com.kuang.log.AfterLog"/>


    <!--方式一：使用原生的spring API接口-->
    <!--配置AOP：需要导入aop的约束-->
    <aop:config>
        <!--切入点：expression：表达式，execution（要执行的位置，****）-->
        <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>

        <!--执行环绕增加-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>

</beans>
```

**MyTest：**

```
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //动态代理代理是接口
        UserService userService = (UserService)context.getBean("userService");
        userService.add();
    }
}
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/428a33eb390c42384809d883f18cb9e2.png)

**定义DiyPointCut类：**

```
package com.kuang.diy;

public class DiyPointCut {
    public void before(){
        System.out.println("======方法执行前=====");
    }
    public void after(){
        System.out.println("======方法执行后=====");
    }
}
```

**applicationContext.xml:**

```
	<!--方式二：自定义类-->
    <bean id="diy" class="com.kuang.diy.DiyPointCut"/>
    <aop:config>
    <!--自定义切面，ref要引用的类-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <aop:pointcut id="point" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
            <!--通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>
```

**测试类或接口内容不变。**
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4310ffc24ac53bf0cf8faa26f22db776.png)

**方式三：使用注解实现**

**自定义类：AnnotationPointCut**

```
package com.kuang.diy;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

//方式三：使用注解方式实现AOP
@Aspect //标注该类为切面
public class AnnotationPointCut {
    @Before("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("======方法执行前=====");
    }

    @After("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("======方法执行后=====");
    }

    //在环绕增强中，可以给定一个参数，代表我们要获取处理切入的点
    @Around("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint jp) throws Throwable{
        System.out.println("环绕前");
        Signature signature = jp.getSignature();
        System.out.println("signature"+signature);

        Object proceed = jp.proceed(); //执行方法
        System.out.println("环绕后");

        System.out.println(proceed);
    }
}
```

**applicationContext.xml**

```
    <!--方式三：使用注解的方式-->
    <bean id="annotationPointCut" class="com.kuang.diy.AnnotationPointCut"/>
    <!--开启注解支持-->
    <aop:aspectj-autoproxy/>
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/58be31f018e1885b339e28095e43ff49.png)

## 12. 整合Mybatis

步骤：

1. 导入相关jar包
   junit、mybatis、mysql数据库、spring相关的、aop植入、mybatis-spring
2. 编写配置文件
3. 测试

### 12.1 回忆Mybatis

1、编写实体类

```
package com.kuang.pojo;

import lombok.Data;

@Data
public class User {
    private int id;
    private String name;
    private String pwd;
}
```

2、编写核心配置文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    <typeAliases>
        <package name="com.kuang.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <package name="com.kuang.mapper"/>
    </mappers>
    
</configuration>
```

3、编写接口

```
package com.kuang.mapper;

import com.kuang.pojo.User;

import java.util.List;

public interface UserMapper {
    public List<User> selectUser();
}
```

4、编写Mapper.xml文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.mapper.UserMapper">
    <select id="selectUser" resultType="user">
        select * from mybatis.user
    </select>
</mapper>
```

5、测试

```
public class MyTest {
    @Test
    public void selectUser() throws IOException {
        String resources = "mybatis-config.xml";
        InputStream in = Resources.getResourceAsStream(resources);
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(in);
        SqlSession sqlSession = build.openSession(true);

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.selectUser();

        for (User user : userList) {
            System.out.println(user);
        }

        sqlSession.close();

    }
}
```

### 12.2 Mybatis-Spring

MyBatis-Spring 会帮助你将 MyBatis 代码无缝地整合到 Spring 中。

文档链接：http://mybatis.org/spring/zh/index.html

如果使用 Maven 作为构建工具，仅需要在 pom.xml 中加入以下代码即可：

```
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
```

**整合实现方式一：**

1、引入Spring配置文件spring-dao.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

<!--    第一步：配置数据源替换mybatis的数据源-->

    <!--DataSource: 使用spring的数据源替换mybatis的配置
        此处使用spring提供的jdbc：org.springframework.jdbc.datasource-->
    <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>

<!--    第二步：配置SqlSessionFactory，关联Mybatis-->

    <!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource"/>
        <!--绑定Mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/kuang/mapper/*.xml"/>
    </bean>

<!--    第三步：注册sqlSessionTemplate，关联sqlSessionFactory-->
    
    <!--SqlSessionTemplate: 就是我们使用的sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器注入sqlSessionFactory,因为其没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>


</beans>
```

2、编写UserMapper接口的实现类UserMapperImpl，私有化sqlSessionTemplate

```
package com.kuang.mapper;

import com.kuang.pojo.User;
import org.mybatis.spring.SqlSessionTemplate;

import java.util.List;

public class UserMapperImpl implements UserMapper{
    //我们所有操作，都使用sqlSession来执行，在原来，现在都使用SqlSessionTemplate
    private SqlSessionTemplate sqlSession;

    public void setSqlSession(SqlSessionTemplate sqlSession) {
        this.sqlSession = sqlSession;
    }

    @Override
    public List<User> selectUser() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.selectUser();
    }
}
```

3、将自己编写的实现类，注入到spring-dao配置文件中

```
    <bean id="userMapper" class="com.kuang.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
```

4、测试

```
public class MyTest {
    @Test
    public void selectUser() throws IOException {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
        UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
        for (User user : userMapper.selectUser()) {
            System.out.println(user);
        }
    }
}
```

整合后，mybatis-config.xml配置文件内容如下：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    <typeAliases>
        <package name="com.kuang.pojo"/>
    </typeAliases>

    
</configuration>
```

**整合实现方式二**

1、修改方式一中的UserMapperImpl文件

```
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
    @Override
    public List<User> selectUser() {
        return getSqlSession().getMapper(UserMapper.class).selectUser();
    }
}
```

2、注册到spring配置文件中

```
    <bean id="userMapper2" class="com.kuang.mapper.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
```

3、测试

```
public class MyTest {
    @Test
    public void selectUser() throws IOException {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
        UserMapper userMapper = context.getBean("userMapper2", UserMapper.class);
        for (User user : userMapper.selectUser()) {
            System.out.println(user);
        }
    }
}
```

注意：

可以编写总配置文件applicationContext.xml，将spring-dao.xml导入其中。
此时，自己编写的实现类，可以直接在applicationContext中编写注入。
在测试类中，可使用new ClassPathXmlApplicationContext(“applicationContext.xml”)。

**applicationContext.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <import resource="spring-dao.xml"/>

    <bean id="userMapper" class="com.kuang.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>

    <bean id="userMapper2" class="com.kuang.mapper.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>

</beans>
```

**测试类：**

```
 ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```

## 13. 声明式事务

### 13.1 回顾事务

- 把一组业务当成一个业务来做，要么都成功，要么都失败。
- 事务在项目开发中，十分重要，涉及到数据的一致性问题，不容马虎。
- 确保完整性和一致性。

**事务ACID原则：**

- 原子性
- 一致性
- 隔离性
- 持久性

**测试：**

利用上面案例的代码新建一个项目，为userMapper接口新增两个方法，删除和增加用户；

```
public interface UserMapper {
    public List<User> selectUser();

    //添加一个用户
    public int addUser(User user);

    //删除一个用户
    public int deleteUser(int id);
}
```

UserMapper文件中，故意将deletes写错，测试是否能够删除。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.mapper.UserMapper">
    <select id="selectUser" resultType="user">
        select * from mybatis.user
    </select>

    <insert id="addUser" parameterType="user">
        insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd})
    </insert>

    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id = #{id}
    </delete>
    
</mapper>
```

编写接口的UserMapperImpl实现类

```
package com.kuang.mapper;

import com.kuang.pojo.User;
import org.mybatis.spring.SqlSessionTemplate;
import org.mybatis.spring.support.SqlSessionDaoSupport;

import java.util.List;

public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper{

    @Override
    public List<User> selectUser() {

        User user = new User(12, "xiaowang", "23244");
        UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
        mapper.addUser(user);
        mapper.deleteUser(4);
        return mapper.selectUser();
    }

    @Override
    public int addUser(User user) {
        return getSqlSession().getMapper(UserMapper.class).addUser(user);
    }

    @Override
    public int deleteUser(int id) {
        return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
    }
}
```

测试类：

```
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper = context.getBean("userMapper", UserMapper.class);

        List<User> userList = userMapper.selectUser();
        for (User user : userList) {
            System.out.println(user);
        }
    }
}
```

测试结果：sql异常，delete错误，但数据库中成功插入数据，无法删除。
Spring中提供了事务管理，只需要配置即可。

### 13.2 Spring中的事务管理

Spring在不同的事务管理API之上定义了一个抽象层，使得开发人员不必了解底层的事务管理API就可以使用Spring的事务管理机制。Spring支持编程式事务管理和声明式的事务管理。

**编程式事务管理**

-将事务管理代码嵌到业务方法中来控制事务的提交和回滚。
-缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码。

**声明式事务管理**

- 一般情况下比编程式事务好用。
- 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
- 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring AOP框架支持声明式事务管理。

**1. 使用Spring事务管理，需要在spring-dao.xml中导入头文件的约束：tx**

```
xmlns:tx="http://www.springframework.org/schema/tx"

http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd"
```

**2. 配置声明事务 spring-dao.xml**

```
    <!--配置声明式事务-->
    <bean id="transactionManager" 					  class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="datasource"/>
    </bean>

```

**3. 配置事务的通知 spring-dao.xml**

```
    <!--结合AOP实现事务的植入-->
    <!--配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给哪些方法配置事务-->
        <!--配置事务的传播特性： new propagation-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" propagation="REQUIRED"/>
            <!--正常写下面一行即可：配置全部的方法-->
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
```

**4. 配置事务切入 spring-dao.xml**

```
    <!--配置事务切入-->
    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.kuang.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>

```

**5.测试**

```
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper = context.getBean("userMapper", UserMapper.class);

        List<User> userList = userMapper.selectUser();
        for (User user : userList) {
            System.out.println(user);
        }
    }
}
```



## 14. 总结

1、Spring的核心：控制反转（IOC）和面向切面编程（AOP）。

2、在Spring中，创建一个实体类后，需要在Spring的核心配置文件(**.xml)中，通过bean的方式进行注册。相当于一个大容器中有了各类的对象，想用的时候直接取就行。

3、IOC利用有参构造创建对象的三种方式：下标赋值、类型创建、参数名。无参构造直接赋值即可。

4、在配置文件中通过bean的方式注册类，就和java中通过new的方式生成对象类似。

5、类取别名：alias。多个配置文件的合并：import。

6、依赖注入：就相当于给类中的属性进行赋值。注入方式有：通过构造器的方式、通过set方法的方式。通过set的方式需要类里面有相应属性的set方法。可以给多种类型参数赋值：普通值、数组、集合等。

7、P命名空间给属性赋值（直接注入）、C命名空间给属性赋值（利用构造器）。使用前需要导入xml约束。

8、Bean的作用域（scope）：单例模式（singleton）和原型模式（prototype）。

9、Bean自动装配：Spring在容器或上下文中自动寻找，并自动给bean匹配属性。byName：利用bean的id确定匹配，而byType利用bean的class确定匹配。同时，可以使用注解@Autowired和@Resources来进行自动装配，前提要在核心配置文件中开启注解的支持。

10、使用注解开发：需要导入AOP的包和开启注解支持。在类的上面加上@Component等同于在核心配置文件中注册bean，在属性的上面加上@value（*）可以直接对属性值注入。

11、使用Java配置Spring：在类的上面加上@Configuration代表该类为配置类，在类中方法的上面加上@Bean类似在核心配置文件中注册，方法名相当于bean中的id，返回值等同于bean中的class属性。

12、代理模式包括的角色：抽象角色（接口或抽象类）、真实角色、代理角色、客户。真实角色和代理角色要实现同一个接口。代理角色可以增加一些额外的操作，而真实角色的代码不用修改。静态代理：一个真实角色需要一个代理角色进行针对性的管理。动态代理：代理类是动态生成的，可以定义生成代理类的工具类，一个动态代理类可以代理多个类，只要实现了同一个接口。

13、Spring实现AOP：实现了在执行方法前和后，可以自定义一些操作。方式有：① 实现API的接口（类似MethodBeforeAdvice和AfterReturningAdvice），在核心配置文件中配置AOP，设定切入点和执行环绕增加。 ② 自定义类实现AOP（主要是切面定义），在核心配置文件中定义切面、切入点和方法通知。 ③ 使用注解实现：自定义类，标注为切面，对方法进行标注，开启注解。

14、Spring和Mybatis的整合：固定模板。

15、Spring中有针对事务的管理办法，在核心配置文件中配置事务的通知即可（固定模板）。
