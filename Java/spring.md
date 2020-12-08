# Spring 框架

## 介绍

Spring是轻量级开源的JavaEE框架，解决企业应用开发的复杂性，核心有两个部分：**IOC和AOP**

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

---

[Spring官网地址](https://spring.io/)

点击进入**GitHub**，进入**wiki**，找到下载地址

![image-20200801135715867](https://gitee.com/kzycn/picCloud/raw/master/20200801135723.png)

![image-20200801140249325](https://gitee.com/kzycn/picCloud/raw/master/20200801140249.png)

**Maven配置：**

```XML
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.0.3.RELEASE</version>
</dependency>
```

**通过maven直接导入**（都学到Spring了不会还不知道maven吧~）

![image-20200801141321332](https://gitee.com/kzycn/picCloud/raw/master/20200801141321.png)

---

## IOC

### 介绍

控制反转（IOC）是面向对象编程的一种设计原则，把对象创建和对象之间的调用过程交个Spring来管理，目的就是为了使耦合度降低

### IOC底层原理

xml解析，工厂模式，反射

### IOC接口

1. IOC思想基于IOC容器完成，IOC容器底层就是对象工厂

2. Spring提供IOC容器实现两种方式（两种接口）

   **BeanFactory**：IOC容器基本实现，是spring内部的接口，不提供开发人员使用

   > 加载配置文件的时候不会创建对象，在获取对象（使用）才去创建对象

   **ApplicationContext**：BeanFactory接口的子接口，提供更多强大的功能，**一般由开发人员使用**

   > 加载配置文件的时候就会帮你创建对象

### IOC操作Bean管理

**什么是Bean管理操作？**

Bean管理指的是两个操作：

1. Spring创建对象
2. Spring注入

Bean管理操作有两种方式：

1. 基于xml配置文件方式实现
2. 基于注解方式实现

**基于xml配置文件方式实现**：

```XML
<bean id="user" class="top.floatlife.spring5.user"></bean>
```

在spring配置文件中，使用bean标签，标签里面添加对应属性，就可以实现对象创建

**bean标签属性：**

1. id属性：给对象起一个别名
2. class属性：类的全路径
3. name属性：跟id一样，但是可以添加特殊符号（不常用）

创建对象时候，默认也是执行无参数构造方法完成对象创建。

**基于xml方式注入属性**：

**DI：依赖注入，就是注入属性**

**第一种：**

创建类，定义属性和对应的set方法

```JAVA
package top.floatlife.spring5;
import java.awt.print.Book;

public class book {

//  创建属性
    private String bname;
    private String user;

//  创建属性对应的set方法
    public void setBname(String bname){
        this.bname = bname;
    }

    public void setUser(String user) {
        this.user = user;
    }

    public static void main(String[] args) {
        book book = new book();

        book.setBname("abc");
    }
}

```

**第二种：**

在spring配置文件配置对象创建，配置属性注入

```XML
<!-- set方法进入属性 -->
    <bean id="book" class="top.floatlife.spring5.book">
<!--使用property完成属性注入，value是要注入的属性值-->
        <property name="bname" value="人间失格"></property>
        <property name="user" value="看书的人"></property>
    </bean>
```

有参构造注入属性

```XML
<--使用constructor-arg标签-->
<constructor-arg name="" value=""></constructor-arg>
<constructor-arg name="" value=""></constructor-arg>
```

### 配置文件的命名空间

> **p命名空间和c命名空间**

**1.p命名空间**

Spring提供了更加简洁的p-命名空间， 作为`<property>`元素的替代方案。为了启用p-命名空间，必须要在 XML文件中与其他的命名空间一起对其进行声明

使用`p`命名空间时需要先声明使用对应的命名空间，即在`beans`元素上加入`xmlns:p="http://www.springframework.org/schema/p"`

![17432835-bf3777accc58672c](https://gitee.com/kzycn/picCloud/raw/master/20200802165225.jpg)

**2.c命名空间**

它是在XML中更为简洁地**描述构造器参数的方式**。要使用它的话，必须要在XML的顶部声明其模式

![17432835-bd056e2dece0c4fc](https://gitee.com/kzycn/picCloud/raw/master/20200802165114.png)

属性名以“c:”开头，也就是命名空间的前缀。接下来就是要装配的构 造器参数名，在此之后是“-ref”，这是一个命名的约定，它会告诉 Spring，正在装配的是一个bean的引用，这个bean的名字是compactDisc，而不是字面量“compactDisc”。

### bean的作用域

> 在Spring中，那些组成应用程序的主体及由Spring IoC容器所管理的对象，被称之为bean
>
> Spring中的bean默认都是单例的

![image-20200803000107013](https://gitee.com/kzycn/picCloud/raw/master/20200803000114.png)

![image-20200803000633546](https://gitee.com/kzycn/picCloud/raw/master/20200803000633.png)

1. 单例模式（Spring默认机制）

```XML
<bean scope="singleton"></bean>
```

2. 原型模式：每次从容器中get时侯，都会产生一个新对象

```XML
<bean scope="prototype"></bean>
```

3. 五种作用域中，**request、session和global session三种作用域仅在基于web的应用中使用**（不必关心你所采用的是什么web应用框架），只能用在基于web的Spring ApplicationContext环境。

---

### Bean的自动装配

- 自动装配是Spring满足bean依赖一种方式

- Spring会在上下文自动寻找，并自动给bean装配属性

**Spring中有三种装配方式：**

1. 在xml中配置
2. 在java中配置
3. 隐式的自动配置

原代码：
![image-20200803090610223](https://gitee.com/kzycn/picCloud/raw/master/20200803090617.png)

**autowire**，**byName属性**

![image-20200803090801909](https://gitee.com/kzycn/picCloud/raw/master/20200803090802.png)

**autowire**，**byType属性**

![image-20200803091119432](https://gitee.com/kzycn/picCloud/raw/master/20200803091119.png)

- **byName**：会在容器上下文中查找，和自己对象set方法后面的值对应的bean id

- **byType**：会在容器上下文中查找，和自己对象属性类型相同的bean id

**小结:**

- byname的时候， 需要保证**所有bean的id唯一** ,并且这个bean需要和自动注入的属性的set方法的值一致!
- bytype的时候，需要保证**所有bean的class唯一** ，并且这个bean需要和自动注入的属性的类型一致!

---

### 使用注解实现自动装配

要使用注解需：

1. 导入约束
2. 配置注解支持

```XML
xmlns:context="http://www.springframework.org/schema/context"

http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context-3.0.xsd

<context:annotation-config></context:annotation-config>
```

![image-20200803132926131](https://gitee.com/kzycn/picCloud/raw/master/20200803132926.png)

###  **@Autowired**

自动装配，其作用是为了消除代码Java代码里面的getter/setter与bean属性中的property。当然，getter看个人需求，如果私有属性需要对外提供的话，应当予以保留。

![image-20200803132213882](https://gitee.com/kzycn/picCloud/raw/master/20200803132214.png)

```JAVA
public @interface Autowired {
    boolean required() default true;
}

//如果显示定义了Autowired的required属性为false，说明这个对象可以为null,否则不允许为null
    @Autowired(required = false)
```

**@Autowired注解**要去寻找的是一个Bean，Tiger和Monkey的Bean定义都给去掉了，自然就不是一个Bean了，Spring容器找不到也很好理解。那么，如果属性找不到我不想让Spring容器抛出异常，而就是显示null，可以吗？可以的，其实异常信息里面也给出了提示了，**就是将@Autowired注解的required属性设置为false即可**

![image-20200803162122996](https://gitee.com/kzycn/picCloud/raw/master/20200803162123.png)

### @**Qualifier（指定注入Bean的名称）**

如果容器中有一个以上匹配的Bean，则可以通过**@Qualifier注解限定Bean的名称**

![image-20200803164121255](https://gitee.com/kzycn/picCloud/raw/master/20200803164121.png)

![image-20200803164141173](https://gitee.com/kzycn/picCloud/raw/master/20200803164141.png)

### 扫描bean

```XML
<!--    指定要扫描的包，该包下的注解就会生效-->
    <context:component-scan base-package="top.floatlife.usespring"/>
```

![image-20200803173326471](https://gitee.com/kzycn/picCloud/raw/master/20200803173326.png)

### @Component

```JAVA
//等价于<bean id= "user" class= "top.floatlife.usespring"/>
// @Component组件
@Component
public class user {
    public String name = "浮生";
}
```

![image-20200803175647228](https://gitee.com/kzycn/picCloud/raw/master/20200803175647.png)

### @Value

```JAVA
//相当于<property name= "name" value= "FUSHENG"/>
@Value( "FUSHENG")
```

### 衍生注解
