```sql
mysql [-h 主机] -u 用户 -p
create database d_name;
use d_name;
create table t_name(id int, name varchar(32), gender varchar(1));
insert into t_name(id,name,gender) values (1,'rose','m');
```

# SQL (Structure Query Language)
CURD-Create Update Retrieve Delete


* DDL数据定义语言：(Data Definition Language)
    * create
    * alter
    * drop
    * truncate
    * commit
    * grant
    * revote
* DML数据操纵语言:(Data Manipulation Language)  
    _DQL数据查询语言：(Data Qurry Language)(从属于DML)_
    * select
    * insert
    * update
    * delete
    * call
    * explain
* DCL数据控制语言：(Data Control Language)
    * commit
    * savepoint
    * rollback
    * set transaction

___


* DDL数据定义语言：(Data Definition Language)
    * 创建数据库
    * 修改数据库
    * 删除数据库  
    * 创建表
    * 修改表
    * 删除表
* DML数据操纵语言:(Data Manipulation Language)  
    _DQL数据查询语言：(Data Qurry Language)(从属于DML)_
    * 插入记录
    * 更新记录
    * 删除记录
    * 查询记录
      * 单表查询
      * 多表查询
* DCL数据控制语言：(Data Control Language)
    * commit
    * savepoint
    * rollback
    * set transaction

## DDL
### 创建数据库
* 语法
```sql
CREATE DATABASE [IF NOT EXISTS] db_name [create_specification [,create_specification]...]
create_specification:  
    [default]CHARACTER SET charset_name  
    [default]COLLATE collate_name  
```

* 查看默认字符集和校验规则
```sql
SHOW VARIABLES LIKE 'character_set_database';
SHOW VARIABLES LIKE 'collation_database';
```

* 查看数据库支持的字符集和校验规则
```sql
SHOW CHARSET;
SHOW COLLATION;
```
* 查看所有数据库
```sql
SHOW DATABASES;
```
* 查看数据库的创建语句
```sql
SHOW CREATE DATABASE db_name;
```
* 使用数据库
```sql
USE db_name;
```

### 修改数据库
```sql
ALTER DATABASE [IF EXISTS] db_name [alter_spacification [,alter_spacification]...]
alter_spacification:
    [DEFAULT] CHARACTER SET charset_name
    [DEFAULT] COLLATE collation_name
```

### 删除数据库
```sql
DROP DATABASE [IF EXISTS] db_ name;
```

### 创建表
* 语法
```sql
CREATE TABLE table_name (
    field1 datatype,
    field2 datatype,
    field3 datatype
) character set 字符集 collate 校验规则 engine 存储引擎;
```
* 例子
```sql
CREATE TABLE users(
    id int,
    name varchar(20) comment 'user name',
    birthday date
)CHARACTER SET utf8 engine MyISAM;
```
* 查看表结构
```sql
DESC table_name;
```
### 修改表
* 增加列
```sql
ALTER TABLE users ADD address VARCHAR(50) COMMENT 'address' AFTER birthday;
```
* 修改列的类型
```sql
ALTER TABLE users MODIFY name VARCHAR(30);
```
* 修改列名
```sql
ALTER TABLE users CHANGE address zhuzhi VARCHAR(60);
```
* 删除列
```sql
ALTER TABLE users DROP zhuzhi;
```
* 修改表名
```sql
ALTER TABLE users RENAME TO user;
```
* 修改字符集
```sql
ALTER TABLE user CHARSET=utf8;
```

### 删除表
```sql
DROP TABLE user2;
```

## DML

### 插入记录
* 语法
```sql
INSERT INTO table_name [(colum[,colum...])] VALUES (value [,value...]);
```
* 例子
```sql
INSERT INTO user (id,name,birthday) VALUES (1,'rose','1998-10-17');
INSERT INTO user VALUES (2,'nike','2001-3-5');
INSERT INTO user VALUES (3,'jack','2002-12-3'),(4,'john','1999-4-2'),(5,'alice','1997-9-23');
```
当主键对应的值已经存在时会插入失败：此时可以选择处理：
>
  * 更新
    ```sql
    INSERT INTO table_name(colum) VALUES (value) ON DUPLICATE KEY UPDATE colum=new_value;
    INSERT INTO user VALUES (1,'susun','1997-2-9') ON DUPLICCATE KEY UPDATE name='susun',birthday='1997-2-9';
    ```
  * 替换
    ```sql
    REPLACE INTO table_name(colum) VALUES (value);
    REPLACE INTO user VALUES(1,'rose','1999-5-23');
    ```

