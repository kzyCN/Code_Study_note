# MySQL查询

> MySQL 数据库使用SQL SELECT语句来查询数据。

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

```SQL
SELECT 字段名 FROM 表名 WHERE 筛选条件;
```

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

---

## 排序查询

### 指定排序方向

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

---

## 分组查询

