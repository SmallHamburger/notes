---
title: 笔记-MySQL使用
tags: [MySQL,数据库]
date: 2017/4/20
categories: 技术类
---

# MySQL学习笔记

***

## MySQL提示符

>`prompt \u`

| 参数 |     描述     |
|:-----|--------------|
|  \D  |  完整的日期  |
|  \d  |  当前数据库  |
|  \h  |  服务器名称  |
|  \u  |  当前用户名  |

***
***

## MySQL语句规范
1. 关键字和函数名全部**大写**
2. 数据库名称,表名称,字段名称全部**小写**
3. SQL语句必须以`;`结尾

***
***

## MySQL数据类型

### 整型

|  数据类型  | 存储字节 |
|:-----------|:--------:|
|   TINYINT  |     1    |
|  SMALLINT  |     2    |
|  MEDIUMINT |     3    |
|     INT    |     4    |
|   BIGINT   |     8    |

### 浮点型

|      数据类型     | 存储字节|
|:------------------|:-------:|
|   FLOAT[(M, D)]   |    4    |
|   DOUBLE[(M, D)]  |    8    |

### 日期

|    日期类型   |                  范围                    |
|:--------------|:----------------------------------------:|
|      YEAR     |    年份yyyy                              |
|      TIME     |    时间(hh-mm-ss)                        |
|      DATE     |    日期(yyyy-mm-dd)                      |
|    DATETIME   |    日期与时间組合(yyyy-mm-dd hh-mm-ss)   |
|   TIMESTAMP   |    yyyymmddhhmmss                        |

### 字符串

|               数据类型            |          最大长度         |
|:----------------------------------|:-------------------------:|
|   CHAR(M)                         |    255                    |
|   VARCHAR(M)                      |    255                    |
|   TINYTEXT                        |    255                    |
|   TEXT                            |    65535                  |
|   MEDIUMTEXT                      |    2^24 - 1               |
|   LONGTEXT                        |    2^32 - 1               |
|   ENUM('value1', 'value2', ...)   |    集合最大数目为65535    |
|   SET('value1', 'value2', ...)    |    集合最大数目为64       |

***
***

## MySQL常用命令

### 常用MySQL数据库信息

#### 显示当前服务器版本
>`SELECT VERSION();`

#### 显示当前日期
>`SELCET NOW();`

#### 显示当前用户
>`SELECT USER();`

#### 查看打开的数据库
>`SELECT DATABASE();`

#### 查找记录
>`SELECT FROM expr, ... FROM tbl_name;`

#### 打开数据库
>`USE db_name;`

#### 修改结束符, 将结束符由';'转换为'//'

`DELIMITER //`

***

### 查看数据库/表信息

#### 查看服务器下的数据库列表
>`SHOW {DATABASES | SCHEMA} [LIKE 'pattern' | WHERE expr]};`

#### 查看创建数据库的语句以及编码方式
>`SHOW CREATE DATABASE db_name;`

#### 查看(当前)数据库的数据表
>`SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr];`

#### 查看数据表的结构
>`SHOW COLUMNS FROM tbl_name;`

#### 查看索引
>`SHOW INDEXES FROM provinces[\G];`

#### 设置当前输入编码方式, 不会影响数据库的编码
>`SET NAMES GBK;`

***

### 创建数据库/表

#### 创建数据库
>`CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] character_name;`

#### 创建数据表
>`CREATE TABLE [IF NOT EXISTS] jeson( id INT UNSIGNED, name VARCHAR(30), age TINYINT UNSIGNED, salary FLOAT(8,2) UNSIGNED );`

#### 创建带有外键的数据表
1. 先创建父表

	>`CREATE TABLE provinces (id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT, name VARCHAR(30) NOT NULL);`
2. 创建子表

	>`CREATE TABLE users(id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT, name VARCHAR(30) NOT NULL,pid SMALLINT UNSIGNED, FOREIGN KEY (pid) REFERENCES provinces (id));`

***

### 修改数据表

#### 修改数据的编码方式
>`ALTER {DATABASE | SCHEMA} [db_name] [DEFAULT] CHARACHTER SET [=] character_name;`

#### 修改数据表名称
>`ALTER TABLE tbl_name RENAME [TO | AS] new_tbl_name;`

