 ``数据库：数据存储的仓库

数据库管理系统：操纵和管理数据库的大型软件

## 基础

### SQL的分类：

​			操作关系型数据库的编程语言，是一套标准。

```sql
通用语法：
1.SQL语句可以单行或多行，以分号结尾；
2.MYSQL数据库的SQL语句不区分大小写，关键字一般用大写
3.单行注释： --注释内容   或 #注释内容
  多行注释 /**/	
```

关系型数据库的特点：

```
1.数据结构化：数据以表格形式存储，易于理解和操作
2.数据操作：使用结构化查询语言SQL执行复杂的查询恶化数据操作
3.数据完整性：数据库可以定义数据的完整性规则，确保数据的准确性和一致性。
4.事务支持：关系型数据库支持事务处理，确保数据操作的原子性、一致性、隔离性和持久化(ACID属性)。
```

#### DDL：数据定义语言

```SQL
主要用来操作数据库、数据库表、字段的定义
1.DDL-数据库操作
查询
	查询所有数据库 SHOW DATABASES;
	查询当前数据库 SELECT DATABASE();
创建
	CREATE DATABASE 数据库名;
	CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLLATE 排序规则];
删除
	DROP DATABASE [IF EXISTS] 数据库名;
使用
 	USE 数据库名;
 
2.1DDL-表操作-查询
 查询当前数据库所有的表结构（前提是先使用该数据库）
 	SHOW TABLES;
 查询表结构
 	DESC 表名；   //describe 描述
 查询指定表的建表语句
 	SHOW CREATE TABLE 表名；
2.2DDL-表操作-创建
CREATE TABLE 表名（
	字段1 字段1类型 	[COMMENT 字段1注释]，
	字段2 字段2类型 	[COMMENT 字段2注释]，
	字段3 字段3类型 	[COMMENT 字段3注释]，
	...
	字段n 字段n类型 	[COMMENT 字段n注释]
	）[COMMENT 表注释]；
DDL-表操纵-数据类型
	mySQL的数据类型比较多，主要分为三类：数值类型、字符串类型、日期时间类型。
2.3DDL-表操作-修改
	添加字段
    ALTER TABLE 表名 ADD 字段名 类型（长度） [COMMENT 注释] [约束]；
    
    修改数据类型
    ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）；
	修改字段名和字段类型
    ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度）[COMMENT 注释] [约束]；
    
    删除字段
    ALTER TABLE 表名 DROP 字段名；
    
    修改表名
    ALTER TABLE 表名 RENAME TO 新表名；
2.4DDL-表操作-删除
	删除表
	DROP TABLE [IF EXISTS] 表名；
	删除指定表，并重新创建该表
	TRUNCATE TABLE 表名；
```

#### DML:数据操作语言 

```sql
主要对表中的记录进行增、删、改操作
添加数据(INSERT)
修改数据(UPDATE)
删除数据(DELETE)

1.DML-添加数据
1.1给指定字段添加数据
INSERT INTO 表名(字段1，字段2，...) VALUES(值1，值2，...);
1.2给全部字段添加数据
INSERT INTO 表名 VALUES (值1，值2，...);
1.3批量添加数据
INSERT INTO 表名 (字段1，字段2，...) VALUES (值1，值2，...),(值1，值2，...),(值1，值2,...);
INSERT INTO 表名 VALUE (值1，值2，...),(值1，值2，...),(值1，值2,...);
#	插入数据时，指定的字段顺序需要与值的顺序是一一对应的。
#	字符串和日期型数据应该包含在引号中。
#	插入的数据大小，应该在字段规定的范围内。

2.DML-修改数据
 UPDATE 表名 SET 字段1=值1，字段2=值2，... [WHERE 条件]；
#	修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的全部数据。

3.DML- 删除数据
DELETE FROM 表名 [WHERE 条件]
#	delete 语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。
#	delete 语句不能删除某一个字段的值（可以使用UPDATE）.
```

#### DQL：数据查询语言

```SQL
用来查询数据库中的表的记录
查询关键字 SELECT
·基本查询
·条件查询(WHERE)
·聚合函数(count,max,min,avg,sum)
·分组查询(GROUP BY)
·排序查询(ORDER BY)
·分页查询 (LIMIT)
```

```SQL
#编写顺序             	  执行顺序
SELECT                            
		字段列表			 4
FROM
		表名列表			 1
WHERE
		条件列表（分组前）	  2
GROUP BY 
		分组字段列表	   		3
[HAVING
		分组后条件列表]
ORDER BY
		排序字段列表	        5
LIMIT
		分页参数		      6
		
where和having的区别：
1.执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤。
2.判断条件不同：where不能对聚合函数进行判断，而having可以。
#	执行顺序 ：where > 聚合函数 > having
#   分组之后，查询字段一般为聚合函数和分组字段，查询其他字段无意义。
```

![image-20240524232137411](images/image-20240524232137411.png)

```SQL
#执行顺序
FROM
WHERE
GROUP BY
HAVING
SELECT
ORDER BY
LIMIT
```

#### 条件查询

![image-20240523220657979](images/image-20240523220657979.png)

```SQL
1.DQL-基本查询
	1.1查询多个字段
	SELECT 字段1，字段2，字段3，... FROM 表名；
	SELECT * FROM 表名；# 查询返回所有字段
	1.2设置别名
	SELECT 字段1 [AS 别名1]，字段2 [AS 别名2]，字段3 [AS 别名3]，... FROM 表名；# AS 可以省略
	1.3去除重复记录
	SELECT DISTINCT 字段列表 FROM 表名；   # distinct 不同的 
	
2.DQL-条件查询
	语法：
		SELECT 字段列表 FROM 表名 WHERE 条件列表；
3.DQL-分组查询
	#DQL-聚合函数 将一列数据作为一个整体，进行纵向计算(作用域某一列)。
	常见的聚合函数
		函数						功能
		count					统计数量
		max						最大值
		min 					最小值
		avg 					平均值
		sum						求和
	语法：
		SELECT 聚合函数(字段列表) FROM 表名
#	null值不参与所有的聚合运算
	分组查询-语法：
	SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件]；
where和having的区别：
1.执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤。
2.判断条件不同：where不能对聚合函数进行判断，而having可以。
#	执行顺序 ：where > 聚合函数 > having
#   分组之后，查询字段一般为聚合函数和分组字段，查询其他字段无意义。
4.DQL-排序查询    
	语法：支持多字段排序，从左往右
	SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1，字段2，排序方式2；
	排序方式
	1.ASC：升序（默认值），可以不指定   --- ascend 提升
	2.DESC：降序		   			 --- descend下降
#如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序。
5.DQL-分页查询
	语法：
	SELECT 字段列表 FROM 表名 LIMIT 起始索引，查询记录数；
# 起始索引从0开始，起始索引=（查询页码-1）*每页显示记录数。
# 分页查询是数据库的方言，不同数据库有不同的实现，MySQL中是LIMIT。
# 如果查询的是第一页数据，起始索引可以省略，直接简写为 limit 10
```

#### DCL: 数据控制语言

```sql
用来管理数据库用户，控制数据库的访问权限。
1.DCL- 用户管理
	1.1 查询用户
		USE mysql;
		SELECT * FROM user [\G];   #\G  列输出
	1.2 创建用户
		CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码'; #主机名为 % 时，通配符，便是任意主机
	1.3 修改密码
		ALTER USER '用户名'@'主机名' IDENTIFIED WITH  mysql_native_password BY '新密码';
	1.4 删除用户
		DROP USER '用户名'@'主机名';
# 主机名可以用%通配；
# 主要用于DBA(database administrator 数据管理员) 使用。

2.DCL- 权限控制
	2.1 查询权限
		SHOW GRANTS FOR '用户名'@'主机名'
	2.2 授予权限
		GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';#指定数据库，指定表授予权限
		GRANT 权限列表 ON *.* TO '用户名'@'主机名';#授予所有数据库，所有表权限
	2.3 撤销权限
		REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
#	多个权限之间使用逗号分隔
#	授权时，使用通配符* 代替表名和数据库名，代表所有。
```

![image-20231204163821962](images/image-20231204163821962.png)

### 函数

#### 	字符串函数（可以对查询的字段列表进行函数操作）

```sql
CONCAT(S1,S2,...Sn)		字符串拼接
LOWER(str)				全部转为小写
UPPER(str)				全部转为大写
LPAD(str,n,pad)			左填充，用字符串pad对str的左边进行填充，使str达到n个字符长度
RPAD(str,n,pad)			右填充，用字符串pad对str的右边进行填充，使str达到n个字符长度
TRIM(str)				去掉字符串头部和尾部的空格
SUBSTRING(str,start,len)返回从字符串str的start位置开始的len个长度的字符串
#字符串第一个索引为1，即字符串的下标从1开始。   //可以不指定截取长度，则默认从截取索引开始到最后
```

#### 	数值函数

```sql
CEIL(x)					向上取整
FLOOR(x)				向下取整
MOD(x,y)				返回x/y的模
RAND()					返回0~1内的随机数
ROUND(x,y)				求参数x的四舍五入的值，保留y位小数
```

#### 	日期函数

```sql
CURDATE()							返回当前日期
CURTIME()							返回当前时间
NOW()								返回当前日期和时间
YEAR(date)							获得指定日期date的年份
MONTH(date)							获得指定日期date的月份
DAY(date)							获得指定日期date的日
DATE_ADD(date,INTERVAL expr type)	返回一个日期/时间值加上一个时间间隔expr后的时间值，type时间单位    
DATEDIFF(date1,date2) 				返回起始时间date1,结束时间date2之间的天数  即第一个时间-第二个时间的天数.
#查询所有员工的入职天数，并根据入职天数进行倒序排序
select name, datediff(curdate(),entrydate) as 'entrydays' from emp order by entrydays desc;
```

#### 	流程函数

