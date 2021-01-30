---
title: Spring 框架
date: 2020-8-1
updated: 2020-8-1
categories: Springboot
tags: Spring
cover: https://gitee.com/kzycn/picCloud/raw/master/2021/20210125161233.jpg
top_img: https://gitee.com/kzycn/picCloud/raw/master/2021/20210125161233.jpg
---



# Spring框架

## 介绍

Spring是轻量级开源的JavaEE框架，解决企业应用开发的复杂性，核心有两个部分：**IOC和AOP**

[Spring官方文档](https://docs.spring.io/spring/docs/current/spring-framework-reference/index.html)

### IOC和AOP介绍

1. IOC：控制反转，把创建对象过程交给Spring进行管理
2. AOP：面向切面，不修改源代码进行功能增强

### Spring特点

1. 方便解耦，简化开发
2. Aop编程的支持
3. 方便程序测试
4. 方便集成其他框架
5. 方便进行事务管理
6. 降低API开发难度

### SSH&SSM

- **SSH : Struct2 + Spring + Hibernate**
- **SSM: SpringMVC + Spring + Mybatis**

---

[Spring官网地址](https://spring.io/)

![image-20201124165927933](https://gitee.com/kzycn/picCloud/raw/master/2020/20201124165930.png)

**Maven配置：**

```XML
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.0.3.RELEASE</version>
</dependency>
```

**通过maven直接导入**（都学到Spring了不会还不知道maven吧~）

## 控制反转（IOC）

`IOC` 是 `Inverse of Control` 的缩写,意思是控制反转. 是降低对象之间耦合关系的设计思想.

通过 IOC ,开发人员不需要关心对象的创建过程,该过程交给Spring IOC**容器**完成.Spring IOC**容器**通过**反** 

**射**机制创建类的实例.

> 将对象的创建交给Spring容器管理,转移控制权

## 依赖注入（DI）

`DI` 是 `Dependency Injection` 的缩写,意思是 Spring IOC 容器 创建对象的时候,同时为这个对象

**注入**它所**依赖**的属性的值. 这个对象与对象之间的关系也交给了 Spring IOC 容器 来维护.

> 将对象与对象之间的关系交给Spring IOC容器维护,转移控制权

总结:

依赖注入是 Spring IOC 容器在运行期间, 动态的将依赖关系注入到对象中.

所以依赖注入是控制反转的实现方式.

依赖注入和控制反转描述是从不同角度描述的同一件事情, 就是通过引入Spring IOC容器,利用依赖关系注

入的方式, **实现对象与对象之间的解耦**.(高内聚,低耦合) 

![控制反转和依赖注入](https://gitee.com/kzycn/picCloud/raw/master/%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC%E5%92%8C%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5.png)

## 面向切面编程（AOP）

Spring 框架的一个关键组件是面向切面的程序设计（AOP）框架。

一个程序中跨越多个点的功能被称为横切关注点，这些横切关注点在概念上独立于应用程序的业务逻

辑。有各种各样常见的很好的关于方面的例子，比如**日志记录、声明性事务、安全性，和缓存**等等。

(Filter) 

Spring 框架的 AOP 模块提供了面向切面的程序设计实现，允许你定义拦截器方法和切入点，**可以实现**

**将应该被分开的代码干净的分开的功能**。

## 核心容器

`spring-context` 模块构架于核心模块之上， 它扩展了 `BeanFactory`， 为它添加了 `Bean` 生命周

期控制、 框架事件体系以及资源加载透明化等功能·。 此外该模块还提供了许多企业级支持， 如邮

件访问、远程访问、 任务调度等。

`ApplicationContext` 是该模块的**核心接口**，它是 `BeanFactory` 的超类， 与`BeanFactory` 不同，

`ApplicationContext` 容器实例化后会自动对所有的单实例 Bean 进行实例化与依赖关系的装

配， 使之处于待用状态。 

`ApplicationContext` 接口的常用子类: 

`FileSystemXmlApplicationContext` 从磁盘绝对路径加载配置文件初始化IOC容器

`ClassPathXmlApplicationContext` 从类路径加载配置文件初始化IOC容器

## Hello World

### 1.创建maven工程

![image-20200927070024820](https://gitee.com/kzycn/picCloud/raw/master/image-20200927070024820.png)

### 2.项目依赖

```XML
<dependencies>
        <!--spring模块-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>

        <!--junit测试模块-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13</version>
            <scope>test</scope>
        </dependency>

        <!--日志模块-->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.12.1</version>
        </dependency>
    </dependencies>
```

### 3.配置文件

![image-20200927070220653](https://gitee.com/kzycn/picCloud/raw/master/image-20200927070220653.png)

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

#### 日志配置文件

```XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--先定义所有的appender-->
    <appenders>
        <!--输出控制台的配置-->
        <console name="Console" target="SYSTEM_OUT">
            <!--输出日志的格式-->
            <patternlayout pattern="%d{yyyy-MM-dd HH:mm:ss} [%p] %c %m %n"/>
        </console>
    </appenders>
    <!-- 然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
    <!-- 日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->
    <loggers>
        <!--org.springframework <logger name="org.springframework" level="INFO"/>-->
        <root level="DEBUG">
            <!--输出到控制台-->
            <appender-ref ref="Console"/>
        </root>
    </loggers>
</configuration>
```

### 配置JavaBean

```JAVA
public class javabean {

    private String massage;

    public String getMassage() {
        return massage;
    }

    public void setMassage(String massage) {
        this.massage = massage;
    }
```

### 装配JavaBean

```XML
<!-- 配置baen
    <bean> 配置需要让Spring IOC容器创建的对象 name ：
    用于之后从spring容器获得实例时使用 class ：
    需要创建实例的全限定类名
    property:类中的成员
    property name 成员的名称
    property value 成员的值
    scope:singleton 单例
    prototype 多例
    lazy-init: true
    按需创建对象 false
    容器启动就创建对象
    scope="singleton"时有效
    -->

    <bean id="javabean"
          class="cn.husei.spring.server.javabean"
          scope="singleton"
          lazy-init="true"
          init-method="init"
          destroy-method="destroy"
    >
        <property name="massage" value="hello spring"/>
    </bean>
```

### 单元测试

```JAVA
public void test01() {
        //原来的执行过程（没有用Spring）
        javabean javabean = new javabean();
        javabean.setMassage("hello spring");
        String msg =  javabean.getMassage();
        System.out.println("消息内容："+msg);

    }

    public void test02(){
        //用spring之后

        //1.读取spring-config.xml文件，完成容器的初始化
        ApplicationContext applicationContext
                = new ClassPathXmlApplicationContext("spring-config.xml");

        //2.从容器中获取对象
        javabean bean = applicationContext.getBean(javabean.class);

        //3.使用对象
        bean.setMassage("hello spring");
        String msg = bean.getMassage();
        System.out.println("消息内容："+msg);

    }
```

### 构造方法注入

#### 方式一 

使用构造方法中参数的索引(常用)

```XML
<bean id="messageService" class="com.imcode.spring.service.MessageService">
	<constructor-arg index="0" value="Hello Spring"/>
</bean>
```

#### 方式二

使用构造方法中参数的名称(常用)

```XML
<bean id="messageService" class="com.imcode.spring.service.MessageService"> 
    <constructor-arg name="message" value="Hello Spring"/> 
</bean>
```

**方式三**

使用构造方法中参数的类型(不常用) 

```XML
<bean id="messageService" class="com.imcode.spring.service.MessageService"> 
    <constructor-arg type="java.lang.String" value="Hello Spring"/> 
</bean>
```

## 启动注解

### XML文件启动注解扫描

```XML
<?xml version="1.0" encoding="UTF-8"?> <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring- beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring- context.xsd"> 
    <!-- 启动注解扫描 -->
	<context:component-scan base-package="com.imcode.spring.service"/> 
</beans>
```

### 控制反转IOC

`@Component`

- `@Component`等价于

  ```xml
  <bean class=""/>
  ```

- `@Component("name")`等价于

  ```XML
  <bean name="" class=""/>
  ```

### 衍生注解

提供3个 `@Component` 注解衍生注解（功能一样）,分别用于数据访问层,服务层和控制层

- `@Repository`
  - 数据访问层
- `@Service`
  - 服务层
- `@Controller`
  - 控制层

### 依赖注入DI

**注入简单数据** 

`@Value`

注入简单数据类型： `@Value("${}")`

**注入对象** 

`@Autowired` 

`@Qualifier("name")` 

>@Autowired 如果注入的是具体类，不会有问题
>
>@Autowired 如果注入的是接口，该接口只有一个实现类，不会有问题
>
>@Autowired 如果注入的是接口，该接口有一个以上的实现类，会有问题，IOC容器不知道应该注
>
>入哪个实 现类了
>
>使用
>
>@Autowired + @Qualifier("名称") 组合使用两个注解解决

`@Resource` 

`@Resource(name = "bean的名称")`

该注解是JDK提供的注解

`@Resource("name") == @Autowired + @Qualifier("name")` 

`@Resource == @Autowired` 

## 面向切面编程AOP

### AOP **简介**

`AOP` 是 `Aspect Oriented Programming` 的缩写，意为：**面向切面编程**，通过预编译方式和**运**

**行期动态代理**实现程序功能的统一维护的一种技术。

`AOP` 是 `OOP` （面向对象编程）的补充，是软件开发中的一个热点，也是Spring框架中的一个重

要内容。

利用 `AOP` 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提

高程序的可重用性，同时提高了开发的效率。

经典应用：**事务管理、性能监视、安全检查、缓存、日志**等