>`RENAME TABLE tbl_name TO new_tbl_name [, tbl_name2 TO new_tbl_name2, ...]`

#### 添加单列, 可以指定列的位置
>`ALTER TABLE tbl_name ADD [COLUMN] col_name col_definition [FIRST | AFTER col_name];`

#### 添加多列, 不可以指定列的位置
>`ALTER TABLE tbl_name ADD [COLUMN] (col_name1 col_definition , col_name2 col_definition, ...);`

#### 删除一列或多列
>`ALTER TABLE tbl_name DROP [COLUMNS] col_name [, DROP [COLUMNS] col_name1, DROP [COLUMNS] col_name2, ...];`

#### 添加主键约束, 只能有一个
>`ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] PRIMARY KEY [index_type] (index_col_name, ...);`

#### 添加唯一约束, 可以有多个
>`ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] UNIQUE [INDEX|KEY]  [index_name] [index_type] (index_col_name, ...);`

#### 添加外键约束
>`ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] FOREIGN KEY  [index_name] (index_col_name, ...) REFERENCES tbl_name (index_col_name);`

#### 添加/删除**默认约束**
>`ALTER TABLE tbl_name ALTER [COLUMNS] COL_NAME {SET DEFAULT literal | DROP DEFAULT};`

#### 删除主键约束
>`ALTER TABLE tbl_name DROP PRIMARY KEY;`

#### 删除唯一约束
>`ALTER TABLE tbl_name DROP {INDEX | KEY} index_name;`

#### 删除外键约束
>`ALTER TABLE tbl_name DROP FOREIGN KEY fk_symbol;`

#### 修改列定义, 数据类型从大类型改为小类型时会造成数据丢失
>`ALTER TABLE tbl_name MODIFY [COLUMNS] col_name col_definition [FIRST | AFTER col_name];`

#### 修改列名称, 也包含列定义的功能
>`ALTER TABLE tbl_name CHANGE [COLUMNS] old_col_name new_col_name col_definition [FIRST | AFTER col_name];`

***

### 删除数据库/表/记录

### 删除数据库
>`DROP {DATABASE | SCHEMA} [IF EXISTS] db_name;`

### 删除数据表
>`DROP TABLE [IF EXISTS] tbl_name;`

### 删除数据表中的记录
>`DELETE FROM provinces WHERE id = 3;`

***

### 向数据表插入记录
>`INSERT [INTO] tabl_name [(col_name, ...)] VALUES(val, ...);`

***
***

## 字段约束

### 字段创建时约束

1. `NULL` 
	* 默认值可以为空
2. `NOT NULL` 
	* 默认值不可以为空
3. `AUTO_INCREMENT`
	* 自动编号, 且必须与主键组合使用
	* 默认情况下起始值为1, 每次的增量为1
4. `PRIMARY KEY`
	* 主键约束
	* 每张表只能存在一个主键
	* 主键保证记录的唯一性
	* 主键自动为`NOT NULL`
5. `UNIQUE KEY`
	* 唯一约束
	* 每张数据表可以存在多个唯一约束
	* 唯一约束可以保证记录的唯一性
	* 唯一约束的字段可以为`NULL`
6. `DEFAULT`
	* 字段的默认值
7. `FOREIGN KEY`
	* 保持数据一致性, 完整性
	* 实现一对一或一对多的关系
	* 外键约束的要求
		* 父表和子表必须使用相同的存储引擎, 而且禁止使用临时表
		* 数据表的存储引擎只能为**INNODB**
		* 外检列和参照列必须具有相似的数据类型. 其中数字的长度或者是否有符号位必须相同; 而字符的长度则可以不同
		* 外键列和参照列必须创建索引. 如果外键列不存在索引的话, MySQL将自动创建索引

### 外键约束的参照操作
1. `CASCADE` 
	* 从父表删除或更新时, 自动删除或更新子表中匹配的行
2. `SET NULL`
	* 从父表删除或更新时, 自动设置子表中的外键列为`NULL`, 如果使用该选项, 必须保证子表列没有指定`NOT NULL`
3. `RESTRICT`
	* 拒绝对父表的删除或更新操作
4. `NO ACTION`
	* 标准的SQL关键字, 与RESTRICT相同

>例:

```
CREATE TABLE provinces(
	id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT, 
	name VARCHAR(30) NOT NULL
);

CREATE TABLE users(
	id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT, 
	name VARCHAR(30) NOT NULL, 
	pid SMALLINT UNSIGNED,
	FOREIGN KEY (pid) REFERENCES provinces (id) ON DELETE CASCADE
);	
```

### 表级约束和列级约束

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;列级约束既可以在列定义时声明, 也可以在列定义后声明; 表级约束只能在列定义后声明.	

1. 表级约束

	对一个数据列建立的约束, 开发中经常使用

2. 表级约束

	对多个数据列建立的约束

## 记录操作

### 插入记录
1. `INSERT [INTO] tbl_name [(col_name, ...)] {VALUES | VALUE} ({expr | DEFAULT}, ...),(...),...;`
	* 插入时主键可以使用`NULL`或者`DEFALUT`使用自动的默认值
	* 拥有默认值得字段可以使用`DEFAULT`
	* 可以在`VALUES`后加多个记录
	* 可以在值中加入函数

>例:

```
INSERT users VALUES
	(DEFAULT, 'zhangsan', '1230', 20*8-100, '1'), 
	(NULL, 'lisi', md5('123'), DEFAULT, 2);

```

2. `INSERT [INTO] tbl_name SET col_name={expr | DEFAULT}, ...;`
	* 与第一种方式区别在于, 此方法可以使用子查询
	* 一次性只能插入一条数据

3. `INSERT [INTO] tbl_name [(col_name, ...)] SELECT ...;`
	* 此方法可以将查询结果插入到指定的数据表中

### 更新记录
1. 单表更新
	* `INSERT [LOW_PRIORUTY] [IGNORE] tbl_reference SET col_name1={expr | DEFAULT} [, col_name2={expr2|DEFAULT}] ... [WHERE where_condition];`

>例:

```
UPDATE users SET age = age + 10 WHERE id % 2 =0;
```

2. 多表更新
	* `UPDATE tbl_references SET col_name1 ={expr1 | DEFAULT} [, col_name2={expr2 | DEFAULT}], ... [WHERE where_condition];`
	* 语法结构
		* `tbl_reference {[INNER | CROSS] JOIN | {LEFT | RIGHT} [OUTER] JOIN} tbl_reference ON conditional_expr;`

> 例:
```MySQL
UPDATE goods INNER JOIN category ON goods_cate=name SET goods_cate=id;
```

### 删除记录
1. 单表删除
	* `DELETE FROM tbl_name [WHERE where_condition];`

2. 多表删除
	* `DELETE tbl_name[.*] [, tbl_name[.*]] ... FROM tbl_references [WHERE where_condition]`

> 例: 删除自身重复的数据
```MySQL
DELETE t1 FROM goods AS t1 LEFT JOIN
	(SELECT MIN(goods_id) goods_id, goods_name FROM goods GROUP BY goods_name HAVING COUNT(goods_name)>= 2) AS t2 
	ON t1.goods_name = t2.goods_name 
	WHERE t1.goods_id < t2.goods_id;
```

### 查找记录
1. 

```
SELECT select_expr [, select_expr2, ...]
[
	FROM tbl_references
	[WHERE where_condition]
	[GROUP BY {col_name | position} [ASC | DESC], ...]
	[HAVING where_condition]
	[ORDER BY {col_name | expr | position} [ASC | DESC], ...]
	[LIMIT {[offset, ] row_count | row_count OFFSET offset}]
];
```

* 查询表达式
	* 每一个表达式表示想要的一列, 必须至少有一个
	* 多个列之间以英文逗号分隔
	* 星号`*`表示所有列, `tbl_name.*` 可以表示命名表的所有列
	* 查询表达式可以使用`[ AS ] alias_name`为其赋予别名
	* 别名可用于`GROUP BY, ORDER BY, HAVING`子句

### 条件表达式
1. `WHERE expr;`
	* 对记录进行过滤, 如果没有指定`WHERE`子句, 则显示所有子句
	* 在`WHERE`表达式中,可以使用MySQL支持的函数或运算符

### 查询结果分组
1. `[GROUP BY {col_name | position} [ASC | DESC], ...];`

### 分组条件
1. `[HAVING where_condition];`
	* 根据部分记录进行分组
	* 条件必须出现在分组条件所选择的字段或者是聚合函数,如count()