``` sql
流程控制函数 可以在SQL语句中实现条件筛选，从而提高语句的效率
IF(value,t,f)								
	如果value为true,则返回t,否则返回f
IFNULL(value1,value2)						
	如果value不为空(null)，返回value1，否则返回value2
CASE WHEN [val1] THEN [res1]... ELSE [default] END	
	如果val1为true,返回res1,...否则返回default
CASE [expr] WHEN [val1] THEN [res1] ...ELSE [default] END 
	如果expr的值等于val1，返回res1,...否则返回default默认值。
```

### 约束

​		1.约束是作用于表中字段上的规则，用于限制存储在表中的数据。

​		2.目的：保证数据库中数据的正确，有效性和完整性。

​		3.分类：

 ![image-20240525153728024](images/image-20240525153728024.png)

```SQL
约束									描述								关键字
非空约束			限制该字段数据不能为空									NOT	NULL
唯一约束			保证该字段的所有数据都是唯一、不重复的					   UNIQUE
主键约束			主键是一行数据的唯一标识，要求非空且唯一				  PRIMARY KEY
默认约束			保存数据时，如果未指定该字段的值，则采用默认值				DEFAULT
检查约束			保证字段值满足某一个条件							   CHECK

外键约束			用来让两张表的数据之间建立连接，保证数据的一致性和完整性	FOREIGN KEY
#	约束是作用域表中的字段上的，可以在创建表/修改表的时候添加约束。
```

```sql
外键约束   建立两张表数据之间的连接
	语法：
添加外键约束
	1.创建表时添加外键：
	CREATE TABLE 表名 (
     	字段1 数据类型 
        ...
        [CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERENCES 主表(主表列表)
    );
    2.对已创建的表添加外键
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(主表列表);
删除外键
	ALTER TABLE 表名 DROP FOREIGN KEY 外键名称
#添加约束条件 针对某个字段  
#删除约束条件 

#在mysql中，设置外键时，它必须关联到另一张表的主键或唯一约束的字段。即保证关联的字段中的每个值都是唯一的。
语法：
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段) REFERENCES 主表名(主表字段名) 
	ON UPDATE CASCADE ON DELETE CASCADE;	#指明在更新时怎么操作和删除时怎么操作。
```

![image-20240525165103730](images/image-20240525165103730.png)

### 多表查询

```
多表关系
多表查询概述
内连接
外连接
自连接
子查询
多表查询案例
```

#### 多表关系

```
项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结构，由于业务之间相互关联，所以各个表结构之间也存在着各种联系，基本上分为三种：
·一对多（多对一）
·多对多
·一对一
```

##### 一对多（多对一） 

```
案例：部门与员工的关系
关系：一个部门对应多个员工，一个员工对应一个部门
实现：在多的一方建立外键，指向一的一方的主键
```

##### 多对多的关系

```
案例：学生与课程的关系
关系：一个学生可以选修多门课程，一个课程也可以供多个学生选择
实现：建立第三张中间表，中间表至少包含两个主键，分别关联两方主键
```

##### 一对一

```sql
案例：用户与用户详情的关系
关系：一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率
实现：在任意一方加入外键，关联另外一方的主键，并设置外键为唯一的（UNIQUE）
```

#### 多表查询

```sql
概述：从多张表中查询数据
笛卡尔积：笛卡尔乘积是指在数学中，两个集合A集合和B集合的所有组合情况。（在多表查询时，需要消除无效的笛卡尔积）
# 通过外键条件实现 ，比如子表的字段关联的父表 的字段

多表查询分类：	
	1.连接查询
		1.1内连接：相当于查询A、B交集部分数据
		1.2外连接
			左外连接：查询左表所有数据，以及两张表的交集部分数据；
			有外连接：查询右表所有数据，以及两张表的交集部分数据；
		1.3自连接：当前表与自身的连接查询，自连接必须使用表别名
	2.子查询
```

##### 连接查询-内连接

```sql
语法：					相当于查询A、B交集部分数据
隐式内连接：
	SELECT 字段列表 FROM 表1，表2 WHERE 条件...;
显示内连接：
	SELECT 字段列表 FROM 表1	[INNER] JOIN 表2 ON 连接条件...;
```

##### 连接查询-外连接

```SQL
左外连接：查询左表所有数据，以及两张表的交集部分数据；
	SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件...;
右外连接：查询右表所有数据，以及两张表的交集部分数据；
	SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件...;
#左外连接和右外连接可以通过语法中表的定义表的顺序来实现互换功能：即左外连接 表1，表2 可以通过右外连接 表2,表1来实现。
```

##### 连接查询-自连接

```SQL
#自连接查询，可以是内连接查询，也可以是外连接查询。即多次从相同的表中查询
SELECT 字段列表 FROM 表A 别名A 表A 表B WHERE...;
SELECT 字段列表 FROM 表A 别名A JOIN 表A 表B ON 条件...;
SELECT 字段列表 FROM 表A 别名A LEFT [OUTER] JOIN 表A 别名B ON 条件...;
SELECT 字段列表 FROM 表A 别名A RIGHT [OUTER] JOIN 表A 别名B ON 条件...;
#自连接查询中，表必须起别名
```

#### 子查询（嵌套查询）

```SQL
SQL语句中嵌套SELECT语句，称为嵌套查询，也称为子查询。
	SELECT * FROM t1 WHERE colunm1=(SELECT column1 FROM t2);
# 	子查询的外部语句可以是INSERT/UPDATE/DELECT/SELECT的任何一个。

根据子查询的结果不同，分为：（内部的查询为子查询）
	·标量子查询（子查询结果为单个值）
	·列子查询（子查询结果为一列）
	·行子查询（子查询结果为一行）
	·表子查询（子查询结果为多行多列）
根据子查询位置，分为 WHERE之后，FROM之后，SELECT之后。
#子查询的返回结果作为外部查询的判断条件。
```

##### 标量子查询

```sql
子查询的结果是单个值（数字、字符串、日期等），最简单的形式，称为标量子查询。
常用操作符：= <> > >= < <=   其中 <> 代表不等于
#子查询的结果是单行单列的值（单个值），可以用作外部查询的判断条件。
```

##### 列子查询

```SQL
列子查询的返回结果是一列（可以是多行）。
常规的操作符 IN、  NOT IN、  ANY、  SOME、  ALL
	操作符								描述
	IN					 	在指定的集合范围之内，多选一
	NOT IN 					不在指定的集合范围之内
	ANY						子查询的返回列表中，有任意一个满足即可
	SOME					与ANY等同，使用SOME的地方都可以使用ANY
	ALL						子查询返回列表的所有值都要满足
```

##### 行子查询

```sql
行子查询的返回结果是一行。
常用的操作符：=、 <>、 IN、 NOT IN 
```

##### 表子查询

```sql
子查询返回结果是一张表（多行多列）。
常用操作符： IN
```

#### 联合查询-union,union all

```SQL
#对于union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集。
	SELECT 字段列表 FROM 表A ...
	UNION [ALL]		#	UNION ALL直接将查询的结果合并，UNION将查询的结果合并并去重。 
	SELECT 字段列表 FROM 表B ...;	
#条件：联合查询的多张表的列数必须保持一致，字段类型也需要保持一致。
```

### 事务

```sql
事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。
#	默认mySQL的事务是自动提交的，也就是说，当你执行一条DML语句，mySQL会立即隐式的提交事务。

进行事务操作的步骤：1.先开启事务，2.执行语句，3.提交事务
```

#### 事务操作

```sql
两种事务操作方式：隐式事务 、显示事务
#隐式事务是由数据库管理系统自动处理事务边界，开发人员不需要显示的指定事务的开始和结束。@@autocommit=1 时每个独立的SQL被执行时，数据库会自动将其视为一个单独任务。而想要手动处理事务，需要将参数设置为手动提交
1.查看/设置事务提交方式
 SELECT @@autocommit;
 SET @@autocommit=0;    #设置为手动提交事务
2.提交事务
COMMIT;    #确认提交后，就没办法在进行回滚
3.回滚事务
ROLLBACK;  #如果事务提交过程中出现异常，不要提交，可以通过回滚来返回到之前的状态

#显示事务。不用设置参数 需要先开启事务，然后执行操作，如果所有的操作都正常进行，那么提交事务，否则回滚事务。
1.开启事务
START TRANSACTION 或 BEGIN;
2.提交事务
COMMIT;
3.回滚事务
ROLLBACK;
```

#### 事务的四大特性：ACID（原子性、一致性、隔离性、持久性）

```
原子性（atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败.
一致性（consistency）:事务完成后，数据库必须从一个一致状态变为另一个一致状态。
隔离性（isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境中运行，即事务的中间状态对其他事务是不可见的，即多个事务并发执行，它们之间的操作也不会干扰。
持久性（durability）：事务一旦提交或回滚，它对数据库中的数据的改变是永久性的。
```

#### 并发事务问题

```bash
问题								描述
脏读	#一个数据读到另一个事务还没有提交的数据
不可重复读  #一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读
幻读	#一个事务按条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在，好像出现‘幻影’
```

#### 事务隔离级别（解决并发事务问题）

![image-20240527221558911](images/image-20240527221558911.png)

![image-20240527221349130](images/image-20240527221349130.png)

```sql
Serializable						
#代表在该隔离级别下，仍会出现的并发事务问题
mysql默认隔离级别可重复读
查看事务的隔离级别
SELECT @@TRANSACTION_ISOLATION;
设置事物的隔离级别
SET [SESSION|GLOBAL] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED |
READ COMMITTED|REPEATABLE READ|SERIALIZABLE}
#set session transaction isolation level read uncommitted ;
设置当前会话事物的隔离级别为读未提交
```

## 进阶

### 存储引擎

#### MySQL体系结构

```
分为四层：
·连接层：最上层是一些客户端和链接服务，主要完成一些类似连接处理、授权认证、及相关的安全方案。服务器也会为安全接入的每个客户端验证它所具有的操作权限。
·服务层：第二层架构主要完成大多数的核心服务功能，如SQL接口，并完成缓存的查询，SQL的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数。
·引擎层：存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API和存储引擎进行通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需求，来选取合适的存储引擎。
·存储层：主要讲数据存储在文件系统上，并完成与存储引擎的交互。
```