### 更新记录
* 语法
```sql
UPDATE table_name SET colum=value [,colum=value...] [WHERE condition] [LIMIT n];
UPDATE user SET name='rain' WHERE name='john';
```
>
     update 语法可以用新值更新原有表中的各列值  
     set    子句指示要修改哪些列和要给予哪些值  
     where  子句指定应更新哪些行。如果没有where子句，则更新所有行  
     limit  where子句后面指定limit,更新限制数量的符合条件的行  

### 删除记录
```sql
DELETE FROM table_name [WHERE condition];  
```
* 删除一条记录
```sql
DELETE FROM user WHERE id=6;
```
* 复制表结构，复制数据
```sql
CREATE TABLE user2 LIKE user;
INSERT INTO user2 SELECT * FROM user;
```
* 删除表中数据
```sql
DELETE FROM user2;      --删除表中数据，表结构还在，每行的操作记录都存入日志文件
TRUNCATE TABLE user2;   --直接清空表中数据，表结构还在
```
DELETE和TRUNCATE的区别:
|DELETE|TRUNCATE|
|:-:|:-:|
|对每行记录操作|对页操作|
|逐行删除数据|文件级清空数据|
|自增不重置|自增重置|
|清空表速度慢|清空表速度快|
|返回删除条数|返回0|

### 查询记录
```
+----+----------+---------+---------+------+
| id | name     | chinese | english | math |
+----+----------+---------+---------+------+
|  1 | 李涛     |      89 |      78 |   90 |
|  2 | 唐僧     |      67 |      98 |   56 |
|  3 | 孙悟空   |      87 |      78 |   77 |
|  4 | 老妖婆   |      88 |      98 |   90 |
|  5 | 红孩儿   |      82 |      84 |   67 |
|  6 | 如来佛祖 |      55 |      85 |   45 |
|  7 | 菩萨     |      75 |      65 |   30 |
+----+----------+---------+---------+------+
```
#### 单表查询 

* 使用DISTINCT去除重复
```sql
SELECT DISTINCT math from student;
```

* SELECT语句中对查询的列进行运算，使用as起别名
```sql
SELECT name,chinese + english + math + 10 as total from student;
+----------+-------+
| name     | total |
+----------+-------+
| 李涛     |   267 |
| 唐僧     |   231 |
| ..       |   ..  |
+----------+-------+
```
* SELECT的WHERE语句  
    + 比较运算符  
    <  <=  >  >=  =  <>  
    BETWEEN ... AND ...  
    IN()  
    LIKE ''  
    NOT LIKE ''  
    IS NULL 
    + 逻辑运算符  
    AND  OR  NOT

```sql
SELECT name FROM student WHERE name LIKE '李%';  -- % 匹配0~n个字符   _ 匹配1个字符
SELECT name FROM student WHERE math > 80;
SELECT name,(chinese+english+math) as total FROM student WHERE (chinese+english+math) > 200 AND math < chinese AND name LIKE '唐%';
SELECT name, english FROM student WHERE english BETWEEN 80 AND 90;
```
* 删除表中的重复数据，重复数据只保存一份
    * 创建一个空表  
        `CREATE TABLE tmp_tt LIKE tt;` 
    * 将数据导入到temp_tt中  
        `INSERT INTO tmp_tt SELECT DISTINCT * FORM tt;`
    * 删除tt表  
        `DROP TABLE tt;`
    * 将temp_tt表改名字  
        `ALTER TABLE tmp_tt RENAME tt;`

* SELECT语句的order by语句  
```sql
    SELECT * FROM user ORDER BY math desc; -- asc 升序（默认）  desc降序
    SELECT (chinese+math+english) AS total FROM user ORDER BY total > 200; --order by可以使用别名
```

* limit分页
```sql
select 字段 from 表名 where 条件 limit 起始位置 ，记录条数
select 字段 from 表名 where 条件 limit 记录条数 offset 起始位置
```

