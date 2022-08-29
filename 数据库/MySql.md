# 基础

## DDL(数据定义语言)

### 一、数据库操作

1.创建数据库
```mysql
CREATA DATABASE 数据库名;
```

2.查询数据库
```mysql
SHOW DATABASES;
```

3.删除数据库
```mysql
DROP DATABASE 数据库名;
```

4.使用数据库
```mysql
USE 数据库名;
```

5.查看正在使用的数据库
```mysql
SELECT DATABASE();
```


### 二、表操作

#### 对表操作

1.查询当前数据库所有表

```mysql
SHOW TABLES;
```

2.查询表结构

```mysql
DESC 表名;
```

3.查询指定表的的建表语句

```mysql
SHOW CREATE TABLE 表名;
```

4.修改表名

```mysql
ALTER TABLE 原表名 RENAEME TO/AS 新表名; 
```

5.删除表

```mysql
DROP TABLE [IF EXISTS] 表名;
```

6.创建表

```mysql
CREATE TABLE 表名(
    字段1 字段1类型 [COMMENT 字段1注释],
    字段2 字段2类型 [COMMENT 字段2注释],
    ...
    字段n 字段n类型 [COMMENT 字段n注释]
)[ COMMENT 表注释 ];
```
*ps:最后一个字段后面没有逗号。*

7.删除指定表并重新创建该表

```mysql
TRUECATE TABLE 表名;
```
#### 对字段操作

##### 1.添加字段

```mysql
ALTER TABLE 表名 ADD 字段名 数据类型(长度); 
```
##### 2.修改字段名和字段类型
```mysql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 数据类型(长度) [COMMENT 注释] [约束];
```
##### 3 .删除字段
```mysql
ALTER TABLE 表名 DROP 字段名;
```


## DML(数据操作语言)

### 添加数据
1.给指定字段添加数据
```mysql
INSERT INTO 表名(字段名1,字段名2,···) VALUE(值1,值2···);
```

2.给全部字段添加数据
```mysql
INSERT INTO VALUE(值1,值2···);
```
3.批量添加数据
```mysql
INSERT INTO 表名(字段名1,字段名2,···) VALUE(值1,值2···),VALUE(值1,值2···);
```
```mysql
INSERT INTO VALUE(值1,值2···),VALUE(值1,值2···);
```

### 修改数据
```mysql
UPDATE 表名 SET 字段名1 = 值1,字段名2 = 值2 ··· [WHERE 条件];
```
*ps:没有条件会修改整张表的数据。*

### 删除数据
```mysql
DELETE FROM 表名 [WHERE 条件];
```
*ps：同理没有where会删全部*



## DQL(数据查询语言)
- 语法（编写顺序）
```mysql
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组之后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

- 执行顺序
FROM  ->  WHERE  ->  GROUP BY  ->  SELECT  ->  ORDER BY  ->  LIMIT
### 基本查询 
#### 查询多个字段

1.查询部分字段
```mysql
SELECT 字段1,字段1,字段2,字段3 ··· FROM 表名;
```
2.查询全部字段

```mysql
SELECT * FROM 表名;
```

#### 设置别名
```mysql
SELECT 字段1[别名1],字段2[别名2] ··· FROM 表名;
```

#### 去除重复记录
```mysql
SELECT DISTINCT 字段列表 FROM 表名;
```
### 条件查询

| 比较运算符          | 功能                                        |
| ----------------- | ------------------------------------------- |
| BETWEEN ... AND ... | 在某个范围内（含最小、最大值）              |
| IN(...)             | 在in之后的列表中的值，多选一                |
| LIKE 占位符         | 模糊匹配（\_匹配单个字符，%匹配任意个字符） |
| IS NULL             | 是NULL                                      |

**比较运算符中的大于、小于等的语法 和 逻辑运算符的语法 与C/C++完全相同**

### 聚合函数

语法：
```mysql
SELECT 聚合函数(字段列表) FROM 表名;
```

常见聚合函数：

| 函数  | 功能     |
| ----- | -------- |
| count | 统计数量 |
| max   | 最大值   |
| min   | 最小值   |
| avg   | 平均值   |
| sum   | 求和     |

*ps : null 不参与所有聚合函数的运算。*
### 分组查询

1.不使用别名

```mysql
SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];
```
1.使用别名

```mysql
SELECT 字段列表 别名 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];
```
使用别名后后面所有该字段的条件使用别名替代
例如：

* 使用别名

```mysql
SELECT adress, count(*) address_count FROM emp WHERE age < 45 GROUP BY workadress HAVING address_count > 1;
```
* 不适用别名

```mysql
SELECT adress, count(*)  FROM emp WHERE age < 45 GROUP BY workadress HAVING count(*) > 1;
```
*ps:where 和 having 的区别：*

- 执行时机不同：where是分组之前进行过滤，不满足where条件不参与分组；having是分组后对结果进行过滤。
- 判断条件不同：where不能对聚合函数进行判断，而having可以。

### 排序查询
语法：
```mysql
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1, 字段2 排序方式2 ···;
```

排序方式 ：
ASC ： 升序
DESC ：降序

*如果是多字段排序, 当第一个字段值相同时, 才会根据第二个字段进行排序。*

### 分页查询
语法
```mysql
SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;
```

ps:
- 起始索引从0开始，起始索引 = （查询页码 - 1） * 每页显示记录数。此处类比数组。
- 分页查询是数据库的方言，不同数据库有不同实现，MySQL是LIMIT
- 如果查询的是第一页数据，起始索引可以省略，直接简写 LIMIT 10

## DCL(数据控制语言)
### 管理用户
1.查询用户
```mysql
USE mysql;
SELECT * FROM user;
````
2.创建用户
```mysql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
````
3.修改用户密码
```mysql
ALTER USER '用户名'@'主机名'  IDENTIFIED WITH mysql_native password BY '新密码';
````
4.删除用户

```mysql
DROP USER '用户名'@'主机名';
```
*ps：*
* 主机名可以用%进行通配。
* 这类SQL开发人员操作比较少，主要是DBA（数据库管理员）使用。