### 对查询结果进行排序
1. `[ORDER BY {col_name | expr | position} [ASC | DESC], ...];`
	
### 限制查询结果返回的数量
1. `[LIMIT {[offset, ] row_count | row_count OFFSET offset}];`

## 查询操作

### 子查询

- 子查询(Subquery)是指出现在其他SQL语句内的`SELECT`子句

- 子查询指嵌套在查询内部, 且必须始终出现在**圆括号内**

- 子查询可以包含多个关键字或条件, 如`DISTINCT`,`GROUP BY`, `ORDER BY`, `LIMIT`, 函数等

- 子查询的外层查询可以是:`SELECT`, `INSERT`, `UPDATE`, `SET`, `DO`等

- 子查询可以返回标量, 一行, 一列或者子查询

> 例:

```MySQL
SELECT * FROM tb_name1 WHERE col_name1 = (SELECT col_name2 FROM tbl_name2);
```

#### 分类

##### 使用比较运算符的子查询

`=  >  <  >=  <=  <>  !=  <=>`

##### 用`ANY  SOME  ALL`修饰的比较运算符

| 运算符 \ 关键字 |  `ANY`  |  `SOME`  |  `ALL`   |
|:---------------:|:-------:|:--------:|:--------:|
|     `>, >=`     |  最小值 |  最小值  |  最大值  |
|     `<, <=`     |  最大值 |  最大值  |  最小值  |
|       `=`       |  任意值 |  任意值  |          |
|     `<>, !=`    |         |          |  任意值  |

> 例:

```MySQL
SELECT goods_name,brand_name, goods_price FROM goods WHERE goods_price <= ALL(SELECT goods_price FROM goods WHERE goods_cate = '超级本');
```
##### 使用`IN, NOT IN`的子查询

`IN` 作用等同于 `=ANY()`

`NOT IN` 作用等同于 `!=ALL()`

##### 使用`NOT EXIST, EXIST`的子查询

如果子查询返回任何行, `EXIST`则返回`TRUE`, 否则为`FALSE`

#### 将查询结果插入到数据表

```MySQL
INSERT [INTO] tbl_name [(col_name, ...)] SELECT ...
```

> 例:
```MySQL
INSERT category (name) (SELECT brand_name FROM goods);
```

### 创建表的同时将查询结果写入到数据表

```MySQL
CREATE TABLE [IF NOT EXIST] tbl_name [(create_definition, ...)] select_statement;
```

> 例:
```MySQL
CREATE TABLE brands (
	id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	brand_name VARCHAR(40) NOT NULL
)
(SELECT brand_name FROM goods GROUP BY brand_name);
```

### 连接

`tbl_reference {[INNER | CROSS] | { LEFT | RIGHT} [OUTER]} JOIN tbl_reference ON confitional_expr`

#### 内连接`INNER JOIN`/`JOIN`/`CROSS JOIN`
交集, 显示左表和右表符合连接条件的记录
> 例:
```MySQL
SELECT goods_id, goods_name, cate_name FROM goods INNER JOIN category ON goods.cate_id=category.cate_id;
```

#### 左外连接`LEFT [OUTER] JOIN`
显示左表的全部记录及右表符合连接条件的记录
```MySQL
SELECT goods_id, goods_name, cate_name FROM goods LEFT JOIN category ON goods.cate_id=category.cate_id;
```

#### 右外连接`RIGHT [OUTER] JOIN`
显示右表的全部记录及左表符合连接条件的记录
```MySQL
SELECT goods_id, goods_name, cate_name FROM goods RIGHT JOIN category ON goods.cate_id=category.cate_id;
```

#### 连接条件`ON`或者`WHERE`
通常使用`ON`关键字来设定连接条件, 使用`WHERE`关键字进行结果集记录的过滤

#### 多表连接
`tbl_reference ` `({[INNER | CROSS] | { LEFT | RIGHT} [OUTER]} JOIN tbl_reference ON confitional_expr)` * n

#### 自身连接

> 例:

```MySQL
SELECT c.type_id,c.type_name,p.type_name FROM tdb_goods_types c LEFT JOIN tdb_goods_types p ON c.parent_id=p.type_id;
```

```MySQL
SELECT p.type_id,p.type_name,c.type_name FROM tdb_goods_types p LEFT JOIN tdb_goods_types c ON c.parent_id=p.type_id;
```