* 聚合函数
```sql
SELECT COUNT(id) FROM student WHERE math > 60; -- count(*)统计所有记录条数，count(colum)排除null
SELECT SUM(chinese+math+english) FROM student; --sum()仅对数值起作用
SELECT AVG(chinese+math+english) FROM student; --avg()返回满足where条件的一列的平均值
SELECT MAX(chinese+math+english) FROM student WHERE math > 60; --max()/min()返回满足where条件的最值
```
--------------------------------------------
**oracle 9i经典测试表**
```sql
mysql> select * from dept;
+--------+------------+----------+
| deptno | dname      | loc      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+

mysql> select * from emp;
+--------+--------+-----------+------+---------------------+---------+---------+--------+
| empno  | ename  | job       | mgr  | hiredate            | sal     | comm    | deptno |
+--------+--------+-----------+------+---------------------+---------+---------+--------+
| 007369 | SMITH  | CLERK     | 7902 | 1980-12-17 00:00:00 |  800.00 |    NULL |     20 |
| 007499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 00:00:00 | 1600.00 |  300.00 |     30 |
| 007521 | WARD   | SALESMAN  | 7698 | 1981-02-22 00:00:00 | 1250.00 |  500.00 |     30 |
| 007566 | JONES  | MANAGER   | 7839 | 1981-04-02 00:00:00 | 2975.00 |    NULL |     20 |
| 007654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 00:00:00 | 1250.00 | 1400.00 |     30 |
| 007698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 00:00:00 | 2850.00 |    NULL |     30 |
| 007782 | CLARK  | MANAGER   | 7839 | 1981-06-09 00:00:00 | 2450.00 |    NULL |     10 |
| 007788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 00:00:00 | 3000.00 |    NULL |     20 |
| 007839 | KING   | PRESIDENT | NULL | 1981-11-17 00:00:00 | 5000.00 |    NULL |     10 |
| 007844 | TURNER | SALESMAN  | 7698 | 1981-09-08 00:00:00 | 1500.00 |    0.00 |     30 |
| 007876 | ADAMS  | CLERK     | 7788 | 1987-05-23 00:00:00 | 1100.00 |    NULL |     20 |
| 007900 | JAMES  | CLERK     | 7698 | 1981-12-03 00:00:00 |  950.00 |    NULL |     30 |
| 007902 | FORD   | ANALYST   | 7566 | 1981-12-03 00:00:00 | 3000.00 |    NULL |     20 |
| 007934 | MILLER | CLERK     | 7782 | 1982-01-23 00:00:00 | 1300.00 |    NULL |     10 |
+--------+--------+-----------+------+---------------------+---------+---------+--------+

mysql> select * from salgrade;
+-------+-------+-------+
| grade | losal | hisal |
+-------+-------+-------+
|     1 |   700 |  1200 |
|     2 |  1201 |  1400 |
|     3 |  1401 |  2000 |
|     4 |  2001 |  3000 |
|     5 |  3001 |  9999 |
+-------+-------+-------+
```
--------------------------------------------

* GROUP BY 语句和HAVING
```sql
SELECT AVG(sal) as myavg FROM emp GROUP BY deptno HAVING myavg <2000;  
SELECT AVG(sal),MIN(sal),deptno,job FROM emp GROUP BY deptno,job; --先按deptno分再按job分
```
* 练习
```sql
* 查询工资高于500或岗位为MANAGER的雇员，同时还要满足他们的姓名首字母为大写的J
select ename,job,sal from emp where (sal>500 or job = 'MANAGER') and ename like 'J%';

* 按照部门号升序而雇员的工资降序排序
select deptno,sal from emp order by deptno,sal desc;

* 使用年薪进行排序
select ename,sal*12+ifnull(comm,0) as nianxin from emp order by nianxin desc;

* 显示工资最高的员工的名字和工作岗位
select ename,job from emp where sal = (select max(sal) from emp);

* 显示工资高于平均工资的员工信息
select empno,ename,sal from emp where sal > (select avg(sal) from emp); 

* 显示每个部门的平均工资和最高工资
select deptno,avg(sal),max(sal) from emp group by deptno;

* 显示平均工资低于2000的部门号和它的平均工资
select deptno,avg(sal) as salavg from emp group by deptno having 2000 > salavg; 

* 显示每种岗位的雇员总数，平均工资
select job,count(*) as zs,avg(sal) from emp group by job;

```

