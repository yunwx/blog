---
title: MySQL数据库基础
date: 2025-01-08 12:55:45
tags: MySQL
categories: 数据库
---


## **一、SQL通用语法**

1.SQL语句可以单行或多行书写，以分号结尾
2.SQL语句可以使用空格/缩进来增强语句的可读性
3.MySQL数据库的SQL语句不区分大小写，关键字建议使用大写
4.注释：
- 单行注释：--注释内容 或 #注释内容（MySQL特有）
- 多行注释：/* 注释内容 */
  

## **二、DDL（数据定义语言）**

### 1.数据库操作

(1).查询
- 查询所有数据库：SHOW DATABASES;
- 查询当前数据库：SELECT DATABASE();
  
(2).创建
- CREATE DATABASE [ IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则]；
  
(3).删除
- DROP DATABASE [IF EXISTS] 数据库名；
  
(4).使用
- USE 数据库名；
  

### 2.表操作

(1).查询
- 查询当前数据库所有表：SHOW TABLES;
- 查询表结构：DESC 表名；
- 查询指定表的建表语句：SHOW CREATE TABLE 表名；
  
(2).创建
- CREATE TABLE 表名（
字段1 字段1类型[COMMENT 字段1注释],
字段2 字段2类型[COMMENT 字段2注释],
字段3 字段3类型[COMMENT 字段3注释],
.......
字段n 字段n类型[COMMENT 字段n注释]
     ）[COMMENT 表注释]；

<span style="color:#FF6B6B">注：最后一个字段后面没有逗号</span>

(3).修改
- 添加：ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束]；
- 修改：
	- 修改数据类型：ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度)；
	- 修改字段名和字段类型：ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束]；
	- 删除字段：ALTER TABLE 表名 DROP 字段名；
	- 修改表名：ALTER TABLE 表名 RENAME TO 新表名；
	  

### 3.数据类型

(1).数值类型：
  >TINYINT：1 byte
  SMALLINT：2 bytes
  MEDIUMINT：3 bytes
  INT或INTEGER：4 bytes
  BIGINT：8 bytes
  FLOAT：4 bytes
  DOUBLE：8 bytes
  DECIMAL：依赖于M（精度）和 D（标度）的值
  
<span style="color:#FF6B6B">注：UNSIGNED表明无符号，即不能为负(例：age tinyint unsigned)</span>

(2).字符串类型：
  >CHAR：0-255 bytes 定长字符串
  VARCHAR：0-65535 bytes 变长字符串
  TINYBLOB：0-255 bytes 不超过255个字符的二进制数据
  TINYTEXT：0-255 bytes 短文本字符串
  BLOB：0-65535 bytes 二进制长文本数据
  TEXT：0-65535 bytes 长文本数据
  MEDIUMBLOB：0-16777215 bytes 二进制形式的中等长度文本数据
  MEDIUMTEXT：0-16777215 bytes 中等长度文本数据
  LONGBLOB：0-4294967295 bytes 二进制形式的极大文本数据
  LONGTEXT：0-4294967295 bytes 极大文本数据

<span style="color:#FF6B6B">注：CHAR和VARCHAR区别：假设声明char(10)和varchar(10)，char即便存储1个字符也会占用10个字符的空间，未占用的空间会用空格填充；varchar存储1个字符就占用1个字符的空间，会根据你存储的内容去计算所需空间。所以char的性能更好，而varchar更省内存</span>

(3).日期类型：
 > DATE：YYYY-MM-DD 日期值
  TIME：HH:MM:SS 时间值或持续时间
  YEAR：YYYY 年份值
  DATETIME：YYYY-MM-DD HH:MM:SS 混合日期和时间值
  TIMESTAMP：YYYY-MM-DD HH:MM:SS 混合日期和时间值，时间戳
  

## **三、DML（数据操作语言）**

### 1.添加数据