#### 存储引擎简介

​	存储引擎就是存储数据、建立索引、更新/查询数据等技术的是是实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也可以被称为表类型。

```SQL
1.在创建表的时候，指定存储引擎
CREATE TABLE 表名(
	字段1  字段1类型 [comment 字段1注释]，
  	...
    字段n  字段n类型 [comment 字段n注释]
) ENGINE=INNODB [COMMENT 表注释];

2.查看当前数据库所支持的存储引擎
SHOW ENGINES;

3.通过建表语句来查看表在创建时的存储引擎
SHOW CREATE TABLE 表名；
```

#### 存储引擎特点

```sql
·innoDB（三大特性：支持事务、外键、行级锁）
	innoDB是一种兼顾高可靠性和高性能的通用存储引擎，在mysql5.5之后，innoDB成为MySQL默认存储引擎。
特点：
	·DML操作遵循ACID模型，支持事务；
	·行级锁，提高并发访问性能；
	·支持外键约束 FOREIGN KEY约束，保存数据的完整性和正确性；
文件
	xxx.ibd ：xxx代表的表名，innoDB引擎的每张表都会对应这样一个表空间文件，存储该表的表结构（frm,sdi）、数据和索引。		
	参数：innodb_file_per_table
	查看参数：show variables like 'innodb_file_per_table';
如果状态为 ON（打开），即代表每一张表都对应一个表空间文件。
ibd2sdi --dump-file member.txt member.ibd
#通过cmd查看ibd文件的配置信息并转存到文件 member.txt


·MyISAM
	是mysql早期的默认存储引擎
特点：
	·不支持事务，不支持外键
	·支持表锁，不支持行锁
	·访问速度快
文件
	xxx.sdi:存储表结构信息
	xxx.MYD:存储数据
	xxx.MYI：存储索引
	
·Memory
	memory引擎的表数据是存储在内存中的，由于受到硬件问题、或断点问题的影响，只能将这些表作为临时表或缓存使用
特点：
	·内存存放
	·hash索引（默认）
文件
	xxx.sdi:存储表结构信息
```

inooDB的逻辑存储结构图

![image-20240528210636744](images/image-20240528210636744.png)

存储引擎选择

```
在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎，对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合：
·InnoDB:是MySQL的默认存储引擎，支持事务、外键。如果应用对事务的完整性有比较搞得要求，在并发条件下要求数据的一致性，数据操作除了插入和查询外，还包含很多的更新、删除操作，InnoDB存储引擎比较合适。
·MyISAM:适用应用是以读操作和插入操作为主，只有很少的插入和删除操作，并且对事物的完整性、并发性要求不高。
·MEMORY:将所有数据保存到内存中，支持hash索引，访问速度快、通常用于临时表及缓存。MEMORY的缺陷就是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性。
```

![image-20240528211421446](images/image-20240528211421446.png)

### 索引

#### ·索引概述

```
	索引是帮助mysql高效获取数据的数据结构（有序）。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引。
#无索引时全表扫面
优缺点：
提高数据检索的效率，减低数据库的i/o成本
通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗

索引也是要占空间的，
索引大大提高了查询效率，同时却降低更新表的速度，如对表进行 insert、update、delete时，效率降低。
```

#### ·索引结构

```
MySQL的索引是在存储引擎层实现的，不同的存储引擎有不同的结构：主要包括以下几种：
索引结构										描述
B+Tree索引*	 	  最常见的索引类型，大部分引擎都支持B+数索引
Hash索引			  底层数据结构是由哈希表实现的，只有精确匹配索引列的查询才有效，不支持范围查询
R-tree(空间索引)	空间索引是mysql引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少
Full-text(全文索引) 是一种通过建立倒排索引，快速匹配文档的方式，类似于 Lucene , Solr, ES

#默认B+tree索引
```

![image-20240529200153789](images/image-20240529200153789.png)

```
hash
采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。
特点：
·Hash索引只能用于对等比较（=，in）,不支持范围查询（between,>,<...）
·无法利用索引完成排序操作
·查询效率高，通常只需要检索一次（没有哈希碰撞时）就可以，效率通常高于B+tree索引。
存储引擎支持：
在mysql中，支持hash索引的时MEMORY引擎，而InnoDB中具有自适应hash功能，hash索引是存储引擎根据B+tree索引在指定条件下自动构建的。
```

##### hash索引的B+tree索引的对比

```
为什么InnoDB存储引擎选择适用B+tree索引结构？
·相较于二叉树，层级更少，搜索效率高；
·对于Btree,无论是叶子节点还是非叶子节点都会保存数据，(页的大小是固定的)这样导致页中存储的键值减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能减低；
·相较于hash索引，B+tree支持范围匹配及排序操作；（所有的数据都会出现在叶子节点，同时所有叶子节点构成一个双向列表）
```

#### ·索引分类

![image-20240529203912207](images/image-20240529203912207.png)

```mysql
主要分类：
	主键索引：用于唯一标识每行数据，表中只能有一个主键索引，通常是自动创建的，关键字为 PRIMARY。
	唯一索引：确保某列中的值是唯一的，可以有多个唯一索引，关键字为 UNIQUE。
	常规索引：用于加速数据检索，可以有多个，没有特定的关键字。
	全文索引：用于全文搜索，查找文本中的关键词，可以有多个，关键字为 FULLTEXT。
```

##### 聚集索引和二级索引

![image-20240529204509658](images/image-20240529204509658.png)

```
主键建立聚集索引，其他的索引建立的是辅助索引（二级索引）。
在innoDB存储引擎中，根据索引的存储形式，又分为以下两种：聚集索引和非聚集索引
聚集索引的选取规则：
·如果存在主键，主键索引就是聚集索引；
·如果不存在主键，将使用第一个唯一（UNIQUE）索引作为聚集索引；
·如果没有主键，或者是没有合适的唯一索引，则innoDB会自动生成一个rowid（一种用于唯一标识数据库表中每一行数据的值）作为隐藏的聚集索引。
```

![image-20231106203040426](images/image-20231106203040426-16992738462571.png)

```cpp
因为id设定为主键，因此以id建立聚集索引结构，其要求子节点存放的时对应id的行数据
而以name字段建立的索引结构为二级索引，其叶子节点存放的是对应的主键id
//即存在主键时，
    
InnoDB主键索引的B+tree高度为多少？
```

![image-20231106203123947](images/image-20231106203123947-16992738855842.png)

```mysql
回表查询：访问的字段建立的是二级索引，通过二级索引找到主键值，再通过主键的聚集索引进行查询。
#没有索引的字段查询：
	如果查询的字段没有建立任何索引（无论是聚集还是辅助索引），数据库将不得不执行全表扫描来查找匹配的行。在全表扫描中，数据库逐行检查，看每行是否符合查询条件。这通常比使用索引要慢得多，因为它需要检查表中的每一行，而不是直接跳到那些可能符合条件的行。
```

![image-20231106203159528](images/image-20231106203159528-16992739224593.png)

#### ·索引语法

```SQL
创建索引
CREATE [UNIQUE|FULLTEXT] INDEX index_name ON table_name (index_col_name,...);#默认添加常规索引
#index_col_name 字段名
#单列索引：针对表中单个字段（1列）创建的索引。
#联合索引：针对表中多个列（多字段）创建的索引。
查看索引
SHOW INDEX FROM table_name;

删除索引
DROP INDEX index_name ON table_name;

#索引的长度取决于几个关键因素：索引的数据类型、索引包含的字段数、每个字段的长度，以及特定的存储引擎如何处理这些索引。
```

#### ·SQL性能分析

```sql
#	SQL性能分析是指对数据库中的SQL查询进行评估和优化的过程，目的是提高查询性能并减少资源消耗。
1·SQL的执行频率（查看增删改查操作在当前数据库中所占的比例）
	通过  show [session|global] status 命令查看当前服务器状态信息。通过如下指令可以查看当前数据库的 INSERT UPDATE DELETE SELECT 访问频次。
#	show [session|global] status like 'Com_______'; 7个下划线，like 模糊匹配

2·慢查询日志
	慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句日志。
	show variables like 'slow_query_log';#查询慢查询日志是否开启
	MYsql的慢查询日志默认没有开启，需要在MySQL的配置文件（/etc/[mysql/]my.cnf）中配置如下信息：
#开启mysql的慢查询日志开关
	slow_query_log=1;
#设置慢查询日志的时间为2s,SQL语句的执行时间超过两秒，记录慢查询日志
	long_query_time=2;
配置完毕后，通过以下指令重新启动Mysql服务器进行测试，查看慢查询日志中记录的信息 /var/lib/mysql/localhost-slow.log
	
3·profile详情
show profiles 能够在做SQL优化时帮助我们了解时间都耗费在哪里。通过 have_profiling参数，能够看到当前MySQL是否支持profile操作：
	select @@have_profiling;
#默认profiling是关闭的，可以通过set语句在session/global 级别开启profiling
	查看当前profiling  select @@profiling;
	设置profiling		set  profiling=1;
	
show profiles;   
#查看每一条SQL的耗时情况

show profile for query query_id;
#（query_id）通过show profiles可以查看到
#查看指定的query_id的SQL语句执行过程中各个阶段的耗时情况

show profile cpu for query query_id;
#查看指定的query_id的SQL语句CPU使用情况；

4·explain执行计划
	EXPLAIN和DESC命令获取MYSQL如何执行SELECT语句的信息，包括在SELECT语句执行过程中表如何连接和连接的顺序。
	EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件；
#直接在select语句之前直接加上关键字 explain/desc
explain执行计划 各字段含义：
>>id 
	select 查询的序列号，表示查询中执行select子句或者是操作表的顺序（id相同时，执行顺序从上往下；id不同时，值越大的越先执行）--多表查询。
>>select_type
	表示select 的类型，常见的取值有SIMPLE（简单表，即不适用表连接或子查询）、PRIMARY（主查询，即外层的查询）、UNION（UNION中第二个或者后面的查询语句）、SUBQUERY(SELECT/WHERE之后包含了子查询)等。
>>type
	表示连接类型，行性能由好到差的连接类型为 NULL、system、const、eq_ref、ref、range、index、all.
>>possible_key
	显示这张表当中可能用得到的索引，一个或多个。
>>key
	实际用到的索引，如果为NULL,则没有使用索引。
>>Key_len
	表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际长度，在不能损失精度的前提下，长度越短越好
>>rows
	MySQL认为必须要执行查询的行数，在innoDB引擎中，是一个估值，可能并不总是准确的
>>filtered
	表示返回结果的行数占需读取行的百分比，filtered的值越大越好。
```

