## SQL基础语法

#### 1.操作数据库

（1）创建数据库

语法格式：

```sql
CREATE DATABASE [IF NOT EXISTS] <数据库名>[[DEFAULT] CHARACTER SET <字符集名>][[DEFAULT]COLLATE<校对规则名>]；
```

语法说明：

- <数据库名>：创建数据库的名称。数据库名称必须符合操作系统的文件夹命名规则。
- IF NOT EXISTS：在创建数据库之前进行判断，只有该数据库目前尚不存在时不能执行操作。此选项可以用来避免数据库已经存在二重复创建的错误。
- [DEFAULT] CHARACTER SET：指定数据库的默认字符集。
- [DEFAULT] COLLATE：指定字符集的默认校对规则。



示例：

创建数据库database_name

```sql
create database datebase_name;
```

（2）删除数据库

语法格式：

```sql
DROP DATABASE [ IF EXISTS ] <数据库名>
```

语法说明：

- <数据库名>：指定要删除的数据库名。
- IF EXISTS：用于防止当数据库不存在时发生错误。
- DROP DATABASE：删除数据库中的所有表格并且同时删除数据库。使用此语句时一定要慎重和小心，以免出现错误删除。如果要使用DROP DATABASE，需要获得数据库DROP权限。



示例：

```sql
drop database database_name;
```



(3)修改数据库

语法格式：

```sql
ALTER DATABASE [数据库名] {[ DEFAULT ] CHARACTER SET <字符集名> ｜[ DEFAULT ] COLLATE <校对规则名>}
```

语法说明：

- ALTER DATABASE用于更改数据库的全局特性。这些特性储存在数据库目录的db.opt文件中。
- 使用ALTER DATABASE需要获得数据库ALTER权限。
- 数据库名称可以忽略，此时语句对应于默认数据库。
- CHARACTER SET 子句用于更改默认的数据库字符集。



示例：

修改数据库database_name

```sql
alter database database_name rename to database_new_name;
```



(4)查看数据库

语法格式：

```sql
SHOW DATABASES [LIKE '数据库名']；
```



语法说明：

- LIKE从句是可选项，用于匹配指定的数据库名称。LIKE从句可以部分匹配，也可以完成匹配。
- 数据库名由单引号''包围。



示例：

查看所有数据库

```sql
show databases;
```



（5）使用数据库

语法格式：

```sql
USE <数据库名>
```

示例：

使用数据库database_name

```sql
use database_name;
```



#### 2.操作数据表

（1）创建数据表

语法格式：

```sql
CREATE TABLE <表名> ([表定义选项])[表选项][分区选项]；
```

其中，[表定义选项]的格式是：<列名1> <类型1>[,...]<列名n><类型n>

语法说明：

- CREATE TABLE：用于创建给定名称的表，必须拥有表CREATE的权限。
- <表名>：指定要创建表的名称，在 CREATE TABLE 之后给出，必须符合标识符命名规则。表名称被指定为databaseb_name.table_name，以便在特定的数据库中创建表。无论是否有当前数据库，都可以通过这种方式创建。在当前数据库中创建表时，可以省略db-name。如果使用加引号的识别名，则应对数据库和表名称分别加引号。例如，'mydatabaseb'.'mytable'是合法的，但'mydatabaseb.mytable'不合法。
- <表定义选项>：表创建定义，由列名（column_name）、列的定义（column_definition）以及可能的空值说明、完整性约束或表索引组成。

- 默认情况是，表被创建到当前数据库中。若表已存在、没有当前数据库或者数据库不存在时，则会出现错误。



示例：

创建了表table_name，包含类型为int的id列

```sql
create table table_name(id int);
```



(2)修改数据表

语法格式：

```sql
ALTER TABLE <表名> [修改选项]；
```

其中，[修改选项]的格式是：

```sql
{ ADD COLUMN <列名> <类型> 
｜CHANGE COLUMN <列名> { SET DEFAULT <默认值>｜DROP DEFAULT }
｜ALTER COLUMN <列名> { SET DEFAULT <默认值>｜DROP DEFAULT }
｜MODIFY COLUMN <列名> <类型>
｜DROP COLUMN <列名>
｜RENAME TO <新表名> }
```



示例：
修改数据表table_name使其添加name列

```sql
alter table table_name add name varchar (30);
```



(3)删除数据表

语法格式：

```sql
DROP TABLE [ IF EXISTS ] <表名>[,<表名1>,<表名2>] ...
```

语法说明：