#### 多表查询
* 自连接（指在同一张表连接查询）
```sql
* 显示员工FORD的上级领导的编号和姓名
    * 使用 子查询/嵌套查询 的方式
    select empno,ename from emp where emp.empno = (select mgr from emp where ename='FORD');
    * 使用自查询的方式(给表起别名，然后用表的别名)
    select leader.empno,leader.ename from emp leader,emp worker where leader.empno=worker.mgr and worker.ename='FORD';
```

* 子查询(嵌套查询)    
子查询是指嵌入在其他sql语句中的select语句，也叫嵌套查询

    * 单行子查询  
    子查询语句只返回一条结果
    ```sql
    * 显示SMITH同一部门的员工
    select * from emp where deptno=(select deptno from emp where ename='SMITH');
    ```

    * 多行子查询  
    子查询语句返回多条结果
    ```sql
    * in关键字；查询和10号部门的工作相同的雇员的名字，岗位，工资，部门号，但是不包含10自己的
    select ename,job,sal,deptno from emp where job in(select distinct job from emp where deptno=10) and deptno <> 10;
    * all关键字；显示工资比部门30的所有员工的工资高的员工的姓名、工资和部门号
    select ename,sal,deptno from emp where sal > all(select sal from emp where deptno=30);
    select ename,sal,deptno from emp where sal > (select max(sal) from emp where deptno=30);
    * any关键字；显示工资比部门30的任意员工的工资高的员工的姓名、工资和部门号
    select ename,sal,deptno from emp where sal > any(select sal from emp where deptno=30);
    select ename,sal,deptno from emp where sal > (select min(sal) from emp where deptno=30);

    in:只要在子查询返回结果的范围中都可以
    all:所有
    any:任意
    max()和min() 可以替换 all和any
    ```

    * 多列子查询  
    子查询语句返回多个列数据
    ```sql
    * 查询和SMITH的部门和岗位完全相同的所有雇员，不含SMITH本人
    select ename,deptno,job from emp where (deptno,job) = (select deptno,job from emp where ename='SMITH') and ename <> 'SMITH';
    ```

    * 在from子句中使用子查询  
    子查询语句出现在from子句中。这里要用到数据查询的技巧，把一个子查询当做一个临时表使用
    ```sql
    * 显示高于自己部门平均工资的员工的信息
    select ename,sal,asal from emp,(select deptno dt,avg(sal) as asal from emp group by deptno) avgt where avgt.dt = deptno and sal > avgt.asal;

    * 查找每个部门工资最高的人的详细资料
    select ename,sal,deptno from emp,(select deptno de,max(sal) msal from emp group by deptno) tmp where deptno=tmp.de and sal=tmp.msal;

    * 显示每个部门的信息（部门名，编号，地址）和人员数量。
        * 使用子查询 
        select dname,deptno,loc,cou_hum from dept,(select deptno dt,count(ename) cou_hum from emp group by deptno) tmp where tmp.dt=dept.deptno;
        * 使用多表
        select dname,dept.deptno,loc,count(ename) from dept,emp where dept.deptno=emp.deptno group by emp.deptno;
    ```
    * 合并查询  
    在实际应用中，为了合并多个select的执行结果，可以使用集合操作符 union，union all  
      * union       取得两个结果集的并集，自动去掉结果集中的重复行
      * union all   取得两个结果集的并集，不会去掉结果集中的重复行
      * 第二个select语句查询的列的个数要与前一个select语句保持一致（个数保持一致就行）
    ```sql
        * 将工资大于2500和职位是MANAGER的人找出来
        select ename,sal,job from emp where sal>2500 union 
        select ename,sal,job from emp where job='MANAGER'; 
        * 将工资大于2500和职位是MANAGER的人找出来
        select ename,sal,job from emp where sal>2500 union all 
        select ename,sal,job from emp where job='MANAGER';

    ```

* 内连接  
内连接实际上就是利用where子句对两种表形成的笛卡儿积进行筛选

    ```sql
    * 语法
    select 字段 from 表1 inner join 表2 on 连接条件 and 其他条件； 
    select 字段 from 表1,表2 where 连接条件 and 其他条件；

    * 显示SMITH的名字和部门名称
    select ename,dname from emp inner join dept on emp.deptno=dept.deptno and emp.ename='SMITH';
    select ename,dname from emp,dept where emp.deptno=dept.deptno and emp.ename='SMITH';
    ```

