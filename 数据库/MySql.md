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

7.查询表的数据
```mysql
SELECT 字段1,字段2 ··· FROM 表名; 
```

8.删除指定表并重新创建该表

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

*ps:*
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

### 权限控制
1.查询权限
```mysql
SHOW GRANTS FOR '用户名'@'主机名';
```
2.授予权限
```mysql
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
```
3.撤销权限

```mysql
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```
*ps ：*

- 多个权限之间用逗号分隔
- 授权时，数据库名和表名可以使用 * 进行通配， 代表所有。

## 函数
*ps : 本处及其简单，看看就好（逃*

- 字符串函数
- 数值函数
- 日期函数
- 流程函数

### 字符串函数

常用函数：

| 函数  | 功能  |
| ------------ | ------------ |
| CONCAT(s1, s2, ..., sn)  | 字符串拼接，将s1, s2, ..., sn拼接成一个字符串  |
| LOWER(str)  | 将字符串全部转为小写  |
| UPPER(str)  | 将字符串全部转为大写  |
| LPAD(str, n, pad)  | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度  |
| RPAD(str, n, pad)  | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度  |
| TRIM(str)  | 去掉字符串头部和尾部的空格  |
| SUBSTRING(str, start, len)  | 返回从字符串str从start位置起的len个长度的字符串  |
### 数值函数

常见函数：

| 函数  | 功能  |
| ------------ | ------------ |
| CEIL(x)  | 向上取整  |
| FLOOR(x)  | 向下取整  |
| MOD(x, y)  | 返回x/y的模  |
| RAND() | 返回0~1内的随机数 |
| ROUND(x, y) | 求参数x的四舍五入值，保留y位小数 |

### 日期函数

常用函数：

| 函数  | 功能  |
| ------------ | ------------ |
| CURDATE()  | 返回当前日期  |
| CURTIME()  | 返回当前时间  |
| NOW()  | 返回当前日期和时间  |
| YEAR(date)  | 获取指定date的年份  |
| MONTH(date)  | 获取指定date的月份  |
| DAY(date)  | 获取指定date的日期  |
| DATE_ADD(date, INTERVAL expr type)  | 返回一个日期/时间值加上一个时间间隔expr后的时间值  |
| DATEDIFF(date1, date2)  | 返回起始时间date1和结束时间date2之间的天数  |
### 流程函数

常用函数：

| 函数  | 功能  |
| ------------ | ------------ |
| IF(value, t, f)  | 如果value为true，则返回t，否则返回f  |
| IFNULL(value1, value2)  | 如果value1不为空，返回value1，否则返回value2  |
| CASE WHEN [ val1 ] THEN [ res1 ] ... ELSE [ default ] END  | 如果val1为true，返回res1，... 否则返回default默认值  |
| CASE [ expr ] WHEN [ val1 ] THEN [ res1 ] ... ELSE [ default ] END  | 如果expr的值等于val1，返回res1，... 否则返回default默认值  |

## 约束

分类：

| 约束  | 描述  | 关键字  |
| ------------ | ------------ | ------------ |
| 非空约束  | 限制该字段的数据不能为null  | NOT NULL  |
| 唯一约束  | 保证该字段的所有数据都是唯一、不重复的  | UNIQUE  |
| 主键约束  | 主键是一行数据的唯一标识，要求非空且唯一  | PRIMARY KEY  |
| 默认约束  | 保存数据时，如果未指定该字段的值，则采用默认值  | DEFAULT  |
| 检查约束（8.0.1版本后）  | 保证字段值满足某一个条件  | CHECK  |
| 外键约束  | 用来让两张图的数据之间建立连接，保证数据的一致性和完整性  | FOREIGN KEY  |

约束是作用于表中字段上的，可以再创建表/修改表的时候添加约束。

### 常用约束

| 约束条件  | 关键字  |
| ------------ | ------------ |
| 主键  | PRIMARY KEY  |
| 自动增长  | AUTO_INCREMENT  |
| 不为空  | NOT NULL  |
| 唯一  | UNIQUE  |
| 逻辑条件  | CHECK  |
| 默认值  | DEFAULT  |
### 外键约束
#### 添加外键：

```mysql
CREATE TABLE 表名(
	字段名 字段类型,
	...
	[CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
);
```
```mysql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);
```

#### 删除外键：
```mysql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名;
```
#### 删除/更新行为

| 行为  | 说明  |
| ------------ | ------------ |
| NO ACTION  | 当在父（主）表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与RESTRICT一致） |
| RESTRICT  | 当在父（主）表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与NO ACTION一致） |
| CASCADE  | 当在父（主）表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子（从）表中的记录 |
| SET NULL  | 当在父（主）表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子（从）表中该外键值为null（要求该外键允许为null） |
| SET DEFAULT  | 父（主）表有变更时，子（从）表将外键设为一个默认值（Innodb不支持） |

更改删除/更新行为：
```mysql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) ON UPDATE 行为 ON DELETE 行为;
```


## 多表查询

### 多表关系

- 一对多（多对一）
- 多对多
- 一对一
#### 一对多

案例：部门与员工
关系：一个部门对应多个员工，一个员工对应一个部门
实现：在多的一方建立外键，指向一的一方的主键

#### 多对多

案例：学生与课程
关系：一个学生可以选多门课程，一门课程也可以供多个学生选修
实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

#### 一对一

案例：用户与用户详情
关系：一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率
实现：在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的（UNIQUE）
### 查询