- <表名>：被删除的表名。DROP TABLE 语句可以同时删除多个表，用户必须拥有该命令的权限。
- 表被删除时，所有的表数据和表定义会被取消，所以使用本语句要小心。
- 表被删除时，用户在该表上的权限并不会自动被删除。
- 参数IF EXISTS用于在删除前判断删除的表是否存在，加上该参数后，在删除表的时候，如果表不存在，SQL语句可以顺利执行，但会发出警告（warning）。



示例：

删除数据表table_name

```sql
drop table table_name;
```



#### 3.操作数据

（1）插入数据

语法格式：

```sql
INSERT INTO  <表名> [ <列名1> [,...<列名n>] ] VALUES (值1) [...,(值n)];
```

语法说明：

- <表名>：指定被操作的表名。
- <列名>：指定需要插入数据的列名。若向表中的所有列插入数据，则全部的列名均可以省略，直接采用 INSERT <表名> VALUES (...)即可。
- VALUES 或 VALUE 子句：该子句包含要插入的数据清单。数据清单中数据的顺序要和列的顺序相对应。



示例：

```sql
insert into table_name(id) values (1);
```



(2)删除数据

语法格式：

```sql
DELETE FROM <表名> [WHERE 子句][ORDER BY 子句][LIMIT 子句]
```

语法说明：

语法说明如下：

- <表名>：指定要删除数据的表名。
- ORDER BY 子句：可选项。表示删除时，表中各行将按照子句中指定的顺序进行删除。
- WHERE 子句：可选项。表示为删除操作限定删除条件，若省略该子句，则代表删除该表中的所有行。
- LIMIT 子句：可选项。用于告知在控制命令被返回数据前被删除行的最大值。



示例：

删除表table_name中全部数据

```sql
delete from  table_name;
```



(3)修改数据

```sql
UPDATE <表名> SET 字段 1=值 1[,字段 2=值 2...][WHERE 子句][ORDER BY 子句][LIMIT 子句]
```

语法说明：

- <表名>：用于指定要更新的表名称。

- SET 子句：用于指定表中要修改的列名及其列值。其中，每个指定的列值可以是表达式，也可以是该列对应的默认值。如果指定的是默认值，可用关键字DEFAULT表示列值。

- WHERE 子句：可选项。用于限定表中要修改的行。若不指定，则修改表中所有的行。
- ORDER BY 子句：可选项。用于限定表中的行被修改的次序。
- LIMIT 子句：可选项。用于限定被修改的行数。



示例：

更新所有行的id列为0

```sql
update table_name set id=0
```



(4)查询数据

语句格式：

```sql
SELECT {* | <字段列名>}

[

FROM <表 1>,<表 2>...

[WHERE <表达式>

[GROUP BY <group by definition>

[HAVING <expression> [{ <operator> <expression> }...]]

[ORDER BY <order by definition>]

[LIMIT[<offset>,]<row count>]

]
```



语法说明：

- {*|字段列名}包含星号通配符的字段列表，表示查询的字段，其中字段列至少包含一个字段名称，如果要查询多个字段，多个字段之间要用逗号隔开，到最后一个字段后不要加逗号。
- FORM <表 1>，<表 2>...，表1和表2表示查询数据的来源，可以单个或多个。
- WHERE 子句是可选项，如果选择该项，将限定查询行必须满足的查询条件。
- GROUP BY <字段>，该子句用于按照指定的字段分组。
- [ORDER BY<字段>]，该子句用于指定按照什么样的顺序显示查询出来的数据，可以进行的排序有升序（ASC）和降序（DESC）。
- [LIMIT[<offset>,]<row count>],该子句用于指定每次现实查询出来的数据条数。



示例：

查询表中全部记录

```sql
select * from table_name;
```



## SQL高级语法

#### 1.操作符

操作符是一个保留字和字符，用于指定条件或者联结多个条件。常见操作符有比较操作符、逻辑操作符、算数操作符。

（1）比较操作符

比较操作符是指等于=、不等于<>、大于>、小于<、大于等于>=、小于等于<=

示例：

id=1；id<>1；id>1；id<1；id>=1；id<=1；

（2）逻辑操作符

逻辑操作符包括与NULL值比较 IS NULL、位于两个值之间BETWEEN、与指定列表比较IN、与类似的值比较LIKE、多个条件与连接AND、多个条件或连接OR

示例：

```sql
id is null;
id between '0' and '10';
id in ('0','1','10');
id like '123%';
id > 10 and id < 20;
id = 10 or id = 20;
```

(2)算术操作符