* 外连接  
    * 左外连接
    ```sql
    * 查询所有学生的成绩，如果这个学生没有成绩，也要将学生的个人信息显示出来
    select * from stu left join exam on stu.id=exam.id;  --kity和nono在表2中不存在
    +------+------+------+-------+
    | id   | name | id   | grade |
    +------+------+------+-------+
    |    1 | jack |    1 |    56 |
    |    2 | tom  |    2 |    76 |
    |    3 | kity | NULL |  NULL |
    |    4 | nono | NULL |  NULL |
    +------+------+------+-------+
    ```
    * 右外连接
    ```sql
    * 对stu表和exam表联合查询，把所有的成绩都显示出来，即使这个成绩没有学生与它对应，也要显示出来。
    select * from stu right join exam on stu.id=exam.id;
    +------+------+------+-------+
    | id   | name | id   | grade |
    +------+------+------+-------+
    |    1 | jack |    1 |    56 |
    |    2 | tom  |    2 |    76 |
    | NULL | NULL |   11 |     8 |
    +------+------+------+-------+
    ```
    ```
    mysql> select * from stu;
    +------+------+
    | id   | name |
    +------+------+
    |    1 | jack |
    |    2 | tom  |
    |    3 | kity |
    |    4 | nono |
    +------+------+

    mysql> select * from exam;
    +------+-------+
    | id   | grade |
    +------+-------+
    |    1 |    56 |
    |    2 |    76 |
    |   11 |     8 |
    +------+-------+
    ```

  * 练习  
  ```sql
  * 列出部门名称和这些部门的员工信息，同时列出没有员工的部门
  select dept.deptno,dname,ename from dept left join emp on dept.deptno=emp.deptno; 
  ```
-------------------------
-----------------------------------------------
一。索引：提高海量数据查询速度
select * from emp where empno = ??; //会一行一行对比查
添加索引后，将表中数据建成二叉树形式，使得查找次数大幅度降低（查找条件要用到了索引）
主键索引，唯一索引，普通索引，全文索引


a.主键索引（字段不能重复）
一张表有且只有一个主键，主键索引效率做高
一般int列作为主键（逻辑主键）
一般和自增（auto_increment）

b.唯一索引（unique）（字段不能重复）
唯一索引表中可以有多个，查询效率仅次于主键索引，允许为null，并且null不作为判断

c.普通索引（index）（允许字段重复）（实际开发用的多）
表中允许多个字段添加普通索引，普通索引允许字段重复，查询效率低于唯一索引
alter table 表名 add index(列名);

d.全文索引：针对txt类型，大文本搜索--FULLTEXT(列名)
在5.6以前只有MyISAM存储引擎支持全文索引，5.6以后InnoDB引擎也支持全文索引，默认只支持英文

select * from articles
	where body like '%database%';
查看语句的索引等的使用情况：
(explain select * from articles//explain可以查看你的MySQL语句用到了哪些信息
	where body like '%database%';)
使用全文索引;
select * from 表名
	where match (全文索引列名) against(要查询的数据)