(1).给指定字段添加数据：INSERT INTO 表名 (字段1,字段2,...) VALUES(值1,值2,...);
(2).给全部字段添加数据：INSERT INTO 表名 VALUES(值1,值2,...);
(3).批量添加数据：
- INSERT INTO 表名 (字段名1,字段名2,...) VALUES (值1,值2,...),(值1,值2,...),(值1,值2,...);
- INSERT INTO 表名 VALUES (值1,值2,...),(值1,值2,...),(值1,值2,...);

<span style="color:#FF6B6B">注：</span>
- <span style="color:#FF6B6B">插入数据时，指定的字段顺序需要与值的顺序是一一对应的</span>
- <span style="color:#FF6B6B">字符串和日期型数据应该包含在引号中</span>
- <span style="color:#FF6B6B">插入的数据大小应该在字段的规定范围内</span>

### 2.修改数据

(1).修改：UPDATE 表名 SET 字段名1=值1,字段名2=值2,...[WHERE 条件];
(2).删除：DELETE FROM 表名 [WHERE 条件];

<span style="color:#FF6B6B">注：</span>
- <span style="color:#FF6B6B">DELETE 语句的条件可以有，也可以没有，如果没有条件，就会删除这张表所有数据</span>
- <span style="color:#FF6B6B">DELETE 语句不能删除某一个字段的值(可以使用UPDATE，直接将该字段修改为null)</span>
  

## **四、DQL（数据查询语言）**

### 1.语法

SELECT 字段列表 FROM 表名列表 WHERE 条件列表 GROUP BY 分组字段列表 HAVING 分组后条件列表 ORDER BY 排序字段列表 LIMIT 分页参数

### 2.基本查询

(1).查询多个字段：
- SELECT 字段1,字段2,字段3... FROM 表名;
- SELECT * FROM 表名;
  
(2).设置别名：SELECT 字段1 [AS 别名1],字段2 [AS 别名2]... FROM 表名;
(3).去除重复记录：SELECT DISTINCT 字段列表 FROM 表名;

### 3.条件查询

(1).语法：SELECT 字段列表 FROM 表名 WHERE 条件列表;
(2).条件：
   
	    
 - 比较运算符
  >`>`：大于
 `>=`：大于等于
 `<`：小于
 `<=`：小于等于
`=`：等于
 `<>` 或 `!=`：不等于
 `BETWEEN...AND...`：在某个范围之内（含最小、最大值，`BETWEEN`后跟最小值，`AND`后跟最大值）
 `IN(...)`：在`IN`之后的列表中的值，多选一
 `LIKE`占位符：模糊匹配（`_`匹配单个字符，`%`匹配任意个字符）
`IS NULL`：是`NULL`

  
 - 逻辑运算符

   >AND 或 &&：并且（多个条件同时成立）
   OR 或 ||：或者（多个条件任意一个成立）
   NOT 或 ！：非，不是
  

### 4.聚合函数

(1).介绍：将一列数据作为一个整体，进行纵向计算
(2).常见聚合函数
>count：统计数量
  max：最大值
  min：最小值
  avg：平均值
  sum：求和
  
(3).语法：SELECT 聚合函数(字段列表) FROM 表名;

<span style="color:#FF6B6B">注：null值不参与所有聚合函数运算</span>

### 5.分组查询

(1).语法：SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];
(2).WHERE 与 HAVING区别
- 执行时机不同：WHERE是分组前进行过滤，不满足WHERE条件，不参与分组；HAVING是分组后对结果进行过滤
- 判断条件不同：WHERE不能对聚合函数进行判断，而HAVING可以
  

### 6.排序查询

(1).语法：SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1,字段2 排序方式2;
(2).排序方式：
- ASC：升序(默认值)
- DESC：降序

<span style="color:#FF6B6B">注：如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序</span>

### 7.分页查询

(1).语法：SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询记录数;

<span style="color:#FF6B6B">注：</span>
- <span style="color:#FF6B6B">起始索引从0开始，起始索引 = (查询页码-1) * 每页显示记录数</span>
- <span style="color:#FF6B6B">分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT</span>
- <span style="color:#FF6B6B">如果查询的是第一页数据，起始索引可以省略 </span>