#### ·索引使用

```SQL
·验证索引效率
在未建立索引之前，执行如下SQL语句，查看SQL的耗时
SELECT * FROM tb_sku(表名) WHERE sn='xx';

针对字段创建索引
create index idx_sku_sn on tb_sku(sn);
创建联合索引
CREATE INDEX index_name ON table_name (column1, column2, ..., columnN);

然后再次执行相同的SQL语句，再次查看SQL的耗时
SELECT * FROM tb_sku(表名) WHERE sn='xx'\G;

·索引使用规则：(不是形式上的左侧和右侧，而是建立索引的左侧和右侧)
1.最左前缀法则
如果索引了多列（联合索引），要遵守最左前缀法则。最左前缀法则指的是查询从索引的最左侧开始，并且不跳过索引中的列。如果跳过某一列（即不存在某一列的查询条件），索引将部分失效（从跳过开始后面的字段索引失效）。
#最左前缀法则要求查询从左侧开始，但是查询语句中不要求顺序，只要求存在。
联合查询顺序：profession,age,status
#下面两条联合查询的结果是一样的，同时也满足最左前缀法则，索引完全有效
explain select * from tb_user where profession='软件工程' and age=31 and status='0';
explain select * from tb_user where age=31 and status='0' and profession = '软件工程';

#联合查询中跳过某一列查询，索引将部分失效
explain select * from tb_user where profession='软件工程' and age=31 ;
explain select * from tb_user where profession='软件工程' and status='0';

#联合查询不满足最左侧查询，索引全部失效
explain select * from tb_user where age=31 and status='0'; #最左侧字段不存在

2.范围查询
联合索引中，出现范围查询（>,<,between,!= 等），范围查询右侧的列索引将失效。
explain select * from tb_user where profession='软件工程' and age>30 and status='0';	#范围查询右侧的列索引失效
explain select * from tb_user where profession='软件工程' and age>=30 and status='0';	#在范围查询时增加=，相当于定义了一个明确的起点，这样右侧的列可能继续被查询优化器利用。
#这是因为一旦进行范围查询，数据库无法精确定位到一个索引的分界点，从而导致无法有效的利用后面的索引列。
解决办法：索引列顺序（尽可能将等值查询的列放在联合索引的前面，将用于范围的列放在后面）

·索引列运算
不要在索引列上进行运算操作，否则索引将会失效。
select * from tb_user where substring(phone,10,2)='15';#索引将会失效

·字符串不加引号
字符串类型的字段在使用时，不加引号(会发生隐式类型转换)，索引失效。

·模糊查询
如果仅仅是尾部进行模糊匹配（前缀匹配），索引不会失效，如果是头部进行模糊匹配，索引失效。
explain select * from tb_user where profession like '软件%'; 有效
explain select * from tb_user where profession like '%工程'; 失效

·索引的使用原则
	1.or连接的条件
用or分割开的条件，如果or前面的条件中有索引，而后面的列中没有索引，那么涉及的索引都不会被用到。（只有两侧的列都有索引的情况下才不会失效）#假设id有索引，phone有索引，age没有索引：
select * from tb_user where id = 10 or age =23;#没有使用任何索引。      //如果 OR 连接条件之一不能使用索引，那么整个查询可能不会利用任何索引。这是因为数据库可能需要评估每一条记录来确定是否满足任一条件。

	2.数据分布影响
如果MySQL评估使用索引比全表扫描更慢，则不使用索引。

·SQL提示
（如果一个字段既有单列索引，又具备联合索引的最左前缀法则，在查询的过程由mysql优化器自动选择的索引方式进行查询），在某些情境下，如果有多个索引，需要按照指定的索引进行查询时，就需要SQL提示。
SQL提示是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些认为的提示来达到优化操作的目的。#表明之后使用
	1.use index	 在你查询语句表名的后面，添加use index来提供你希望mysql去参考的索引列表，就可以让mysql不再考虑其他可用的索引。(只是建议，mysql还会进行权衡来判断是否要接收建议)
select * from tb_user use index(索引名) where 条件;

	2.ignore index	禁止查询优化器使用指定的索引
select * from tb_user ignore index(索引名) where 条件;

	3.force index  强制mysql使用一个特定的索引
select * from tb_user force index(索引名) where 条件;
#注意的是use/ignore/force index(索引名)，后面必须要加上where条件。以上 * index(索引名) 其中索引名可以为单个或多个，也可以为空。force index(索引名) 其中不能为空

·覆盖索引
尽量使用覆盖索引（查询使用了索引，并且需要返回的列在该索引中已经全部能找到），减少select *。即不用回表查询就能够找到所需要的数据。

explain select... 中显示的 extre 的值
# using index condition: 查找使用了索引，但是需要回表查询数据；
#using where;using index:查找使用了索引，并使用了索引覆盖即需要的数据都在索引列中能够找到，所以不需要回表查询数据。
```

```sql
前缀索引	针对字段值的前面一部分作为索引字段。
	当字段类型为字符串（varchar,text等）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO，影响查询效率。此时可以只将字符串的一部分前缀建立索引，这样大大节约索引空间，从而提高索引效率。
语法：
	create index 索引名 on 表名 (字段名(n));  # n代表取该字段的前n位建立索引。
前缀长度
	根据索引的选择性来决定，而选择是值不重复的索引值（基数）和数据表的记录总数的比值，索引选择性越高则查询效率越高，唯一索引的选择性是1，这种事最好的索引选择性，性能也最好。
	select count(distinct email) / count(*) from tb_user;
	select count(distinct substring(email,1,5)) / count(*) from tb_user;
	
对于下面这张图，想先通过前缀索引进行辅助索引查询，再通过查到的主键进行聚集索引，最后找到索引行，在进行整个字段的判断，最后输出正确的查询结果。
```

![image-20231106203534502](images/image-20231106203534502-16992741361594.png)

```sql
单列索引和联合索引
单列索引:即一个索引只包含一列（一个字段）； 	每一节点的键值只是一个字段值
联合索引：即一个索引包含多个列（多个字段）； 每一个节点的键值是多个字段值的组合,按照字段顺序依次排序
#在业务场景中，如果存在多个查询条件，考虑针对针对于查询字段建立索引时，建议建立联合索引，而非单列索引。
#	多条件联合查询时，MySQL优化器会评估哪个字段的索引效率更高，会选择该索引完成本次查询。

#	创建联合索引时，索引的顺序是按照创建来联合索引时的字段顺序确定的，并且要满足最左前缀法则（即最左侧的索引字段判断条件要存在）。因此在创建联合索引时，要选择合适的字段作为最左侧列。

```

![image-20240601162253055](images/image-20240601162253055.png)

#### ·索引设计原则

```
1.针对于数据量较大，且查询比较频繁的表建立索引；
2.针对常作为查询条件（where）、排序（order by)、分组（group by）操作的字段建立索引；
3.尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高；
4.如果是字符串类型的字段，字段的长度较长，建议建立前缀索引(前缀索引的长度根据根据索引选择性来确定)；
5.尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省空间，避免回表查询，提高查询效率
6.要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价越大，会影响增删该的效率；
7.如果索引不能存储NULL值，请在创建表时使用 NOT NULL 约束它。当优化器知道每列是否包含BULL值，他可以更好地确定哪个索引最有效的用于查询。
```

### SQL优化

#### 插入数据优化

```sql
#查看全局参数
select @@参数名;
#设置全局参数
set global 参数名= ..;
```

```sql
#######插入多条数据时，通过以下几种方式进行插入优化
·批量插入
	insert into 表名 value （），（），（）..;
·手动提交事务
	start transaction;
	... #操作
	commit;
·主键的插入顺序:
	最好是有序的数据插入，这样能够减少树的重新平衡操作，提高插入效率；有助于减少数据页的碎片化，因为数据按顺序存放，减少了数据页拆分和合并的频率
#######大批量数据插入
如果一次性需要插入大量的数据，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令进行插入。
#客户端连接服务端时，加上参数 --local-infile
	mysql --local-infile -u root -p
#设置全局参数 local-infile为1，启动mysql命令行客户端,并允许从本地文件系统导入数据.
查看 select @@local_infile;
设置 set global local_infile =1 ;
#执行load指令将准备好的数据加载到表结构中
load data local infile '/root/sqll.log' into table 'tb_user' fields terminated by ',' lines terminated by '\n';
```

#### 主键优化

```sql
·数据的组织方式
在innoDB存储引擎当中，表数据是根据主键顺序组织存放的，这种存储方式的表称为索引组织表（IOT）.  

·页分裂
页可以为空，也可以填充一半，也可以填满。每个页包含2-N行数据（如果一行数据较大，会行溢出），根据主键排序。
#当主键乱序插入的时候，可能就会发生页分裂。

·页合并
当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged）为删除并且它的空间变的允许被其他记录声明使用。	当页中删除的记录达到 MERGE_THRESHOLD(默认为 50%) ,innoDB会开始寻找最靠近的页（前或后）看看是否可以将两个页和并以优化空间使用。
#	MERGE_THRESHOLD：合并页的阈值，可以自己设置，在创建表或者创建索引时指定。

·主键设计原则
	选择合适的数据类型：满足业务需求的情况下，尽量降低主键的长度。
	使用自增主键：插入数据时，尽量选择顺序插入，选择使用 AUTO_INCREMENT 自增主键。
	尽量不要使用UUID 做主键或者是其他自然主键，如身份证号。
	业务操作时，尽量避免对主键的修改。
```