```MySQL
SELECT p.type_id,p.type_name,COUNT(c.type_name) FROM tdb_goods_types p LEFT JOIN tdb_goods_types c ON c.parent_id=p.type_id GROUP BY  p.type_id;
```

### 无限级数据表设计
> 例:
```MySQL
CREATE TABLE tdb_goods_types(
	type_id   SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	type_name VARCHAR(20) NOT NULL,
	parent_id SMALLINT UNSIGNED NOT NULL DEFAULT 0
); 
```

## 函数

### 字符函数

| 函数名称      |  描述                          |
|---------------|--------------------------------|
|  CONCAT()     |  字符连接                      |
|  CONCAT_WS()  |  使用指定的分隔符进行字符连接  |
|  FORMAT()     |  数字格式化, 返回值为字符型    |
|  LOWER()      |  转换成小写字母                |
|  UPPER()      |  转换成大写字母                |
|  LEFT()       |  获取左侧n个字符               |
|  RIGHT()      |  获取右侧n个字符               |
|  LENGTH()     |  获取字符串的长度              |
|  LTRIM()      |  删除前导空格                  |
|  RTRIM()      |  删除后续空格                  |
|  TRIM()       |  删除前导和后续空格            |
|  SUBSTRING()  |  字符串截取                    |
|  [NOT] LIKE   |  模式匹配                      |
|  REPLACE()    |  字符串替换                    |

> 例: `CONCAT()`

```MySQL
SELECT CONCAT('HELLO', ' ', 'WORLD');
SELECT CONCAT(firstname, lastname) as fullname FROM users;
```

> 例: `CONCAT_WS()`

```MySQL
SELECT CONCAT_WS('.', 'firstname', 'lastname');
```

> 例: `FORMAT()`

```MySQL
SELECT FORMAT(123456.7890, 2);
```

> 例: `LOWER()``UPPER()`

```MySQL
SELECT LOWER('HELLO WORLD');
SELECT UPPER('hello world');
```
> 例: `LEFT()``RIGHT()`

```MySQL
SELECT LEFT('HELLO', 2);
SELECT RIGHT('WORLD', 2);
```

> 例: `TRIM()`

```MySQL
SELECT TRIM(LEADING '!' FROM '!! hello world !!'); //删除前导 !
SELECT TRIM(TRAILING'!' FROM '!! hello world !!'); //删除后续 !
SELECT TRIM(BOTH'!' FROM '!! hello world !!'); //删除前导和后续 !
```

> 例: `REPLACE()`

```MySQL
SELECT REPLACE('!! hello !! world !!', '!', '?');
```

> 例: `SUBSTRING()`

```MySQL
SELECT SUBSTRING('HELLO WORLD', 3);
SELECT SUBSTRING('HELLO WORLD', 3, 6);
```

> 例: `SUBSTRING()`

```
SELECT SUBSTRING('HELLO WORLD', 3);
SELECT SUBSTRING('HELLO WORLD', 3, 5);
SELECT SUBSTRING('HELLO WORLD', -1);
SELECT SUBSTRING('HELLO WORLD', -5, 2); //从尾部数第5个开始截取(包含), 截取2个
```

> 例: `LIKE()`

```MySQL
SELECT 'MySQL' LIKE 'MY%'; //%匹配零个至多个字符, _匹配一个字符, 返回值为true
SELECT 'MySQL%' LIKE '%1%%' ESCAPE '1'; //1后面的%不再认作通配符
```

### 数值运算符与函数

|   函数名称           |  描述                |
|:---------------------|:---------------------|
|   CEIL()             |  向上取整            |
|   FLOOR()            |  向下取整            |
|   ROUND()            |  四舍五入            |
|   DIV                |  整数除法            |
|   MOD                |  取余数(取模, %)     |
|   POWER()            |  幂运算              |
|   TRUNCATE()         |  数字截取                 |
|   [NOT] BETWEEN AND  |  [不]在范围之内,闭合区间  |
|   [NOT] IN()         |  [不]在列出值范围内  |
|   IS [NOT] NULL      |  [不]为空            |

> 例: `ROUND()`

```MySQL
SELECT ROUND(123456.789, 2);
```

> 例: `TRUNCATE()`

