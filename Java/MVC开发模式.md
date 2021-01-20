# MVC三层架构

> - model模型
>   - 数据库访问dao
>   - 业务逻辑处理service    领域对象模型
> - view视图（JSP）
>   - 展示数据
>   - 提供可以供我们操作的请求
> - controller控制器（请求转发 servlet）
>   - 接受用户的请求
>   - 响应给客户端内容
>   - 重定向或者转发
>

## 为什么使用MVC开发模式

传统JSP开发网页的缺点：**耦合度高**

MVC开发模式解决了耦合度高的问题

## 什么是MVC开发模式

M代表Model（模型）

指对象，数据库等（最底层）

V代表View（视图）

指视图，网页或应用上的给用户看的东西

C代表Controller（控制器）

分离View和Model，分别控制model和view进行业务操作