![image-20231108160938255](images/image-20231108160938255-16994309796501.png)

#### order by 优化

```sql
1.	using filesort: 通过表的索引或者全表扫描，读取满足条件的数据行，然后再排序缓冲区 sort buffer中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 Filesort 排序。
2.	using index:通过索引顺序扫描直接返回有序数据，这种情况即为 using index，不需要额外排序，操作效率高。
只有 order by 排序的顺序与索引的顺序一致，才能有效的使用联合索引，顺序不一致，则是违背最左前缀准则（即排序的顺序与索引的顺序一致）。 而 where 后面的条件顺序只有满足最左侧前缀准则才有效（即最左侧索引存在即可）。

#根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则；
#尽量使用覆盖索引
#多字段排序，一个升序，一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）
create index idx_age_pho on tb_user(age asc, phone desc); #创建索引时设定规则
如果不可避免地出现filesort,大数据量排序时，可以适当增大排序缓冲区大小sort_buffer_size(默认256K).
#问题：如果大于需要的排序缓冲区时，该怎么进行排序？
#查看当前排序缓冲区大小
show variables like 'sort_buffer_size';
```

```mysql
order by如果大于需要的排序缓冲区时，该怎么进行排序？
	当需要排序的数据量超过了MYSQL的排序缓冲区大小时，MYSQL将使用磁盘临时文件来辅助完成排序操作。通常在 using filesort 中很常见。

排序过程：
1.数据读取：MySQL 首先从表中读取符合查询条件的数据。
2.内存排序：尝试在内存中的排序缓冲区（sort buffer）对数据进行排序。
3.磁盘辅助：如果排序数据量超过了排序缓冲区的容量，MySQL 会将部分数据写入到磁盘上的临时文件中。
4.合并排序：使用类似于归并排序的技术，MySQL 会分批次地从磁盘读取和排序数据，最终合并成一个完整的排序结果
#影响：性能影响，磁盘i/o操作远比内存操作慢。
优化策略：
1.增加排序缓冲区大小
	通过设置 SET session sort_buffer_size = <新的大小>。
2.优化查询
	重新设计查询逻辑，尽量减少需要排序的数据量。
3.使用索引
	给需要排序的字段建立索引，这样可以利用索引顺序直接返回已排序的数据。
```

#### group by 优化

```
在分组操作中，可以通过索引来提高效率； 
在分组操作中，索引的使用也需要满足最左前缀法则；
```

#### limit 优化

```sql
如果是limit 2000000,10 ,此时需要MySQL排序前2000010记录，仅仅返回2000000-2000010的记录，其他记录丢弃，查询排序的代价非常大。

优化思路：一般分页查询时，通过创建 覆盖索引 能够比较好的提升性能，可以通过覆盖索引+子查询 形式进行优化：
explain select * from tb_sku t, (select id from tb_sku order by id limit 2000000,10) a where t.id = a.id;
```

#### count 优化

```sql
explain select count(*) from tb_user;
·MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行count(*)时会直接返回这个数，效率很高；
·innoDB 引擎在执行count(*)时，需要把数据一行一行的从引擎中读取出来，然后累计计数。
优化思路：自己计数。

·count的几种用法
1.count()是一个聚合函数，对于返回的结果集一行一行的进行判断，如果count函数的参数不是NULL，累计值就加1，否则不加，最后返回累计值。
用法： count(*)、 count(主键)、 count(字段)、count(1)
>count(主键)
innoDB引擎会遍历整张表，把每一行的主键id 值取出来，返回给服务层。服务层拿到主键后，直接按行进行累加。（主键不可能为null）
>count(字段)
如果没有not null约束：innoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层。服务层判断是否为null,不为null时计数累加。
如果有not null约束：innoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加。 
>count(1)
innoDB遍历整张表但不取值。服务层对与返回的每一行放一个数字‘1’进去，直接按行进行累加。
>count(*)
innoDB引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加。
#按照效率排序   count(*)>count(1)>count(主键id)>count(字段)
```

#### update 优化

```sql
在执行update 语句更新数据时，要根据索引字段进行更新（即条件对应的字段是有索引的字段），否则原本的行级锁就变成了整表锁。（主要是因为索引可以实现快速定位）
表级锁：MySQL中锁定粒度最大的一种锁，对当前操作的整张表加锁，实现简单，资源消耗也少，加锁快，不会出现死锁。但是锁定粒度大，触发锁冲突的概率最高，并发度低。
行级锁：MySQL中锁定粒度最小的一种锁，只针对当前操作的行进行加锁。行级锁能大大减少数据库操作的冲突。加锁的开销也大，加锁慢，会出现死锁。加锁粒度最小，并发度高。
#	innoDB的行锁是针对索引加的行锁，不是针对记录加的锁，并且该索引不能失效，否则会升级为表锁 

事务执行update更新表中的数据时，有索引的字段加的是行级锁，没索引的字段加的表级锁？
```

### 视图/存储过程/触发器

#### 视图

```sql
·介绍
视图（view）是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表（基表），并且是在使用视图时动态生成的。
通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。所以在创建视图时，主要的工作是在创建这条SQL查询语句上。
#创建视图可以让你保存复杂的查询，并像访问普通表一样使用它们。这可以帮助简化复杂的SQL操作，提高数据访问的效率，并实现更好的数据安全管理。
1.创建（或替换）一个视图
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [WITH [CASCADED|LOCAL]CHECK OPTION]
		替换某一试图								通过select查询返回内容作为视图的内容			
2.查询
查看创建视图语句	SHOW CREATE VIEW 视图名称；
查看视图数据		 SELECT * FROM 视图名称  (条件)...;

3.修改
方式一： CREATE OR REPLACE VIEW 视图名称[(列名列表)] AS SELECT语句 [WITH [CASCADED|LOCAL]CHECK OPTION]
方式二： ALTER VIEW 视图名称（[列名列表]） AS SELECT语句 [WITH [CASCADED|LOCAL]CHECK OPTION]

4.删除
DROP VIEW [IF EXISTS] 视图名称 [视图名称]...;

#往视图中插入数据时实际上是插入到视图所关联的基础表中。视图本质是一个虚拟表，它并不存储实际数据，而是基于基础表的数据生成一个查询结果集.
#视图的插入、更新、删除能否影响基础表取决于视图的定义和复杂性；简单视图通常是单个表，不包含复杂的SQL构造，插入、更新和删除操作会直接影响基础表，而对于复杂视图（设计多表查询、聚合函数、子查询等），视图可能对这些操作进行一些限制，或者禁止这些操作。
·视图的检查选项（只有具备检查选项时，才会检查语句中的条件）
	当使用 WITH [] CHECK OPTION 子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如， 插入、更新、删除，以使其符合视图的定义。MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致（不仅会检查当前视图依赖的条件，还会检查依赖视图中的依赖条件，前提是依赖视图的检查选项也存在(如果是cascaed则不管原来的那个视图有没有检查选项都会检擦)）。为了确定检查的范围，mysql提供了两个选项： CASCADED 和 LOCAL ，默认值是  CASCADED 
·两种检查选项：
	CASCADED （级联）：基于另一个视图创建视图时，如果当前语句中包含了检查选项(CASCADED)它还会检查依赖视图中的规则以保持一致，不仅会检查当前视图依赖的条件，还会检查依赖视图中的依赖条件(即使依赖视图在创建时没有加检查选响)。
	LOCAL（本地）：基于另一个视图创建视图时，如果当前语句中包含了检查选项(LOCAL)，会检查当前语句的依赖条件，并递归检查依赖视图的创建语句中是否包含检查选项，如果有，而且是 LOCAL，则会检查当前递归层的依赖视图；如果是 CASCADED ，则会检查之后（包括当前）所有的依赖视图的条件，如果没有检查条件，则不会进行检查。

总结：
CASCADED（级联）：当使用CASCADED选项创建视图时，系统会递归地检查视图的依赖关系。具体来说，如果在当前创建视图的语句中包含了CASCADED选项，系统会检查当前视图依赖的条件，并且会继续检查依赖视图中的规则，以保持一致性。即使依赖视图在创建时没有加入CASCADED选项，系统也会检查它们的依赖条件，确保整个依赖链中的条件都满足。

LOCAL（本地）：相比之下，当使用LOCAL选项创建视图时，系统只会检查当前语句的依赖条件，并递归检查依赖视图的创建语句中是否包含LOCAL选项。如果有LOCAL选项，系统会检查当前递归层的依赖视图。如果依赖视图中包含CASCADED选项，则会检查之后（包括当前）所有的依赖视图的条件。如果依赖视图中没有检查条件，则不会进行检查

视图为什么要有检查选项呢？
	保证数据的完整性。确保所有通过视图执行的数据修改（更新、插入、删除）都符合视图定义的条件。
```

##### 视图的更新与作用

```sql
#要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。
如果视图包含以下任何一项，则视图不可更新：
1.聚合函数或窗口函数（ SUM()、 MIN()、 MAX()、 COUNT()等）
2.DISTINCT
3.GROUP BY
4.HAVING
5.UNION 或者 UNION ALL

作用：
1.简单：视图不仅可以简化用户对数据的理解，也可以简化它们的操作。那些经常被使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件。
2.安全：数据库可以授权，但不能授权到数据库特定行和特定列上。通过视图，用户只能查询和修改它们所能见到的数据。
3.数据独立：视图可以帮助用户屏蔽真实表结构变化带来的影响。
```

##### 视图与表