```MySQL
SELECT TRUNCATE(123456.789, 2); //没有四舍五入
SELECT TRUNCATE(123456.789, -1);
```

> 例: `[NOT] BETWEEN AND`

```MySQL
SELECT 6 BETWEEN 2 AND 10; //左右都闭合的区间
```

> 例: `IN()`

```MySQL
SELECT 6 IN(2, 4, 6, 8); 
```

### 日期时间函数

|   函数名称           |  描述            |
|:---------------------|:-----------------|
|   NOW()              |  当前日期和时间  |
|   CURDATE()          |  当前日期        |
|   CURTIME()          |  当前时间        |
|   DATE_ADD()         |  日期变化        |
|   DATEDIFF()         |  日期差值        |
|   DATE_FORMAT()      |  日期格式化      |

> 例: `DATE_ADD()`

```MySQL
SELECT DATE_ADD(CURDATE(), INTERVAL 365 DAY);
```

> 例: `DATEDIFF()`

```MySQL
SELECT DATEDIFF(CURDATE(), '2018-05-13');
```

> 例: `DATE_FORMAT()`

```MySQL
SELECT DATE_FORMAT(CURDATE(), '%m/%d/%Y');
```

### 信息函数

|   函数名称           |   描述                 |
|:---------------------|:-----------------------|
|   CONNECTION_ID()    |   连接ID               |
|   DATABASE()         |   当前数据库           |
|   LAST_INSERT_ID()   |   最后插入记录的ID号   |
|   USER()             |   当前用户             |
|   VERSION()          |   版本信息             |

### 聚合函数
返回值只有一个

|   函数名称   |   描述     |
|:-------------|:-----------|
|   AVG()      |   平局值   |
|   COUNT()    |   计数     |
|   MAX()      |   最大值   |
|   MIN()      |   最小值   |
|   SUM()      |   求和     |

### 加密函数

|   函数名称   |   描述                       |
|:-------------|:-----------------------------|
|   MD5()      |   信息摘要算法               |
|   PASSWORD() |   密码算法, 常用于修改密码   |

> 例: `MD5()`

```MySQL
SELECT MD5('admin');
```

> 例: `PASSWORD()`

```MySQL
SELECT PASSWORD('admin');
SET PASSWORD = PASSWORD('admin');
```
## 自定义函数
用户自定义函数(UDF)是对MySQL扩展的途径, 其用法与内置函数相同

两个必要条件: 
1. 参数, 非必须, 参数不能超过1024个
2. 返回值, 所有函数都有返回值

### 创建自定义函数

```MySQL
CREATE FUNCTION func_name
	RETURNS
	{STRING | INTEGER | REAL | DECIMAL}
	routine_dbody; 
```
#### 函数体

1. 函数体由合法的sql语句构成
2. 函数体可以是简单的SELECT或INSERT语句
3. 函数体如果为复合结构使用BEGIN...END语句
4. 符合结构可以包含声明, 循环, 控制结构

#### 创建不带参数的自定义函数

```MySQL
CREATE FUNCTION f1() RETURNS VARCHAR(30)
	RETURN DATE_FORMAT(NOW(), '%Y年%m月%d日 %H点%i分%s秒');
SELECT f1();
```

#### 创建带有参数的自定义函数

```MySQL
CREATE FUNCTION f2(num1 INT, num2 INT) RETURNS INT
	RETURN (num1 + num2)/2;
```

#### 创建具有复合结构的函数

```MySQL
DELIMITER //
CREATE FUNCTION adduser1(username VARCHAR(20))
	RETURNS INT UNSIGNED
	BEGIN
		INSERT users(username) VALUES (username);
		RETURN LAST_INSERT_ID();
	END
//
```
## 存储过程
将sql语句的执行过程存储起来

优点:
1. 增强sql语句的功能和灵活性
2. 实现较快的执行速度
3. 减少网络流量

### 创建存储过程

```MySQL
CREATE [DEFINER = {user | CURRENT_USER}]
PROCEDURE sp_name([proc_parameter[, ...]])
[characteristic ...] routine_body
```
pro_parameter:

```MySQL
[IN | OUT | INOUT] param_name type
```
参数:

1. `IN`: 表示该参数的值必须在调用存储过程时指定
2. `OUT`: 表示该参数的值可以被存储过程改变, 并且可以返回
3. `INOUT`: 表示该参数的调用时指定, 并且可以被改变和返回

