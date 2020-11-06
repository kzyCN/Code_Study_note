## 检索数据

> 检索数据使用select语句

[TOC]

### 基础查询

#### 检索单个列

```SQL
select 字段名
from 表名
```

#### 检索多个列

```SQL
select 
from 表名
```

#### 检索所有列

```SQL
select *
from 表名
```

#### 去重(distinct关键字)

`distinct`关键字去重

```SQL
select distinct 字段名
from 表名
```

#### 限制结果的查询

`limit`返回第一行或者前几行

`limit 5,5`表示返回从行5开始的5行，第一个数为开始位置，第二个数为要检索的行数

```SQL
select 字段名
from 表名
limit 5
```

### 排序检索数据

#### order by子句

`order by`子句取一个或多个字段进行排序

```SQL
select 字段名
from 表名
order by 字段名
```

`desc`降序排序

`asc`升序排序

```SQL
select 字段名
from 表名
order by desc 字段名
```

### 数据过滤

#### `where`子句

> `where`子句的位置
>
> 在同时使用`order by`子句和`where`子句，应该让`order by `位于`where`之后

```SQL
select 字段名
from 表名
where 过滤条件
```

#### `where`子句操作符

| 操作符  | 说明               |
| ------- | ------------------ |
| =       | 等于               |
| <>      | 不等于             |
| ！=     | 不等于             |
| <       | 小于               |
| <=      | 小于等于           |
| >       | 大于               |
| > =     | 大于等于           |
| between | 在指定的两个值之间 |

#### 空值检查

`null`空

`not null`非空

```SQL
select 字段名
from 表名
where 过滤条件 null (not null)
```

#### 组合where语句（and,or,in,not）

1. `and`操作符
2. `or`操作符
3. `in`操作符
4. `not`操作符（否定它后面所跟的任何条件）

`in`操作符用来指定条件范围，范围中的每个条件都可以匹配

```SQL
select 字段名
from 表名
where 字段 in(范围1,范围2)
```

### 用通配符进行过滤

#### like操作符

为在搜索子句中使用通配符，必须使用like操作符

#### 通配符

1. 百分号（%）通配符，百分号表示任何字符出现任意次数
2. 下划线（_）通配符，下划线表示只匹配单个字符而不是多个字符

### 使用正则表达式进行搜索

#### 使用`regexp`关键字进行正则表达式搜索

```SQL
select 字段名
from 表名
where 字段 regexp 正则表达式
```

`regexp`后面跟正则表达式