```
表（Table）：
	表是数据库中的基本结构，用于存储数据。每个表都包含一个或多个列（字段），每个列定义了表中存储的特定类型的数据。表中的每一行包含了一条记录，每个字段存储了记录的特定信息。
	表可以用于存储大量的数据，它们定义了数据的结构（列的数据类型、长度等）和约束（主键、外键等），确保数据的一致性和完整性。
	表可以进行插入（INSERT）、查询（SELECT）、更新（UPDATE）和删除（DELETE）等操作，是数据库中最基本的数据存储和操作单元。
	
视图（View）：
	视图是虚拟的表，它是基于一个或多个表的查询结果集。视图本身不包含实际的数据，而是一个动态的、基于查询的结果集，提供了一种按特定条件筛选和呈现数据的方式。
	视图可以包含来自一个或多个表的列，甚至可以包含计算字段和聚合函数的结果。通过视图，用户可以方便地查看和操作数据库中的数据，而无需了解底层的表结构和复杂的查询语句。
	视图提供了一种安全性机制，可以限制用户只能访问视图中的特定列或特定行，而不需要直接访问表。
总的来说，表是实际存储数据的地方，而视图是对表的一种抽象，它提供了一种更灵活、方便、安全的数据访问方式。视图可以简化复杂的查询操作，提高数据的安全性和隐私性。
```

#### 存储过程

```sql
·介绍
存储过程（Stored Procedure）是一种预先编译并存储在数据库管理系统中的一组SQL语句（集合）。存储过程可以接受参数、执行SQL查询、执行各种数据库操作，然后返回结果。存储过程通常用于执行特定的数据库操作或者完成特定的任务。
存储过程有以下几个主要特点：
	1.预编译：存储过程在数据库中被预先编译，这样一来，当存储过程被调用时，数据库管理系统无需重新解析和编译SQL语句，从而提高了执行速度。
	2.参数：存储过程可以接受输入参数，这些参数可以在存储过程内部使用，使得存储过程更加灵活和通用。参数可以是输入参数、输出参数或者输入输出参数。
	3.封装性：存储过程可以包含一系列SQL语句，控制结构（例如循环、条件判断等），以及错误处理逻辑，从而实现了一定程度的封装性和安全性。
	4.复用性：由于存储过程被存储在数据库中，多个应用程序可以共享并重复使用相同的存储过程，提高了代码的复用性。
	5.安全性：存储过程可以定义访问数据库的权限，数据库管理员可以控制哪些用户或者角色可以执行特定的存储过程。
	使用存储过程的主要优势包括提高了性能、减少了网络流量（因为只需要传递存储过程的名称和参数，而不是完整的SQL语句）、增加了安全性，以及提供了更好的代码组织和维护性。不同的数据库管理系统（如MySQL、SQL Server、Oracle等）都支持存储过程的创建和使用。

·语法
1.创建
	CREATE PROCEDURE 存储过程名称([参数列表])
	BEGIN 
		--SQL 语句
	END;

2.调用
	CALL 存储过程名称([参数]);

3.查看存储过程
	SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA = '数据库名'; 
	#查询指定数据库的存储过程及状态信息
	SHOW CREATE PROCEDURE 存储过程名称; 
	# 查询某个存储过程的定义
#在命令行中，执行创建存储过程的SQL时，需要通过关键字delimiter指定SQL语句的结束符（因为命令行见到 ‘;’会认为结束并执行）。
命令行中： delimiter 结束符
最后在用：  delimiter ; 改回原来的状态
	
4.删除
	DROP PROCEDURE [IF EXISTS] 存储过程名称;
	
·语法结构
1.变量 
(1)系统变量：是MySQL服务器提供的，不是用户定义的，属于服务器层面。分为全局变量(GLOBAL),会话变量(SESSION)
>查看系统变量  (不加选项，默认是 session)
	SHOW [SESSION|GLOBAL] VARIABLES;     		  ---查看所有系统变量
	SHOW [SESSION|GLOBAL] VARIABLES LIKE '...';   ---通过模糊匹配的方式查找变量
	SELECT @@[SESSION|GLOBAL].系统变量名;		   ---查看指定变量的值
>设置系统变量
	SET [SESSION|GLOBAL] 系统变量名 = 值;
	SET @@[SESSION|GLOBAL].系统变量名 = 值;
#	如果没有指定SESSION/GLOBAL,默认是SESSION，会话变量
#	mysql服务重新启动之后，所有的全局参数都会失效，可以通过改配置文件来永久生效 /.../my.cnf

(2)用户定义变量：是用户根据需求自己定义的变量，用户变量不用提前声明，在用的时候直接使用“@变量名”使用就可以。其作用域为当前连接。
>赋值
	SET @var_name1 = expr [,@var_name2=expr]...; #一次可以设置多个变量
	SET @var_name1 := expr [, @var_name2=expr]...;   #一般选用
	SELECT @var_name1 = expr [, @var_name2=expr]...;
	SELECT 字段名 INTO @var_name FROM 表名; #将表中查询的数据赋值给变量
>使用
	SELECT @var_name1 [,@var_name2,...];
	
(3)局部变量：根据需要定义的在局部生效的变量，访问之前，需要DECLARE声明。可以作为存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的 BEGIN ... END 块。(声明和赋值的操作以及使用都是在存储过程内)
>声明
	DECLARE 变量名 变量类型 [DEFAULT ...]; # default ... 指定默认值
#变量类型(数据库字段类型)：INT BIGINT CHAR VARCHAR DATE TIME等
>赋值
	SET 变量名=值;
	SET 变量名:=值;
	SELECT 字段名 INTO 变量名 FROM 表名 ...;
	
·if    #流程控制语句
语法：
IF 条件1 THEN
		...
ELSEIF 条件2 THEN
		...
ELSE
		...
END IF;

·存储过程的参数
	类型								含义								备注
	IN					该类参数作为输入，也就是需要调用时传入值				默认
	OUT					该类参数作为输出，也就是该参数可以作为返回值				
	INOUT				既可以作为输入参数，也可以作为输出参数
语法：
	CREATE PROCEDURE 存储过程名([IN/ OUT/ INOUT 参数名 参数类型])
	BEGIN
			---SQL语句
	END;
	
·case  #流程控制语句
语法一：
	CASE case_value
		WHEN when_value1 THEN statement_list1
		[WHEN when_value2 THEN statement_list2]
		...
		[ELSE statement_list]
	END CASE;
语法二：
	CASE
		WHEN search_condition1 THEN statement_list1
		[WHEN search_condition2 THEN statement_list2]
		...
		[ELSE statement_list]
	END CASE;

循环结构： while 、 repeat、
·while   #流程控制语句
while是有条件的循环控制语句。满足条件后，在执行循环体中的SQL语句。具体语法：
	WHILE 条件 DO   #满足条件，则开始循环
		SQL逻辑...
	END WHILE;
·repeat 
repeat是有条件的循环控制语句，当满足条件的时候退出循环。具体语法：
	REPEAT    #先执行一次逻辑，然后判断是否满足，如果满足，则退出循环
	 	SQL逻辑...
	 	UNTIL 条件
	END REPEAT;
·loop
loop实现简单的循环，如果不在SQL逻辑中增加退出条件，可以用其来实现简单的死循环。loop配合以下两个语句使用：
	LEAVE : 配合循环使用，退出循环。
	ITERATE:必须用在循环中，作为跳过当前循环剩下的语句，直接进入下一次循环。
	[begin_label:] LOOP
		SQL逻辑...
	END LOOP [end_label];
LEAVE label ;  #退出指定标记的循环体
ITERATE label; #直接进入下一次循环

·游标
游标（Cursor）是一种用于在数据库管理系统中处理查询结果集的数据库对象。游标允许程序对查询结果集进行遍历，逐行处理数据。它通常用于编程语言与数据库之间的交互，允许程序对查询结果进行逐行处理，而不是一次性将所有数据加载到内存中。
	在使用游标时，可以执行查询操作，然后通过游标逐行获取查询结果集的数据，进行相应的处理，例如更新、删除或插入数据。游标提供了对结果集的灵活控制，允许程序员在结果集中前进、后退、跳转到特定位置等操作。
	游标的基本步骤：
	1.声明游标：在程序中声明一个游标，用于存储查询结果集的引用。
	DECLARE 游标名称 CURSOR FOR 查询语句；
	2.打开游标：执行查询语句，并将查询结果集与游标关联起来，时游标指向结果集的第一行。
	OPEN 游标名称；
	3.逐行处理数据：使用游标提供的功能，逐行获取数据，并进行相应的处理。
	FETCH 游标名称 INTO 变量 [，变量];#变量的声明要在游标声明之前。
	4.关闭游标：在处理完查询结果集后，关闭游标，释放相关资源。
	CLOSE 游标名称;
·条件处理程序
用于在数据库中处理错误和异常情况。当数据库操作发生错误时，条件处理程序可以捕获这些错误并执行相应的处理逻辑。
条件处理程序的几个关键概念：
	异常（Exception）： 异常是指在程序执行过程中发生的错误或意外情况。在数据库中，异常通常与SQL语句执行相关。
	异常处理程序（Exception Handler）： 异常处理程序是一段代码，用于捕获并处理异常。当异常发生时，相关的异常处理程序将被调用来处理异常情况。
	异常类别（Exception Category）： 异常可以分为不同的类别，表示不同类型的错误，例如唯一约束冲突、空值插入、数据类型不匹配等。每个异常类别都可以有相应的异常处理程序。
语法：
DECLARE handler_action HANDLER FOR condition_value [,condition_value]... statement;
其中：
handler_action
	CONTINUE: 继续执行当前程序
	EXIT： 终止执行当前程序
condition_value
	SQLSTATE value:状态码，如02000
	SQLWARNING:所有以01开头的 SQLSTATE 代码的简称
	NOT FOUND:所有以02开头的 SQLSTATE 代码的简称
	SQLEXCEPTION:所有没有被SQLWARNING和NOT FOUND 捕获的SQLSTATE简写
```

#### 存储函数

```sql
存储函数是有返回值的存储过，存储函数的参数只能是IN类型。具体语法如下：
CREATE FUNCTION 存储函数名([参数列表])
RETURNS type [characteristic...]
BEGIN
	--SQL语句
	RETURN ...;
END;
characteristic说明：
	DETERMINISTIC :相同的输入参数总是产生相同的结果
	NO SQL : 不包含SQL语句
	READS SQL DATA :包含读取数据的语句，但不包含写入数据的语句
```

#### 触发器