过程体:

1. 过程体由合法的sql语句构成
2. 过程体可以使任意增删改查及连接sql语句
3. 过程体如果为复合结构则使用`BEGIN...END`语句
4. 符合结构可以包含声明, 循环, 控制结构

### 调用存储过程

```MySQL
CALL sp_name([parameter[, ...]]);
CALL sp_name[()];
```
### 修改存储过程

删了再建

### 创建没有参数的存储过程

```MySQL
CREATE PROCEDURE sp1() SELECT VERSION();
```

### 创建含有`IN`参数的存储过程

```MySQL
DELIMITER //
CREATE PROCEDURE removeUserByID(IN userID INT UNSIGNED)
BEGIN
DELETE FROM users WHERE id = userID ;
END
//
DELIMITER ;
CALL removeUserByID(3);
```

### 创建含有返回值的存储过程

```MySQL
DELIMITER //
CREATE PROCEDURE removeUserAndGetCountByID(IN userID INT UNSIGNED, OUT userNums INT UNSIGNED)
BEGIN
DELETE FROM users WHERE id = userID ;
SELECT count(id) FROM users INTO userNums;
END
//
DELIMITER ;
CALL removeUserAndGetCountByID(3, @userNums);
```

### 设置用户变量
用户全局变量, 只针对当前用户有效

`SET @userNums = 1;`

### 创建有多个`OUT`类型参数的存储过程

```MySQL
DELIMITER //
CREATE PROCEDURE removecount(IN userid INT UNSIGNED, OUT affectcount INT UNSIGNED, OUT countleft INT UNSIGNED) 
BEGIN 
DELETE FROM users WHERE id = userid; 
SELECT ROW_COUNT() INTO affectcount; 
SELECT COUNT(id) FROM users INTO countleft;
END
//
CALL removecount(14, @affectcount, @countleft);
SELECT @affectcount, @countleft;
```

### 返回被影响的行数

`SELECT ROW_COUNT();`

### 存储过程与自定义函数的区别

- 存储过程实现的功能要复杂些, 而函数的针对性更强
- 存储过程可以返回多个值, 而函数只能返回一个值
- 存储过程一般独立执行, 而函数可以作为其他sql语句的组成部分执行

### 注意事项

- 创建存储过程或者自定义函数时需要通过`DILIMITER`语句修改定界符
- 如果函数体或者过程体由多个语句, 需要包含在`BEGIN...END`语句块中
- 存储过程通过该`CALL`来调用

## 存储引擎
MySQL可以将数据以不同的技术存储在文件(内存)中, 这种技术就称为存储引擎

### 存储引擎特点

|   特点   | MyISAM | InnoDB | Memory | Archive|
|----------|:------:|:------:|:------:|:------:|
| 存储限制 | 256TB  |  64TB  |   有   |   无   |
| 事务安全 |   -    |  支持  |   -    |   -    |
| 支持索引 |  支持  |  支持  |  支持  |   -    |
| 锁颗粒   |  表锁  |  行锁  |  表锁  |  行锁  |
| 数据压缩 |  支持  |   -    |   -    |  支持  |
| 支持外键 |   -    |  支持  |   -    |   -    |

### 锁
- 共享锁(读锁)

	在同一时间段内, 多个用户可以读取同一个资源, 读取过程中数据不会发生任何变化

- 排它锁(写锁)

	在任何时候只能有一个用户写入资源, 当进行写锁时会阻塞其他的读锁或者写锁操作

### 锁颗粒
- 表锁

	一种开销最小的锁, 但并发不好
- 行锁

	一种开销最大的锁, 但并发性好

### 事务
用于保证数据库的完整性

### 事务的特性
- 原子性(Atomicity)
- 一致性(Consistency)
- 隔离性(Isolation)
- 持久性(Durability)

### 索引
是对数据表中一列或多列的值进行排序的一种结构

- 普通索引
- 唯一索引
- 全文索引
- btree索引
- hash索引

### 修改存储引擎
- 修改MySQL配置文件实现

	`default-storage-engine = engine`
- 通过创建数据表命令实现

	`CREATE TABLE tbl_name(...) ENGINE = engine;`
- 通过修改数据表命令实现

	`ALTER TABLE tbl_name ENGINE [=] engine_name;`
