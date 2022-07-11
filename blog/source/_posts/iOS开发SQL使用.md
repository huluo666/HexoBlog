---
title: iOS开发SQL使用
tags: iOS
categories: iOS
date: 2017-04-13 15:09:31
grammar_cjkRuby: true
---

在ios项目中使用sqlite需要添加 `libsqlite3.dylib`库

**SQL语句的特点**

> 不区分大小写（比如数据库认为insert和InSerT是一样的）
> 每条语句都必须以分号 “ ; ” 结尾。



**SQL中常用关键字**

> select、insert、update、delete、from、create、where、desc、order、by、group、table、alter、view、index等
>
> 注意:数据库中不可以使用关键字来命名表、字段
>
> SQL 语句中除过 `SELECT` 语句都可以称之为更新操作。包括 `CREATE`，`UPDATE`，`INSERT`，`ALTER`，`COMMIT`，`BEGIN`，`DETACH`，`DROP`，`END`，`EXPLAIN`，`VACUUM`，`REPLACE` 等。一般只要不是以 `SELECT` 开头的 SQL 语句，都是更新语句。



**二、SQL语句的种类**

> 1.**数据定义语句**（DDL：Data Definition Language）
>
> 包括create和drop等操作
>
> 在数据库中创建新表或删除表（create table或 drop table）
>
>
>
> 2**.数据操作语句**（DML：Data Manipulation Language）
>
> 包括insert、update、delete等操作
>
> 上面的3种操作分别用于添加、修改、删除表中的数据
>
>
>
> 3**.数据查询语句**（DQL：Data Query Language）
>
> 可以用于查询获得表中的数据
>
> 关键字select是DQL（也是所有SQL）用得最多的操作
>
> 其他DQL常用的关键字有where，order by，group by和having



### 一、创建（删除）表

#### 1、创建

> - create table 表名 (字段名1 字段类型1, 字段名2 字段类型2, …) ;
>
>
> - create table if not exists 表名 (字段名1 字段类型1, 字段名2 字段类型2, …) ;

示例:

```
create table t_student (id integer, name text, age inetger, score real)
```

```objectivec
CREATE TABLE IF NOT EXISTS     -- 创建表
t_student (					   -- 表的名称t_student
-- id主键 默认地AUTO_INCREMENT 的开始值是 1，每条新记录递增 1。
id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
name TEXT,					   -- 字段类型
age INTEGER,
score REAL
);
```

- 创建新表(create table)    删除表(drop table)


- 注意事项：

  - 1、创建表格时, 最好加个表格是否已经存在的判断, 这个防止语句多次执行时发生错误。

    `if not exists`: 判断表不存在时才创建表.。

  - 2、SQL不区分大小写,编写SQL规范,最好关键字都使用大写.表名最好加上`t_`前缀。

  - 3、`PRIMARY KEY`: 约束为主键。
  - 4、`AUTOINCREMENT`: 自动递增。

创建表推荐写法：

```
CREATE TABLE IF NOT EXISTS t_student (id integer PRIMARY KEY AUTOINCREMENT, name text, age integer, score integer);
```

#### 2、删除表

> 格式: `DROP TABLE IF EXISTS 表名;`

删除表示例

```
DROP TABLE IF EXISTS t_student;
```



#### 3、修改表(重命名表)

> 格式: `ALTER TABLE 旧表名 RENAME TO 新表名;`

重命名表示例

```
ALTER TABLE t_student RENAME TO t_person;
```



#### 4、数据表添加字段

> 格式: `ALTER TABLE 表名 ADD COLUMN 字段名 数据类型 限定符`

数据表添加字段示例

```
ALTER TABLE t_student ADD COLUMN address text;
```



SQLite数据类型类型

> **SQLite将数据划分为以下几种存储类型：**
> integer : 整型值
> real : 浮点值
> text : 文本字符串
> blob : 二进制数据（比如文件）

实际上SQLite是无类型的
就算声明为integer类型，还是能存储字符串文本（主键除外）
建表时声明啥类型或者不声明类型都可以，也就意味着创表语句可以这么写：
create table t_student(name, age); 为了保持良好的编程规范，编写建表语句的时候最好加上每个字段的具体类型