```SQL
	触发器是与表有关的数据库对象，指在insert/update/delete之前或之后，触发并执行触发器中定义的SQL语句集合，。触发器的这种特性可以协助应用在数据库端确保数据的完整性，日志记录，数据校验等操作。
使用别名OLD和NEW来应用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在的触发器还支持行级触发，不支持语句级触发。
	触发器类型								NEW和OLD
INSERT型触发器					NEW表示将要或者已经新增的数据
UPDATE型触发器					OLD表示修改之前的数据，NEW表示将要或修改后的数据
DELETE型触发器					OLD表示将要或者已经删除的数据

语法
·创建
	CREATE TRIGGER trigger_name
	BEFORE/AFTER INSERT/UPDATE/DELETE      #AFTER 表示在操作之后触发
	ON 表名 FOR EACH ROW --行级触发器
	BEGIN
		trigger_stmt;  -- 逻辑实现	
	END;
·查看
	SHOW TRIGGER;
·删除
	DROP TRIGGER [数据库名.]trigger_name; #如果没有指定数据库名，默认为当前数据库。
	
案例：
#insert触发     对于tu_user表执行插入操作时会触发，触发后向出发记录表中插入插入记录
create trigger tu_user_insert_trigger
    after insert
    on tb_user
    for each row
begin
    insert into user_logs(id, operation, operate_time, operate_id, operate_params) VALUE
        (null, 'insert', now(), NEW.id,
         concat('插入的数据内容为:', new.id, new.name, new.phone, new.email, new.profession));
end;

#update触发	对于tu_user表执行更新操作时会触发，触发后向出发记录表中插入更新记录
create trigger tu_user_update_trigger
    after update
    on tb_user
    for each row
begin
    insert into user_logs(id, operation, operate_time, operate_id, operate_params) VALUE
        (null, 'update', now(), NEW.id,
         concat('更新之前的数据内容为:', old.id, old.name, old.phone, old.email, old.profession,
                '|更新之后的数据内容为:', new.id, new.name, new.phone, new.email, new.profession));
end;

#delete触发  对于tu_user表执行删除操作时会触发，触发后向出发记录表中插入删除记录
create trigger tu_user_delete_trigger
    after delete
    on      tb_user
    for each row
begin
    insert into user_logs(id, operation, operate_time, operate_id, operate_params) VALUE
        (null, 'delete', now(), OLD.id,
         concat('删除之前的数据内容为:', old.id, old.name, old.phone, old.email, old.profession));
end;
```

![image-20231108161010139](images/image-20231108161010139-16994310170602.png)

### 锁

```
锁是计算机协调多个进程或线程并发访问某一资源的机制。在数据库中，除了传统的计算资源（CPU、RAM、I/O）的竞争外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性，有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。
通过锁的粒度分类：
1.全局锁：锁定数据库中的所有表。
2.表级锁：每次操作锁定整张表。
3.行级锁：每次操作锁定对应的行数据。
```

#### 全局锁

```sql
全局锁是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML的写语句，DDL语句，以及更新操作的事务提交语句都将被阻塞
--典型的使用场景是做全库的逻辑备份，对所有的表进行锁定，从而获得一致性视图，保证数据的完整性。
flush tables with read lock;   #加全局锁
unlock tables;                 #解全局锁


远程备份：终端 mysql -h172.19.77.236 -uyuan -pYuan0812@ itcast > F:/md文件/itcast.sql
本地备份：终端 mysql  -uyuan -pYuan0812@ itcast > F:/md文件/itcast.sql
#在innoDB引擎中，可以在备份中加上参数 --single-transaction 参数来完成不加锁的一致性数据备份。
mysql --single-transaction -uyuan -pYuan0812@ itcast > F:/md文件/itcast.sql
```

#### 表级锁

```sql
每次操作锁住整张表。锁定力度大，发生冲突的概率最高，并发度最低。应用在MyISAM 、innoDB、BDB等存储引擎中。
表级锁分为以下三类：
1.表锁
2.元数据锁(meta data lock, MDL)
3.意向锁
·表锁
	1.表共享读锁（read lock）    
	2.表独占写锁（write lock）
	语法：
		1.加锁： lock tables 表名... read/write;
		2.解锁： unlock tables;
#共享读锁允许多个事务同时读取表的数据，而不会相互干扰。这意味着多个事务可以同时获取表的共享读锁，从而可以并发地读取表中的数据。同时加了共享读锁，不允许进行写操作。
#独占写锁是一种排他锁，只允许一个事务独占地对表进行写操作。当一个事务获取了表的独占写锁时，其他事务无法同时获取相同表的共享读锁或独占写锁。独占写锁通常用于写入、更新和删除等可能改变表数据的操作，保证了在写操作时不会有其他事务读取或写入相同的数据。
#独占写锁会阻止其他事务的读取和写入操作，而持有独占写锁的事务既可以进行写操作，也可以进行读操作。

·元数据锁(meta data lock, MDL)
	MDL加锁过程是系统自动控制的，无需显示使用。在访问一张表的时候会自动加上。MDL锁主要作用是维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。（为了避免DML和DDL冲突，保证读写的正确性）
查看元数据锁：
select object_type,object_schema,object_name,object_type,lock_duration from performance_schema.metadata_locks;

·意向锁
为了避免DML在执行时，加的行锁和表锁的冲突，在innoDB中引入了意向锁，使得表锁不用检查每行数据是否加了行锁，使用意向锁来减少表锁的检查。
1.意向共享锁（IS）：由语句 select ... lock in share mode 添加
2.意向排它锁（IX）：由insert 、update、delete、select ... for update 添加(自动添加)
	意向共享锁（IS）:与表锁共享锁（read）兼容，与表锁排它锁（write）互斥
	意向排他锁（IX）：与表锁共享锁（read）和表锁排它锁（write）都互斥。意向锁之间不会互斥。
查看意向锁及行锁的加锁情况：
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```

![image-20231109103753865](images/image-20231109103753865-16994974753411.png)

#### 行级锁

```sql
行级锁，每次操作锁住对应的行数据。锁定粒度更小，发生锁冲突的概率最低，并发度最高。应用在innoDB存储引擎中。
innoDB的数据是基于索引组织的，行锁是通过对索引上的索引项加锁来实现的，而不是对记录加锁。对于行级锁，分为三类
1.行锁（record lock）:锁定单个行记录的锁，防止其他事务对此进行update和delete。在RC、RR隔离级别下都支持。
2.间隙锁（gap lock）:锁定索引记录间隙（不含该记录），确保索引记录间隙不变，防止其他事务在这个间隙进行	     INSERT，产生幻读。在RR隔离级别下都支持。
3.临建锁（next-key lock）:行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙gap.在RR隔离级别下支持。
```

##### 1.行锁

​	1.共享锁（S）：允许一个事务去读一行，阻止其他事务获得相同数据集的排他锁。
​	2.排他锁（X）：允许获取排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁。
#共享锁和共享锁之间是共享的，而和排他锁是冲突的；排它锁和共享锁及排它锁都冲突。（此处针对的是并发访问过程中，针对与同一行数据操作的加锁情况）

![](images/image-20231109103822610-16994975040912.png)

默认情况下，innoDB在REPEATABLE READ 事务隔离级别运行。innoDB使用next-key 锁进行行搜索和索引扫描，以防止幻读。

​	1.针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将自动优化为行锁。
​	2.innoDB的行锁是针对于索引加的锁，不通过索引条件检索数据，那么innoDB将对表中的所有记录加锁，此时就会升级为表锁。

##### 2.间隙锁/临键锁

默认情况下，innoDB在REPEATABLE READ 事务隔离级别运行。innoDB使用next-key 锁进行行搜索和索引扫描，以防止幻读。

​	1.索引上的等值查询（唯一索引），给不存在的记录加锁时，优化为间隙锁。

​	2.索引上的等值查询（普通索引），向右遍历时最后一个值不满足查询需求时，next-key lock 退化为间隙锁。

​	3.索引上的范围查询（唯一索引）--会访问到第一个不满足条件的值为止。

```sql
#间隙锁的唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙采用间隙锁。
```

![image-20231109112815483](images/image-20231109112815483-16995004965393.png)

### InnoDB引擎

#### 逻辑存储结构

![image-20231109113115992](images/image-20231109113115992-16995006770774.png)

```
表空间（ibd文件）：一个mysql实例可以对应多个表空间，用于存储记录，索引等数据。
段：分为数据段、索引段、回滚段，innoDB是索引组织表，数据段就是B+树的叶子节点，索引段就是B+树的非叶子节点。段用来管理多个区（extent）。
区：表空间的单元结构，每个区的大小为1M，默认情况下，innoDB存储引擎页大小为16K，即一个区中有64个连续的页。
页：innoDB存储引擎磁盘管理的最小单元，每个页的大小默认为64K。为了保证也得连续性，innoDB存储引擎每次从磁盘申请4-5个区。
行：innoDB存储引擎数据是按行进行存放的。
```

#### 架构

#### 事务原理

```
事务：是一组操作的集合，是一个不可分割的单位，是误会吧所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。
四大特性：原子性、一致性、隔离性、持久性。
事务原理
1.原子性
2.一致性
3.隔离性
4.持久性
```

![image-20231109212733109](images/image-20231109212733109-16995364556191.png)

```
·redo log   （持久性）
	重做日志，记录的是事务提交时数据页的物理修改，是用来实现事物的持久性。
该日志文件由两部分构成：重做日志缓冲 和 重做日志文件，前者是在内存中，后者是在磁盘。当事务提交之后会把所有修改信息都存到该日志文件中，用于在刷新脏页到磁盘发生错误时，进行数据恢复。
·undo log   （原子性）
	回滚日志，用于记录数据被修改之前的信息，作用包含两个：提供回滚 和 MVCC(多版本并发控制)。
undo log 和 redo log记录物理日志不一样，它是逻辑日志。可以认为当delete一条记录时，undo log 会记录一条对应的insert记录，反正亦然，当update一条语句时，它记录一条对应相反的update记录。当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。
undo log销毁：undo log在事务执行时产生，事务提交时，并不会立即删除undo log，因为这些日志还可能用于MVCC
undo log存储：undo log 采用段的方式进行管理和记录，存放在前面介绍的rollback segment回滚段中，内部包含1024个undo log segment.
```

