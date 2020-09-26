# 事务管理（ACID）

### 原子性（Atomicity）
原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。
### 一致性（Consistency）
事务前后数据的完整性必须保持一致。
### 隔离性（Isolation）
事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离。
### 持久性（Durability）
持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障也不应该对其有任何影响



### 原子性

针对同一个事务

![这里写图片描述](https://img-blog.csdn.net/20180906211811672?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RlbmdqaWxp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这个过程包含两个步骤

A： 800 - 200 = 600
B: 200 + 200 = 400

原子性表示，这两个步骤一起成功，或者一起失败，不能只发生其中一个动作

### 一致性（Consistency）

针对一个事务操作前与操作后的状态一致

![这里写图片描述](https://img-blog.csdn.net/20180906211811672?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RlbmdqaWxp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

操作前A：800，B：200
操作后A：600，B：400

一致性表示事务完成后，符合逻辑运算

### 持久性（Durability）
表示事务结束后的数据不随着外界原因导致数据丢失

操作前A：800，B：200
操作后A：600，B：400
如果在操作前（事务还没有提交）服务器宕机或者断电，那么重启数据库以后，数据状态应该为
A：800，B：200
如果在操作后（事务已经提交）服务器宕机或者断电，那么重启数据库以后，数据状态应该为
A：600，B：400

### 隔离性（Isolation）

针对多个用户同时操作，主要是排除其他事务对本次事务的影响

![这里写图片描述](https://img-blog.csdn.net/20180907101233416?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RlbmdqaWxp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

两个事务同时进行，其中一个事务读取到另外一个事务还没有提交的数据，B
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200111115243422.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RlbmdqaWxp,size_16,color_FFFFFF,t_70)

### 事务的隔离级别
### 脏读：
指一个事务读取了另外一个事务未提交的数据。



### 不可重复读：
在一个事务内读取表中的某一行数据，多次读取结果不同。（这个不一定是错误，只是某些场合不对）

页面统计查询值

点击生成报表的时候，B有人转账进来300（事务已经提交）

### 虚读(幻读)
是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致。
（一般是行影响，多了一行）