### 二、DML数据库操作操作(增删改)

### 1、增

插入数据（insert）

> 格式： insert into 表名 (字段1, 字段2, …) values (字段1的值, 字段2的值, …) ;

SQL插入数据示例

```
insert into t_student (name, age) values (‘mj’, 10) ;
INSERT INTO t_student (name, age, height) VALUES ('zhangsan', 20, 1.78);
```

- 注意：数据库中的字符串内容应该用单引号 ’ 括住

- 1.字段和值一定要对应，否则也会添加错误的数据进表里

   2.TEXT类型需要添加引号''



### 2、删

> 格式: delete from 表名 ;
>
> 格式: delete from 表名 where 条件;

删除表示例

```
delete from t_student ;
/*
	DML删除数据
	DELETE		-- 删除数据
	FROM		-- 从哪张表中删除数据
	WHERE		-- 条件
 */
DELETE FROM t_student WHERE id = 1;
```



### 3、改

更新数据（update）

> 格式: update 表名 set 字段1 = 字段1的值, 字段2 = 字段2的值, … ;

SQL更新数据示例

```
/*
	DML更新数据
	UPDATE			-- 更新数据
	t_student		-- 需要更新数据的表
	SET			-- 更新哪个字段
	name = 'liudehua'	-- 更新字段的值
	WHERE			-- 条件

注意：
 1.如果不指定条件会更新所有数据
 */
update t_student set name = ‘jack’, age = 20 ;
更新一个字段
update t_person set name = 'xiaofang' where name = 'wangwu';
更新多个字段，每个字段之间使用,分隔
update t_person set age = 20, height = 2.0 where name = 'xiaofang';
```



### 三、DQL数据库查询语句(查询)

查询数据（select）

- 1.查询数据格式

> 格式1: `select * from 表名;`, `*`:通配符,表示所有字段.
> 格式2: `select 字段1, 字段2, … from 表名;`
> 格式3: `select 字段1, 字段2, … from 表名 where 条件;`

```
/*
	DQL查询数据
	SELECT		-- 查询
	name, age	-- 查询的字段
	FROM		-- 从哪张表中查询
    t_student   -- 查询的表名
 */
-- 查询所有字段
SELECT * FROM t_student;
-- 查询指定字段
SELECT name, age FROM t_student;
-- 查询 age < 22 的记录的name, age字段
SELECT name, age FROM t_student WHERE age < 22;

-- 查询 age < 22 的所有字段
SELECT * FROM t_student WHERE age < 22;

-- 查询记录总数
SELECT COUNT(*) FROM t_student;

-- 查询 age < 22 的记录总数
SELECT COUNT(*) FROM t_student WHERE age < 22;

-- 查询最大的 age
SELECT MAX(age) FROM t_student;

-- 查询最小的 age
SELECT MIN(age) FROM t_student;

-- 查询所有记录的所有字段，根据 age 升序排序
SELECT * FROM t_student ORDER BY age;

-- 查询所有记录的所有字段， 根据 age 降序排序
SELECT * FROM t_student ORDER BY age DESC;

-- 使用多个字段排序，先按age降序排序，当age相同再根据height降序排序
SELECT * FROM t_student ORDER BY age DESC,height DESC;

-- 返回指定的记录
-- LIMIT常用于分页
-- 0 表示跳过的3条
-- 2 表示获取2条
SELECT * FROM t_student LIMIT 3, 2;

-- 取出年龄最大的3条记录
SELECT * FROM t_student ORDER BY age DESC LIMIT 3;

-- 查询是给字段取别名
SELECT name AS JP_name, age AS JF_age, height AS JF_height FROM t_student;


从数据库查出名字叫做wangwu的记录
select * from t_student where name = 'wangwu';
从数据库查出名字以wang开头的记录
select * from t_student where name like 'wangwu%';
从数据库查出名字中包含a的记录，通常用于模糊查询，建议不要搞太多字段组合模糊查询，那样性能会非常差！
select * from t_student where name like '%a%';
```

排序查询