算术操作符有加法+、减法-、乘法*、除法/，支持组合使用

示例：

where col1 + col2 > '20';

where col1 - col2 > '20';

where col1 * 10 > '20';

where (col1 / 10) > '20';



#### 2.连接

（1）内连接

语法格式：

```sql
SELECT <列名1，列名2 ...>
FROM <表名1> INNER JOIN <表名2> [ON子句]
```

语法说明：

- <列名1，列名2...>：需要检索的列名。
- <表名1><表名2>：进行内连接的两张表的表名。

示例：

```sql
select id,name from table1 inner join table2 on table1.cid=table2.cid;
```

(2) 全连接

语法格式：

```sql
SELECT <列名1，列名2 ...>

FROM <表名1>FULL JOIN <表名2>[ON子句]
```

语法说明：

- <列名1，列名2...>：需要检索的列名。
- <表名1><表名2>：进行全连接的两张表的表名。



示例：

```sql
select id,name from table1 full join table2 on table1.cid=table2.cid;
```



(3)左连接

语法格式：

```sql
SELECT <列名1，列名2 ...>
FROM <表名1>LEFT JOIN <表名2> [ON子句]
```

语法说明：

- <列名1，列名2 ...>：需要检索的列名。
- <表名1><表名2>：进行左连接的两张表的表名。



示例：

```sql
select id,name from table1 left join table2 on table1.cid=table2.cid;
```



(4)右连接

语法格式：

```sql
SELECT <列名1，列名2 ...>
FROM <表名1> RIGHT JOIN <表名2> [ON子句]
```

语法说明：

- <列名1，列名2...>：休要检索的列名。
- <表名1><表名2>：进行右连接的两张表的表名。



示例：

```sql
select id,name from table1 right join table2 on table1.cid=table2.cid;
```



### 3.视图

视图是一个虚拟表，包含一系列带有名称的列和行数据，但视图并不是数据库这是存储的数据表。存储在数据库中的查询操作SQL语句定义了视图的内容，列数据和行数据来自视图查询所引用的实际表，引用视图时动态生成这些数据。视图的结构形式和表一样，可以进行查询、修改、更新和删除等操作。

（1）创建视图

语法格式：

```sql
create view <视图名> as <select语句>
```

语法说明：

- <视图名>：指定视图的名称。该名称在数据库中必须是唯一的，不能与其他表或视图同名。
- <SELECT语句>：指定创建视图的SELECT语句，可用于查询多个基础表或源视图。



示例：

```sql
create view view_name as select * from table_name;
```



(2) 查看视图

语法格式：

DESCRIBE <视图名>；

语法说明：

- <视图名>：查看视图名称。该名称在数据库中必须是唯一的，不能与其他表或视图同名。



示例：

```sql
describe view_name;
```



(3) 修改视图

语法格式：

```sql
ALTER VIEW <视图名> AS <SELECT语句>
```

语法说明：

- <视图名>：指定视图的名称。该名称在数据库中必须是唯一的，不能与其他表或视图同名。
- <SELECT语句>：指定修改视图的SELECT语句，可用于查询多个基础表或源视图。



示例：

```sql
alter view viwe_name as select * from table_name;
```



(4) 删除视图

语法格式：

```sql
DROP VIEW <视图名1> [,<视图名2>...]
```

语法说明：

- 视图名：指定删除的视图名称。



示例：

```sql
drop view view_name;
```



### 4.索引

索引是一种十分重要的数据库对象。索引是数据库性能调优的基础，常用于实现数据的快速检索。对表建立一个索引，在列创建了索引之后，查找数据时可以直接根据该列上的索引找到对应记录行的位置，从而快捷地查找到数据。索引存储了指定列数据值的指针，根据指定的排序顺序对这些指针排序。



(1) 创建索引

语法格式：

```sql
CREATE <索引名> ON <表名> (<列名> [<长度>][ASC|DESC])
```

此外，还可以在CREATE TABLE、ALTER TABLE时创建索引

语法说明：

- <索引名>：指定索引名。一个表可以创建多个索引，但每个索引在该表中的名称是唯一的。
- <表名>：指定要创建索引的表名。
- <列名>：指定要创建索引的列名。通常可以考虑将查询语句中在JOIN子句和WHERE子句里经常出现的列作为索引列。
- <长度>：可选项。指定使用列前的length个字符来创建索引。使用列的一部分创建索引有利于减小索引文件的大小，节省索引所占的空间。在某些情况下，只能对列的前缀进行索引。索引列的长度有一个最大上限自字节数。
- ASC|DESC：可选项。ASC指定索引按照升序来排列，DESC指定索引按照降序来排列，默认为ASC。