创建主键索引：
create table user1(id int primary key, name varchar(30)); 
create table user2(id int, name varchar(30), primary key(id));
create table user3(id int, name varchar(30)); alter table user3 add primary key(id);
创建唯一索引：
create table user4(id int primary key, name varchar(30) unique); 
create table user5(id int primary key, name varchar(30),unique(name));
create table user6(id int primary key, name varchar(30)）； alter table user6 add unique(name);
创建普通索引：
create table user8(id int primary key,name varchar(20),email varchar(30), index(name);
create table user9(id int primary key, name varchar(20), email varchar(30)); alter table user9 add index(name);
create table user10(id int primary key, name varchar(20), email varchar(30)); create index idx_name on user10(name);
创建全文索引：
CREATE TABLE articles (
	id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
	title VARCHAR(200),
	body TEXT,
	FULLTEXT (title,body)
)engine=MyISAM;

创建索引的原则：
1.比较频繁作为查询条件的字段应该创建索引
2.唯一性太差的字段不适合单独创建索引，比如性别
3.更新非常频繁的字段不适合创建索引（频繁改动二叉树结构，时间开销大）
4.不会出现在where子句的字段不该创建索引


查询索引:
查看当前表中所有的索引信息
show keys from 表名；
show index from 表名；
desc 表名；


删除索引：
alter table 表名 drop primary;
alter table 表名 drop index 索引名(删除单个索引，索引名就是key_name)
drop index on 表名（删除表中所有索引）





****************************  存储过程  ****************************
存储过程：
DELIMITER $$
CREATE PROCEDURE PROCTtest(p1 INT,IN p2 INT,OUT p3 INT,INOUT p4 INT)
BEGIN
SELECT p1,p2,p3,p4;
SET p1=10+p1;
SET p2=10+p2;
SET p3=10+p3;
SET p4=10+p4;
END$$
DELIMITER ;
注意：DELIMITER后面要有一个空格；IN/OUT/INOUT不指定的话，默认是IN；OUT/INOUT不能传常量，要定义变量再传进去（set @v3=3），看变量（select @v3;）

查看有哪些存储过程：
select name from mysql.proc where db='bbb' and 'type'='procedure';
查看存储过程的信息：
show create procedure PROCTtest;
删除存储过程：
drop procedure PROCTtest;

函数和存储过程的区别：存储过程（C） 函数（H）
C用于实现复杂的过程比如插入数据，H用于实现针对性的具体是逻辑比如根据分类名称查找分类ID；
H只能用IN类型，而C的话IN/OUT/INOUT三种都可以；
H只能返回一个变量，而C可以返回多个；
H要有返回结果，要有return，C不能用return而是用参数来返回结果；
H通常在函数语句中调用，存储过程通常是作为一个独立的部分来执行；
H保证数据库的安全，影响数据库的性能。

DELIMITER $$
CREATE PROCEDURE PROC4()
begin
declare i int;
set i=5;
while i<10000 do
insert into stu values(i,concat('index',i));
set i=i+1;
end while;
end;
$$
DELIMITER ;


事务：（ACID）（必考）       （转账，减钱和加钱，必须同时成功或失败）
事务是由一组sql语句组成，这一组语句存在相关性，这一组语句要么全部成功，要么全部失败。
只有InnoDB支持事务。

1.开始一个事务
start transaction;

2.创建保存点
savepoint 保存点名；

3.事务回滚
rollback to 保存点名;
a.当回滚到先发生的保存点时，无法再回到后发生的保存点
b.当事务提交后，无法回滚

4.事务提交
commit;

5.隔离级别
当有多个客户端开启各自事务同时操纵同一张表时，进行隔离操作。

*脏读：当一个事务正在访问数据，并且对数据做了修改，而事务还未提交时，另一个事务能访问到此数据，就叫脏读。(读了没提交的数据)
*不可重复读：当一个事务A重复读取表中数据时（此时未提交），由于另一个事务B修改表中数据并且提交事务，造成的事务A读取到不同的数据。
*幻读：当一个事务A重复读取表中数据时（此时未提交），由于另一个事务B增加或者删除数据并且提交事务，造成的事务A读取到不同的数据。
（不可重复读的重点是修改：同样的条件, 你读取过的数据,再次读取出来发现值不一样了 幻读的重点在于新增或者删除：同样的条件, 第1次和第2次读出来的记录数不一样）

查询MySQL事务隔离级别：
select @@tx_isolation;
设置事务的隔离级别：（不推荐）-针对当前用户；
set session transaction isolation level 四种级别；
mysql默认的隔离级别是可重复读,一般情况下不要修改
 



6.事务的ACID特性
A：原子性
事务是一个不可分割单位，事务中的操作要么全部发生，要么都不发生。
C：一致性
事务都是从一个一致性状态到另一个一致性状态。保持系统始终处于一致状态。
I:隔离性
当多个用户并发访问数据库时，每个用户的事务不会被其他事务操作的数据干扰
D：持久性
事务一旦提交，对数据库的修改就是永久的。



3。视图view  （简化查询手段），安全性考虑
视图是一个虚拟的表。视图内容由查询定义。
视图的数据变化会影响到基表，基表数据也会影响视图


创建视图语法：
create view 视图名
	as select 语句；

视图与数据表的区别：
1.数据表要占用磁盘空间，视图不需要。
2.视图不能添加索引。
3.视图可以简化查询



用户管理：
[data pro]

增删改查
查询！！！
事务概念，ACID，没有隔离级别发生的三大现象

---------------------------------------------------




* 练习
```sql
* 笛卡尔积
select * from emp,dept;

* 显示部门号为10的部门名，员工名和工资
select ename,dname,sal from dept,emp where dept.deptno=emp.deptno and emp.deptno=10; 
( 连接查询时，有 查询约束条件 和 表连接条件 )

* 显示各个员工的姓名，工资，及工资级别
select ename,sal,grade from emp,salgrade where sal>=losal and sal<=hisal;

```




```sql

```

```sql
涉及按什么什么分类时，要用group by,再有什么条件的话，加having

where子句
select ename,job from emp where sal = (select max(sal) from emp);
select empno,ename,sal from emp where sal > (select avg(sal) from emp); 
!select empno,ename,sal,avg(sal) as salavg from emp where sal > salavg; 
select ename,sal,asal from emp,(select deptno dt,avg(sal) as asal from emp group by deptno) avgt where avgt.dt = deptno and sal > avgt.asal;
select ename,sal,deptno from emp,(select deptno de,max(sal) msal from emp group by deptno) tmp where deptno=tmp.de and sal=tmp.msal;

别名
SELECT name,chinese + english + math + 10 as total from student;
select deptno,avg(sal) as salavg from emp group by deptno having 2000 > salavg; 
select ename,sal*12+ifnull(comm,0) as nianxin from emp order by nianxin desc;
select leader.empno,leader.ename from emp leader,emp worker where leader.empno=worker.mgr and worker.ename='FORD';
select emp.ename,emp.sal from (select deptno,avg(sal) as mysal from emp group by deptno) avgt,emp where avgt.deptno = emp.deptno and emp.sal > avgt.mysal;

```



```sql
定义顺序：
(1) SELECT (2)DISTINCT<select_list>
(3) FROM <left_table>
(4) <join_type> JOIN <right_table>
(5)         ON <join_condition>
(6) WHERE <where_condition>
(7) GROUP BY <group_by_list>
(8) WITH {CUBE|ROLLUP}
(9) HAVING <having_condition>
(10) ORDER BY <order_by_condition>
(11) LIMIT <limit_number>

执行顺序：
(8) SELECT (9)DISTINCT<select_list>
(1) FROM <left_table>
(3) <join_type> JOIN <right_table>
(2)         ON <join_condition>
(4) WHERE <where_condition>
(5) GROUP BY <group_by_list>
(6) WITH {CUBE|ROLLUP}
(7) HAVING <having_condition>
(10) ORDER BY <order_by_list>
(11) LIMIT <limit_number>
```
可以看到，一共有十一个步骤，最先执行的是FROM操作，最后执行的是LIMIT操作。每个操作都会产生一个虚拟表，该虚拟表作为一个处理的输入，看下执行顺序：
```md
(1) FROM        : 对FROM子句中的左表<left_table>和右表<right_table>执行笛卡儿积，产生虚拟表VT1;
(2) ON          : 对虚拟表VT1进行ON筛选，只有那些符合<join_condition>的行才被插入虚拟表VT2;
(3) JOIN        : 如果指定了OUTER JOIN(如LEFT OUTER JOIN、RIGHT OUTER JOIN)，那么保留表中未匹配的行作为外部行添加到虚拟表VT2，产生虚拟表VT3。如果FROM子句包含两个以上的表，则对上一个连接生成的结果表VT3和下一个表重复执行步骤1~步骤3，直到处理完所有的表;
(4) WHERE       : 对虚拟表VT3应用WHERE过滤条件，只有符合<where_condition>的记录才会被插入虚拟表VT4;
(5) GROUP By    : 根据GROUP BY子句中的列，对VT4中的记录进行分组操作，产生VT5;
(6) CUBE|ROllUP : 对VT5进行CUBE或ROLLUP操作，产生表VT6;
(7) HAVING      : 对虚拟表VT6应用HAVING过滤器，只有符合<having_condition>的记录才会被插入到VT7;
(8) SELECT      : 第二次执行SELECT操作，选择指定的列，插入到虚拟表VT8中;
(9) DISTINCT    : 去除重复，产生虚拟表VT9;
(10) ORDER BY   : 将虚拟表VT9中的记录按照<order_by_list>进行排序操作，产生虚拟表VT10;
(11) LIMIT      : 取出指定街行的记录，产生虚拟表VT11，并返回给查询用户
```








with 
yy as (select id, name, math-chinese as total from student),
tt as (select id, name, math+chinese total from student)
select tt.total-yy.total from yy,tt where yy.id = tt.id;	