#### MVCC

```
基本概念--当前读
	读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。如：
	select ... lock in share mode(共享锁)，select ...for update、update、insert、delete（排它锁）都是一种当前读。
基本概念--快照读
	简单的select（不加锁）就是快照读，快照读，读取的是记录数据的可见版本，有可能是历史数据，不加锁，是非阻塞读。
	1.Read Committed:每次select，都生成快照读。
	2.Repeatable Read:开启事务后第一个select语句才是快照读的地方。
	3.Serializable:快照读会退化为当前读。

MVCC 全称多版本并发控制。维护一个数据库的多个版本，使得读写操作没有冲突，快照读为MySQL实现MVCC提供了一个非阻塞读功能。MVCC的具体实现，还需依赖于数据库记录中的三个隐式字段，事务链表，uodo log日志，readView.
```

```
MVCC--实现原理
·记录当中的隐藏字段
	DB_TRX_ID 	:最近修改事务id，记录插入这条记录或最后一次修改记录的事务id.
	DB_ROLL_PTR :回滚指针，指向这条记录的上一个版本，用于配合undo log ,指向上一个版本。
	DB_ROW_ID	:隐藏主键，如果表结构没有指定主键，将会生成该隐藏字段，产生一个聚簇索引。
查看ibd文件中的数据字典信息：
命令行：ibd2sdi 表空间文件名

·undo log日志
	回滚日志，在insert、update、delete的时候产生的便于数据回滚的日志。
	当insert的时候，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除。
	而update、delete的时候，产生的undo log日志不仅在回滚时需要，在快照读的时候也需要，不会立即被删除。
	
```

### MySQL管理

#### 系统数据库

```
mysql数据安装完成后，自带了四个数据库，具体作用如下：
数据库						含义
mysql				存储Mysql服务器正常运行所需要的各种信息（时区、主从、用户、权限等）
information_schema  提供了访问数据库元数据的各种表和视图，包括数据库、表、字段类型及访问权限等。
performance_schema	为Mysql服务器运行时状态提供一个底层监控功能，主要用于收集数据库服务器性能参数
sys					包含一系列DBA和开发人员利用performance_schema性能参数进行性能调优和诊断的视图
```

#### 常用工具

```sql
·mysql   该mysql不是指mysql服务，而是指mysql的客户端工具
语法：
	mysql [options] [database]
选项：
	-u ,--user=name					#指定用户名
	-p ,--password                  #指定密码
	-h ,--host						#指定服务器ip和域名
	-P ,--port						#指定连接端口
    -e ,--execute					#执行SQL语句并退出
# -e 选项可以在mysql客户端执行SQL语句，而不用连接到MySQL数据库再执行，对于一些批处理脚本，这种方式尤为方便
示例：	mysql -uroot -p123456 db01 -e "select * from stu"

·mysqladmin
是一个执行管理操作的客户端程序。可以用它来检查服务器的配置和当前状态、创建并删除数据库等。
	mysqladmin --help 通过帮助文档查看选项
示例：root@DELL:/home/yuan# mysqladmin -uyuan -pYuan0812@ variables

·mysqlbinlog
由于服务器生成二进制日志文件以二进制格式保存，所以想要检查这些文本的文本格式，需要使用到 mysqlbinlog日志管理工具。位置： /var/lib/mysql
```

![image-20231110113046296](images/image-20231110113046296-16995870480421.png)

```
·mysqlshow 客户端对象查找工具，用来很快的查找存在哪些数据库、数据库中的表、表中的列或者索引。
语法：
	mysqlshow [options] [db_name [table_name [col_name]]]
选项：
	--count   显示数据库及表的统计信息（数据库、表均可以不指定）
	-i		  显示指定数据库或者指定表的状态信息
示例：
	#查询每个数据库的表的数量及表中记录的数量
	mysqlshow -uyuan -pYuan0812@ --count
	#查询itcast库中每个表的字段数及行数
	mysqlshow -uyuan -pYuan0812@ itcast --count
	#查询itcast库中指定tb_user表的详细状况
```

```
·mysqldump 用来备份数据库或在不同数据库之间进行数据迁移。备份内容包括创建表及插入表的SQL语句。通过 > 指定备份路径
```

![image-20231110154345777](images/image-20231110154345777.png)

```sql
·mysqlimport / source
数据导入工具，用来导入mysqldump加 -T参数后导出的文本文件
语法：
	mysqlimport [options] db_name textfile [textfile2..]
	
如果要导入sql文件，可以使用mysql中的source指令：
mysql命令行：	source /root/xxx.sql      #通过备份的sql文件，来实现导入
```

## 日志

### 二进制日志--不包含查询语句

日志查看

```
通过二进制日志查询工具 mysqlbinlog 来查看二进制日志：
$ mysqlbinlog [参数选项] logfilename
参数选项
	-d  指定数据库名称，只列出指定的数据库相关操作
	-o	忽略掉日志中的前n行命令
	-v	将行事件（数据变更）重构为SQL语句
	-W	将行事件（数据变更）重构为SQL语句，并输出注释
```

日志删除

![image-20240618205928066](C:\Users\yuan\AppData\Roaming\Typora\typora-user-images\image-20240618205928066.png)

```mysql
在mysql中进行删除
在mysql的配置文件中配置二进制日志的过期时间，设置之后，二进制日志过期会自动删除。
变量：
binlog_expire_logs_auto_purge #二进制日志过期开关 
binlog_expire_logs_seconds	  # 过期时间设置
```

### 查询日志--包含了所有的增删改查等操作

​	查询日志中记录了客户端的所有操作语句，而二进制日志中不包含查询数据的SQL语句。默认情况下，查询日志是未开启的。

```mysql
通过变量 general_log 来查询和设置查询日志开关
show variables like '%general%'
```

### 慢查询日志

​	慢查询日志中记录了所有执行时间超过参数 long_query_time 设置值并且扫描记录不小于 min_examined_row_limit 的所有SQL语句的日志，默认未开启。

```mysql
long_query_time 默认为10s,最小为0，精度可以为微秒
参数： 
	slow_query_log	#慢查询日志开关
	long_query_time #慢查询时间设置
	
默认情况下，不会记录管理语句，也不会记录不使用索引进行查找的查询。
log_show_admin_statements #记录执行较慢的管理语句开关
log_queries_not_using_indexes #记录执行较慢的未使用索引的语句开关。
```

### 错误日志

​	错误日志记录着mysql启动和停止,以及服务器在运行过程中发生的错误的相关信息。在默认情况下，系统记录错误日志的功能是关闭的，错误信息被输出到标准错误输出。

```mysql
变量 log_error	#错误日志关联的文件
```

​    



## MySQL安装

```
远程地址连接
cd /etc/mysql/mysql.conf.d/
sudo vim mysqld.cnf
重启mysql服务：sudo service mysql restart

mysql -p主机地址 -u用户名 -p密码
将其中的bind_address=127.0.0.1 注释掉，允许远程连接
账号 ：yuan
密码： Yuan0812@
```

## MYSQL服务的启动、停止和重新启动

```
通过service mysqld restart  或者是 systemctl mysqld restart
通过service mysqld start  或者是 systemctl mysqld start
通过service mysqld stop  或者是 systemctl mysqld stop

或者是指定路径下的操作
sudo /etc/init.d/mysql start
sudo /etc/init.d/mysql restart
sudo /etc/init.d/mysql stop
```

## 1.apt-get 服务

```bash
首先： 
#这个命令，会访问源列表里的每个网址，并读取软件列表，然后保存在本地电脑。
 sudo apt-get update
#这个命令，会把本地已安装的软件，与刚下载的软件列表里对应软件进行对比，如果发现已安装的软件版本太低，就会提示你更新。
 sudo apt-get upgrade
#总而言之，update是更新软件列表，upgrade是更新软件。
```

## 2.开启FTP

```bash
sudo apt-get install vsftpd  
#安装FTP服务，安装完成后，FTP服务器就搭建好啦，可以在其他电脑通过FTP服务访问控制Ubuntu系统。
#如果是“服务器拒绝SFTP，但是监听到FTP连接”错误，是因为没有安装SSH。
	通过 sudo apt-get install ssh 来安装ssh
	sudo service ssh start 开启ssh服务
#如果修改了vsftp.conf配置文件，需要保存并退出，然后执行
	sudo /etc/init.d/vsftpd restart  重启服务
```

## 3.通过ssh远程连接

```bash
1.#linux系统首先安装ssh客户端和服务器
sudo apt install openssh-client openssh-server
2.#开启ssh服务
sudo service ssh start 
3.#进行远程连接
ssh username@remote_host
如果出现“ECDSA主机密钥已更改，您已请求严格检查”说明本地计算机上保存的密钥没用了，一般这个问题，是你重置过你的服务器后，你再次想访问会出现这个问题。
	先输入：ssh-keygen -R hostname			例如：ssh-keygen -R 47.100.3.227
#意思就是从 known_hosts 文件中删除所有属于 hostname 的密钥。然后在登陆就可以啦。
```

## 4.通过ssh实现在远程和本地之间传输文件的2种方法

```bash
方法一：使用scp 命令通过SSH复制文件（或目录）
将文件从远程及其复制到本地机器（假设远程为linux）：
	scp user_name@ip_address:/home/user_name/filename .
将文件从本地机器复制到远程机器
	scp filename  user_name@ip_address:/home/...path
# 如果是复制目录，则加上 -r 选项

方法二：使用 rsync 通过SSH复制文件和目录
将文件从远程机器复制到本地机器
	rsync user_name@ip_address:/home/user_name/filename .
将文件从本地机器复制到远程机器
	rsync filename user_name@ip_address:/home/...path
#同样的 加上 -r 选项复制目录

```

​                                                                                                                               