> 1、ASC 升序（默认的排序方法） 2、DESC 降序
>
> 格式: `SELECT 字段1(或*) FROM 表名 ORDER BY 要排序的字段名 ASC;`
>
> 格式: `SELECT 字段1(或*) FROM 表名 ORDER BY 要排序的字段名 DESC;`
>
> 格式: `SELECT * FROM 表名 ORDER BY 字段名1 排序类型, 字段名2 排序类型;`



```
●  select * from t_student order by age desc ; //降序
●  select * from t_student order by age asc ; // 升序(默认)
用多个字段进行排序,由左至右排序的优先级依次降低，也就是第一个排序列的优先级是最高的
●  select * from t_student order by age asc, height desc ;
先按照年龄排序(升序),年龄相等就按照身高排序(降序)
```



limit分页查询

> 格式: `select * from 表名 limit 数值1, 数值2 ;`

●  使用limit可以精确地控制查询结果的数量,比如每次只查询10条数据

```
select * from t_student limit 4, 8 ;// 可以理解为:跳过最前面4条语句,然后取8条记录
SELECT * FROM t_student LIMIT 5*(n-1), 5;//一页显示5条数据,查询第n页的数据
```



对数据进行统计

```
取出所有数据的总数目
select count(*) from t_student;
统计符合条件的记录条数
select count(*) from t_student where name like 'wang%';
选择指定列的最大值
select max(age) from t_student;
选择指定列的最小值
select min(age) from t_student;
计算指定列的平均值
select avg(age) from t_student;
计算指定列数值的总数
select sum(age) from t_student;
```





**四、约束**

1、简单约束

> 建表时可以给特定的字段设置一些约束条件，常见的约束有
>
> `not null` ：规定字段的值不能为null
>
> `unique` ：规定字段的值必须唯一
>
> `default` ：指定字段的默认值
>
> （建议：尽量给字段设定严格的约束，以保证数据的规范性）
>
> 示例
>

```
create table t_student (id integer, name text not null unique, age integer not null default 1) ;
//name字段不能为null，并且唯一
//age字段不能为null，并且默认为1
```

说明：如果想要让主键自动增长（必须是integer类型），应该增加autoincrement

2、主键 primary key

```
  // integer类型的id作为t_student表的主键
   create table t_student (id integer primary key,name text,age integer);
/*
    主键字段：只要声明为primary key，就说明是一个主键字段
    主键字段默认就包含了not null 和 unique 两个约束
    如果想要让主键自动增长（必须是integer类型），应该增加autoincrement
*/
    create table t_student (id integer primary key autoincrement,name text,age integer);
```



**SQL语句的基本使用**

### 条件语句

- 1.条件语句的常见格式

> where 字段 = 某个值 ; // 不能用两个 =
> where 字段 is 某个值 ; // is 相当于 =
> where 字段 != 某个值 ;
> where 字段 is not 某个值 ; // is not 相当于 !=
> where 字段 > 某个值 ;
> where 字段1 = 某个值 and 字段2 > 某个值 ; // and相当于C语言中的 &&
> where 字段1 = 某个值 or 字段2 = 某个值 ; // or 相当于C语言中的 ||



iOS 更新

```
char *update = "INSERT OR REPLACE INTO PERSIONINFO(NAME,AGE,SEX,WEIGHT,ADDRESS)""VALUES(?,?,?,?,?);";
//上边的update也可以这样写：
//NSString *insert = [NSString stringWithFormat:@"INSERT OR REPLACE INTO PERSIONINFO('%@','%@','%@','%@','%@')VALUES(?,?,?,?,?)",NAME,AGE,SEX,WEIGHT,ADDRESS];
```



#### FMDB的使用

Github：https://github.com/ccgus/fmdb

FMDB是iOS平台的SQLite数据库框架，以OC的方式封装了SQLite的C语言API。

优点：

使用起来更加面向对象，省去了很多麻烦、冗余的C语言代码

提供了多线程安全的数据库操作方法，有效地防止数据混乱





相关文章

[runoob SQLite 教程](http://www.runoob.com/sqlite/sqlite-create-table.html)

[w3school SQL 教程](http://www.w3school.com.cn/sql/index.asp)

