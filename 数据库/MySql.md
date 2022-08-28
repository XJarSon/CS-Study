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
语法
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