# MySQL常用函数

> 类似于Java的方法，封装作用

调用：

```SQL
select 函数名();
```

分类：

- 单行函数
- 分组函数（做统计用）

## 单行函数

> 字符函数
> 数学函数
> 日期函数
> 其他函数
> 流程控制函数

### 字符函数

`length`获取参数值的字节个数

```SQL
SELECT LENGTH ('JOHN')
```

`conact`拼接字符串

```SQL
SELECT CONACT(字段,'_',字段) FROM 表名
```

`upper`,`lower`改变大小写

```sql
#变大写
SELECT UPPER('john')
#变小写
SELECT LOWER('joHn')
```

`substr`,`substring`从特定位置开始的字符串返回一个给定长度的子字符串（**MySQL索引从1开始**）

```SQL
substr(string string,num start,num length);

string为字符串；
start为起始位置；
length为长度。

#截取从指定索引处后面所有字符
SELECT SUBSTR('MySQL索引从1开始'，4)out_put;
#截取从指定索引处指定字符长度的字符
SELECT SUBSTR('MySQL索引从1开始'，1,3) out_put;

substring（str, pos） 
substring（被截取字段，从第几位开始截取）
substring（str, pos, length） 
substring（被截取字段，从第几位开始截取，截取长度） 
```

`instr`获取子串第一次出现的索引，如果没有找到，则返回0（从1开始）

```SQL
INSTR（str,substr） / instr(源字符串, 目标字符串)

参数说明：

str：从哪个字符串中搜索；
substr:要搜索的子字符串。

例：INSTR('apple','a')，返回的查询结果是1（a出现在字符串‘apple’中的第一个索引；
```

### 数学函数

`round`四舍五入

```SQL
select round(数值);
select round(数值，位数);
```

`ceil`向上取整

```SQL
select ceil(数值);
```

`floor`向下取整

```SQL
select floor(数值);
```

`truncate`截断

```SQL
select truncate(1.69999,1);
```

`mod`取余

```SQL
select mod(数值，数值);
```

`count()`总和

```SQL
SELECT count(*) from 表名
```

### 日期函数

`now`返回当前系统日期＋时间

```SQL
select now();
```

`curdate`返回当前系统日期，不包括时间

```SQL
select curdate();
```

`curtime`返回当前系统时间，不返回日期

```SQL
select curtime();
```

`str_to_date`将字符通过指定格式转换成日期

```SQL
select str_to_date('1999-1-1','%Y-%c-%d');
```

![image-20200809102435579](https://gitee.com/kzycn/picCloud/raw/master/20200809102442.png)

### 