示例：

```sql
create index index_name on table_name(column_name,column_name);
```

(2) 删除索引

语法格式：

```sql
DROP INDEX <index> ON <表名>
```

语法说明：

- <索引名>：要删除的索引名
- <表名>：指定该索引所在的表名。



示例：

```sql
drop index index_name on table_name;
```



### 5.事务

事务是并发控制的单位，是用户定义的一个操作序列，主要用于处理操作量大，复杂度高的数据。这些操作要么都做，要么都不做，是一个不可分割的工作单位。通过事务，SQL能将逻辑相关的一组操作绑定在一起，以便保持数据的完整性。

一般来说，事务是必须满足4个条件（ACID）：原子性（Atomicity，又称为不可分割性）、一致性（Consistency）、隔离性（Isolation，又称为独立性）、持久性（Durability）。

（1）开始事务

语法格式：

```sql
BEGIN TRANSACTION <事务名称>｜@<事务变量名称>；
```

语法说明：

- @<事务变量名称>是由用户定义的变量，必须用char、varchar、nchar或nvarchar数据类型来申明该变量。
- BEGIN TRANSACTION 语句的执行使全局变量@@TRANCOUNT的值加1.



示例：

Begin transaction



(2) 提交事务

语法格式：

```sql
ROLLBACK [TRANSACTION][<事务名称>|@<事务变量名称>｜<存储点名称>｜@<含有存储点名称的变量名>；
```

语法说明：

- 当条件回滚只影响事务的一部分时，事务不需要全部撤销已执行的操作。可以让事务回滚到指定位置，此时，需要在事务中设定保存点（SAVEPOINT）。保存点所在位置之前的事务语句不用回滚，即保存点之前的操作被视为有效的。保存点的创建通过“SAVING TRANSACTION<保存点名称>”语句来实现，再执行“ROLLBACK TRANSACTION<保存点名称>”语句回滚到该保存点。
- 若事务回滚到起点，则全局变量@@TRANCOUNT的值减1；若事务回滚到指定的保存点，则全局变量@@TRANCOUNT的值不变。



示例：

```sql
rollback
```



### 6.约束

（1）主键约束

主键约束是一个列或者列的组合，其值能唯一地标识表中的每一行，这样的一列或多列称为表的主键，通过主键约束可以强制表的实体完整性。



语法格式：

<字段名> <数据类型> PRIMARY KEY [默认值]



示例：

```sql
PRIMARY KEY(id)
```

(2) 唯一约束

唯一约束要求该列唯一，允许为空，但只能出现一个空值。唯一约束能够确保一列或者几列不出现重复值。

语法格式：

<字段名> <数据类型> UNIQUE



示例：

unique(id)

(3) 外键约束

外键约束用来在两个表的数据之间建立链接，它可以是一列或者多列。一个表可以有一个或多个外键。

语法格式：

```sql
[CONSTRAINT <外键名>]FOREIGN KEY 字段名[,字段名2,...]
REFERENCES <主表名> 主键列1[,主键列2,...]
```

语法说明：

外键名为定义的外键约束的名称，一个表中不能有相同名称的外键；字段名表示子表需要添加外键约束的字段列；主表名即辈子表外键所依赖的表的名称；主键列表示主表中定义的主键列或者列组合。



示例：

foreign key(id)



(4) 非空约束

非空约束（NOT NULL）可以通过CREATE TABLE 或 ALTER TABLE 语句实现。在表中某个列的定义后加上关键字 NOT NULL 作为限定词，来约束该列的取值不能为空。



语法格式：

<字段名> <数据类型> NOT NULL；

示例：

```sql
id int(10) not null
```



(5) 检查约束

检查约束基于行中其他列的值在特定的列中对值进行限制，用于限制列中的值的范围。

语法格式：

CHECK <表达式>

语法说明：

<表达式>指的就是SQL表达式，用于指定需要检查的限定条件。

示例：

```sql
check (id>0)
```



### 小结

SQL语言是结构化查询语言的简称，其具有功能丰富、语言简洁、灵活易学的优点。从上面的SQL语言介绍中我们可以看出SQL语法接近英语口语，我们很容易学习和上手，希望本篇文章可以帮助到想要学习SQL语言的数据产品经理们，让数据产品经理日常工作中数据的获取更加简单、方便，从而再次基础上能够作出更好的数据产品。