合并查询（笛卡尔积，会展示所有组合结果）：
```mysql
select * from employee, dept;
```

> 笛卡尔积：两个集合A集合和B集合的所有组合情况（在多表查询时，需要消除无效的笛卡尔积）

消除无效笛卡尔积：
```mysql
select * from employee, dept where employee.dept = dept.id;
```



### 内连接查询

内连接查询的是两张表交集的部分

#### 语法:

隐式内连接

```mysql
SELECT 字段列表 FROM 表一,表二 WHERE 条件···;
```

显式内连接

```mysql
SELECT 字段列表 FROM 表一 [inner] JION 表2 ON 连接条件···;
```

显式性能比隐式高

例子：

```mysql
-- 查询员工姓名，及关联的部门的名称
-- 隐式
select e.name, d.name from employee as e, dept as d where e.dept = d.id;
-- 显式
select e.name, d.name from employee as e inner join dept as d on e.dept = d.id;
```



### 外连接查询

左外连接：
查询左表所有数据，以及两张表交集部分数据
```mysql
SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ...;
```
相当于查询表1的所有数据，包含表1和表2交集部分数据

右外连接：
查询右表所有数据，以及两张表交集部分数据
```mysql
SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ...;
```

左外连接和右外连接可以相互转化

例子：

```mysql
-- 左
select e.*, d.name from employee as e left outer join dept as d on e.dept = d.id;
select d.name, e.* from dept d left outer join emp e on e.dept = d.id; 
-- 这条语句与下面的语句效果一样
-- 右
select d.name, e.* from employee as e right outer join dept as d on e.dept = d.id;
```

左连接可以查询到没有dept的employee，右连接可以查询到没有employee的dept

### 自连接查询

当前表与自身的连接查询，自连接必须使用表别名

语法(内连接)：
```mysql
SELECT 字段列表 FROM 表A AS 别名A , 表A AS 别名B WHERE 条件 ...;
```
语法(外连接)：
```mysql
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ...;
```


自连接查询，可以是内连接查询，也可以是外连接查询

例子：

```mysql
-- 查询员工及其所属领导的名字
select a.name, b.name from employee a, employee b where a.manager = b.id;
-- 没有领导的也查询出来
select a.name, b.name from employee a left join employee b on a.manager = b.id;
```



### 联合查询 union, union all

把多次查询的结果合并，形成一个新的查询集

语法：

```mysql
SELECT 字段列表 FROM 表A ...
UNION [ALL]
SELECT 字段列表 FROM 表B ...
```

#### 注意事项

- UNION ALL 会有重复结果，UNION 不会
- 联合查询比使用or效率高，不会使索引失效



### 子查询

SQL语句中嵌套SELECT语句，称谓嵌套查询，又称子查询。
```mysql
SELECT * FROM t1 WHERE column1 = ( SELECT column1 FROM t2);
```
**子查询外部的语句可以是 INSERT / UPDATE / DELETE / SELECT 的任何一个**

根据子查询结果可以分为：

- 标量子查询（子查询结果为单个值）
- 列子查询（子查询结果为一列）
- 行子查询（子查询结果为一行）
- 表子查询（子查询结果为多行多列）

根据子查询位置可分为：

- WHERE 之后
- FROM 之后
- SELECT 之后



#### 标量子查询

子查询返回的结果是单个值（数字、字符串、日期等）。

例子：

```mysql
-- 查询销售部所有员工
select id from dept where name = '销售部';
-- 根据销售部部门ID，查询员工信息
select * from employee where dept = 4;
-- 合并（子查询）
select * from employee where dept = (select id from dept where name = '销售部');

-- 查询xxx入职之后的员工信息
select * from employee where entrydate > (select entrydate from employee where name = 'xxx');
```



#### 列子查询

返回的结果是一列（可以是多行）。

常用操作符：

| 操作符  | 描述  |
| ------------ | ------------ |
| IN  | 在指定的集合范围内，多选一  |
| NOT IN  | 不在指定的集合范围内  |
| ANY  | 子查询返回列表中，有任意一个满足即可  |
| SOME  | 与ANY等同，使用SOME的地方都可以使用ANY  |
| ALL  | 子查询返回列表的所有值都必须满足  |

例子：

```mysql
-- 查询销售部和市场部的所有员工信息
select * from employee where dept in (select id from dept where name = '销售部' or name = '市场部');
-- 查询比财务部所有人工资都高的员工信息
select * from employee where salary > all(select salary from employee where dept = (select id from dept where name = '财务部'));
-- 查询比研发部任意一人工资高的员工信息
select * from employee where salary > any (select salary from employee where dept = (select id from dept where name = '研发部'));
```



#### 行子查询

返回的结果是一行（可以是多列）。
常用操作符：=, <, >, IN, NOT IN

例子：

```mysql
-- 查询与xxx的薪资及直属领导相同的员工信息
select * from employee where (salary, manager) = (12500, 1);
select * from employee where (salary, manager) = (select salary, manager from employee where name = 'xxx');
```



#### 表子查询

返回的结果是多行多列
常用操作符：IN

例子：

```mysql
-- 查询与xxx1，xxx2的职位和薪资相同的员工
select * from employee where (job, salary) in (select job, salary from employee where name = 'xxx1' or name = 'xxx2');
-- 查询入职日期是2006-01-01之后的员工，及其部门信息
select e.*, d.* from (select * from employee where entrydate > '2006-01-01') as e left join dept as d on e.dept = d.id;
```