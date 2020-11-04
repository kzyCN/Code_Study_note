# MySQL查询

> MySQL 数据库使用SQL SELECT语句来查询数据。
[TOC]
## 基础查询

> 基本语法

```sql
SELECT 字段名 FROM 表名;
```

### 查询多个字段


> 查询语句中你可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件。

```SQL
SELECT column_name,column_name FROM table_name;
```

### 查询全部字段

> 你可以使用星号（*）来代替其他字段，SELECT语句会返回表的所有字段数据

```SQL
SELECT * FROM 表名;
```

### 查询函数

> 可以查询函数

```SQL
SELECT VERSION();
```

### 起别名

> mysql可以给字段起别名方便理解
>
> 如果要查询的字段有重名情况，可以使用别名区分开来
>
> AS可以省略

```SQL
SELECT 字段名 AS 别名 FROM 表名;
#---
SELECT 字段名  别名 FROM 表名;
```

- 可以通过 as 给**表**起**别名**

```sql
-- 如果是单表查询 可以省略表名
select id, name, sex from t_student;

-- 表名.字段名
select t_student.id,t_student.name,t_student.sex from t_student;

-- 可以通过 as 给表起别名 
select s.id,s.name,s.sex from t_student as s;
```

### 去重

> 在字段名前加上distinct（翻译：不同的）

```SQL
SELECT DISTINCT 字段名 FROM 表名;
```
### 加+号的作用

> mysql中的加号只有一个作用：运算符

- 两个操作数都为数值型，则做加法运算
- 只要其中一方为字符型，试图将字符型数值转换成数值型
- 如果转换成功，则继续做加法运算
- 如果转换失败，则将字符型数值转换成0
- 只要其中一方为null，则结果肯定为null

---

## 条件查询

> 使用where子句对表中的数据筛选，结果为true的行会出现在结果集中

```SQL
SELECT 字段名 FROM 表名 WHERE 筛选条件;
```

- 语法如下：

```sql
select * from 表名 where 条件;
例：
select * from t_student where id=1;
```

- where后面支持多种运算符，进行条件的处理
  - 比较运算符
  - 逻辑运算符
  - 模糊查询
  - 范围查询
  - 空判断

### 按条件表达式筛选

```SQL
条件运算符：< > == != <= >=
```

### 按逻辑表达式筛选

```SQL
&& || !
and or not
```

### 操作符

- and操作符，用来指示检索满足所有给定条件的行
- or操作符，指示检索匹配任意条件的行
- [in操作符](#3.in操作符)

### 通配符

- 百分号（%）通配符，表示任何字符出现任意次数，包含0个字符
- 下划线（_）通配符，只匹配单个字符，不包含0个字符

### 模糊查询

```SQL
like
between and
in
is null
```

#### 1.like关键字

> 模糊查询


```SQL
# 查询包含字符a的字段信息
SELECT 字段名 FROM 表名 WHERE 字段名 LIKE '%a%';
```

#### 2.between and关键字

> 访问查询

```SQL
SELECT 字段名 FROM 表名 WHERE 字段名 >=100 AND 字段名 <=120;
# 等价于
SELECT 字段名 FROM 表名 WHERE 字段名 BETWEEN 100 AND 120;
```

#### 3.in操作符

> in用来指定条件范围，范围中的每个条件都可以进行匹配

```SQL
SELECT 字段名 FROM 表名 WHERE 字段名 >=100 IN (1,2);
```

#### 4.is null关键字

> 查询某个字段为null的字段

```SQL
# 查询字段2为空的字段1
SELECT 字段1,字段2 FROM 表 WHERE 字段2 IS NULL;
```

### 安全等于

> 1.可作为普通运算符的=
> 2.也可以用于判断是否是NULL 

```SQL
where 字段 is NULL/(is not NULL) ->where 字段 <=>NULL
```

- 注意：`null` 与 `''` 是不同的
- 判断空  `is null`

例13：查询没有填写身高的学生

```sql
select * from t_student where height is null;
```

- 判断非空 `is not null`

例14：查询填写了身高的学生

```sql
select * from t_student where height is not null;
```

例15：查询填写了身高的男生

```sql
select * from t_student where height is not null and sex=1;
```

## 排序查询

### 语法

```sql
select * from 表名 order by 列1 asc|desc [,列2 asc|desc,...]
```

### 说明

- `orderb by` 排序
- 将行数据按照列1进行排序，如果某些行列1的值相同时，则按照列2排序，以此类推
- 默认按照列值从小到大排列 `asc`
- `asc` 从小到大排列，即**升序**
- `desc` 从大到小排序，即**降序**

#### 降序排序

> DESC关键字

```SQL
SELECT 字段名 FROM 表名 WHERE 字段名 ORDER BY DESC;
```

### 升序排序

> ASC关键字

```SQL
SELECT 字段名 FROM 表名 WHERE 字段名 ORDER BY ASC;
```

## 聚合函数

为了快速得到**统计数据**，经常会用到如下5个聚合函数

### 总数

- `count(*)` 表示计算总行数，括号中写星与列名，结果是相同的

例1：查询学生总数

```sql
select count(*) from t_student;

-- 统计男生的人数
SELECT COUNT(*) FROM t_student WHERE sex='男';
```

### 最大值

- `max(列)`  表示求此列的最大值

例2：查询女生的编号最大值

```sql
select max(id) from t_student where sex=2;
```

### 最小值

- `min(列)` 表示求此列的最小值

例3：查询未删除的学生最小编号

```sql
select min(id) from t_student where is_delete=0;
```

### 求和

- `sum(列)` 表示求此列的和

例4：查询男生的总年龄

```sql
select sum(age) from t_student where sex=1;

-- 平均年龄
select sum(age)/count(*) from t_student where sex=1;
```

### 平均值

- `avg(列)` 表示求此列的平均值

例5：查询班级女生平均身高

```sql
SELECT AVG(height) FROM t_student WHERE sex='女';
```

## 分组查询

### group by

- `group by` 的含义: 将查询结果按照1个或多个字段进行分组，**字段值相同的为一组**
- `group by` 可用于单个字段分组，也可用于多个字段分组

例1：按性别对学生分组

```sql
select sex from t_student group by sex;
```

根据 `sex` 字段来分组， `sex` 字段的全部值有3个 '男', '女',,'保密'，所以分为了3组 当 `group by` 单独使用时，只**显示出每组的第一条记录**, 所以  `group by` 单独使用时的实际意义不大

### group by + 集合函数

- 通过 **聚合函数** 和 **分组** 来对 **值的集合** 做一些操作

例2：统计不同 **性别分组** 的 **平均年龄**

```sql
select sex,avg(age) from t_student group by sex;
```

例3：统计不同 **性别分组** 的 **人数**

```sql
select sex,count(*) from t_student group by sex;
```

### group by + having

- `having` 条件表达式：用来**分组查询后**指定一些条件来输出查询结果
- `having` 作用和 `where` 一样，但 `having` 只能用于 `group by`

例4：统计不同 性别分组的 **人数大于2人的组**

```sql
select sex,count(*) from t_student group by sex having count(*)>2;
```

### group by + with rollup

- `with rollup` 的作用是：在最后新增一行，来记录当前列里所有记录的总和

```sql
select sex,count(*) from t_student group by sex with rollup;
```

### group by + group_concat()

- `group_concat(字段名)`  可以作为一个输出字段来使用，
- 表示分组之后，根据分组结果，使用 `group_concat()` 来放置每一组的某字段的值的集合

```sql
select sex,group_concat(name) from t_student group by sex;
select sex,group_concat(id) from t_student group by sex;
```

## 分页查询

