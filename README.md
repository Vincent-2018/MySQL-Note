数据库笔记
===

一、DDL：操作数据库和表
---

### 1. 操作数据库

#### 1.1 C（create）创建

##### 1.1.1 创建数据库

```mysql
create database 数据库名称;
```

##### 1.1.2 创建数据库判断是否存在（不存在则创建）

```mysql
create database if not exists 数据库名称;
```

##### 1.1.3 创建数据库设计字符集

```mysql
create database 数据库名称 character set 字符集格式;
```

##### 1.1.4 创建数据库判断是否存在（不存在则创建），并指定字符集

```mysql
create database if not exists 数据库名称 character set 字符集格式;
```



#### 2.1 R（Retrieve）查询

##### 2.1.1 查询所有数据库的名称

```mysql
show databases;
```

##### 2.1.2 查询某个数据库的字符集

```mysql
show create database 数据库名称;
```



#### 3.1 U（Update）修改

##### 3.1.1 修改数据库的字符集

```mysql
alter database 数据库名称 character set 字符格式;
```



#### 4.1 D（Delete）删除

##### 4.1.1 删除数据库

```mysql
drop database 数据库名称;
```

##### 4.1.2 删除数据库，判断是否存在

```mysql
drop database if exists 数据库名称;
```



#### 5.1 使用数据库：

##### 5.1.1 查询当前正在使用的数据库名称

```mysql
select database();
```

##### 5.1.2 使用数据库

```mysql
use 数据库名称;
```

---



### 2. 操作表

#### 2.1 C（create）创建

##### 2.1.1 创建表

```mysql
create table 表名称 (列名1 数据类型1…列名n 数据类型n);
```

> 最后一列不需要,

| 数据类型  | 备注                                                         |
| --------- | ------------------------------------------------------------ |
| int       | 整数类型                                                     |
| double    | 小数类型，需要指定小数有几位，小数点后有几位                 |
| date      | 日期类型，只包含年月日的日期，yyyy-MM-dd                     |
| datetime  | 日期类型，包含年月日时分秒，yyyy-MM-dd HH:mm:ss              |
| timestamp | 时间戳类型，包含年月日时分秒，yyyy-MM-dd HH:mm:ss<br />如果将来不给这个字段赋值，或赋值为null，则默认使用当前系统时间给这个字段赋值 |
| varchar   | 字符串类型,需要指定字符串长度                                |

```mysql
-- 练习
create table student(
id int,
name varchar(32),
age int,
score double(4,1),
birthday date,
insert_time timestamp);
```

##### 2.1.2 复制表

```mysql
create table 表名称like 被复制的表名称;
```



#### 2.2 R（Retrieve）查询

##### 2.2.1 查询某个数据库中所有的表名称

```mysql
show tables();
```

##### 2.2.2 查询某个表的字符集

```mysql
show create table 表名称;
```

##### 2.2.3 查询表结构

```mysql
desc 表名称; 
```



#### 2.3 U（Update）修改

##### 2.3.1 修改表名

```mysql
alter table 表名称 rename to 新的表名称;
```

##### 2.3.2 修改表的字符集

```mysql
alter table 表名称 character set 字符集格式;
```

##### 2.3.3 添加一列

```mysql
alter table 表名称 add 列名 数据类型;
```

##### 2.3.4 修改列的名称、类型

```mysql
alter table 表名称 change 旧列名 新列名 新数据类型;
```

```mysql
alter table 表名称 modify 旧列名 新数据类型;
```

##### 2.3.5 删除列

```
alter table 表名称 drop 列名;
```



#### 2.4 D（Delete）删除

##### 2.4.1 删除某个表

```mysql
drop table 表名称;
```

##### 2.4.2 删除某个表，如果存在

```mysql
drop table if exists 表名称;
```

---



二、DML：增删改表中的数据
---

### 1. 添加数据

#### 1.1 添加一条记录

```mysql
insert into 表名(列名1,列名2,列名3,…列名n) values(值1,值2,值3…值n);
```

> ①、列名和值一一对应
>
> ②、如果表名后不定义列命名，则默认给所有列添加值
>
> ③、除了数字类型，其他类型需要用引号（单、双引号皆可）

----



### 2. 删除数据

#### 2.1 删除一条记录

```mysql
delete from 表名称 [where 条件];
```

> 如果不加where条件，则删除表中所有数据



#### 2.2 删除所有记录

```mysql
delete from 表名称;
```

> 不推荐使用，有多少条记录就会执行多少次



#### 2.3 截断表（清空表所有数据，保留表结构）

```mysql
truncate table 表名称;
```

> 删除表，然后再创建一个一模一样的空表

---



### 3. 修改数据

#### 3.1 修改某行中某列的数据

```mysql
update 表名 set 列名1=值2，列名2=值2 …列名n=值n [where 条件]
```

> 如果不加where条件，则修改表中所有数据

---



三、DQL：查询表中的数据
---

### 1. 查询所有记录

```mysql
select * from 表名称;
```

---



### 2. 语法

```mysql
select 字段列表 from 表名列表 where 条件列表 group by 分组字段 having 分组之后的条件限定 order by 排序 limit 分页限定
```

---



### 3. 基础查询

#### 3.1 查询多个字段

```mysql
select 列名1,列名2…列名n from 表名称;
```



#### 3.2 去除重复项

```mysql
select distinct 列名1,列名2…列名n from 表名称;
```



#### 3.3 计算列（一般指进行四则运算）

```mysql
select 列名1,列名2,列名1+[-*/]列名2[-*/]…列名n from 表名称;
```

> 如果有null参与的运算，计算结果都为null
>
> 如果实在要将null相加，使用函数ifnull(表达式1,表达式2)
>
> 表达式1：哪个字段需要判断是否为null
>
> 表达式2：该字段为null后的替换值
>
> 如：select 列名1,列名2,ifnull(列名1,0)+列名2 from 表名称



#### 3.4 起别名

```mysql
select 列名1,列名2,列名1+列名2+…列名n AS 新列名 from 表名称;
```

> as：as也可以省略，使用空格+新列名

---



### 4. 条件查询

#### 4.1 where子句后跟条件

##### 4.1.1 运算符 >、<、>=、<=、=、<>

```mysql
select * from 表名称 where 列名1 >0;
```

```mysql
select * from 表名称 where 列名1<>0;
-- 此句sql等同于select * from 表名称 where 列名!=0;
```

##### 4.1.2 BETWEEN…AND

```mysql
select * from 表名称 where 列名1 between 0 and 10;
```

##### 4.1.3 IN（集合）

```mysql
select * from 表名称 where 列名1 in (10,20,30);
```

##### 4.1.4 LIKE（模糊查询）

> 占位符
>
> _：单个任意字符
>
> %：多个任意字符

```mysql
-- 查询马姓两个字的人
select * from student where name like '马_';
```

```mysql
-- 查询姓马的人
select * from student where name like '马%';
```

```mysql
-- 查询包含马的人
select * from student where name like '%马%';
```

##### 4.1.5 ISNULL / IS NOT NULL

```mysql
select * from 表名称 where 列名1 is null;
```

##### 4.1.6 and或&&

```mysql
select * from 表名称 where 列名1>=0 and 列名1<=10;
```

##### 4.1.7 or或||

```mysql
select * from 表名称 where 列名1=10 || 列名1=20 || 列名1=30;
```

##### 4.1.8 not或!

```mysql
-- 配合in或者is not null使用
```

---



### 5.  排序查询

#### 5.1 order by 子句

```mysql
oder by 排序字段1 排序方式1,排序字段2 排序方式2…
```

> 升序：ASC升序，默认的
>
> 降序：DESC降序

> 注意：如果有多个排序条件，则当前面的条件值一样时，才会判断第二条件

---



###  6. 聚合函数

> 将一列数据作为一个整体，进行纵向的计算

#### 6.1 count：计算个数

select count(列名称) from 表名称;

> 一般选择非空的列：主键
>
> count(*)只要有一列不为null，就为有效的计数



#### 6.2 max：计算最大值

```mysql
select max(列名称) from 表名称;
```



#### 6.3 min：计算最小值

```mysql
select min(列名称) from 表名称;
```



#### 6.4 sum：计算和

```mysql
select sum(列名称) from 表名称;
```



#### 6.5 avg：计算平均值

```mysql
select avg(列名称) from 表名称;
```



> 注意：聚合函数的计算会排除非空的记录
>
> 解决方案： 
>
> ①、字段中包含null的值，可以使用ifnull(列名称,0)
>
> ②、选择不包含非空的列进行计算

---



### 7. 分组查询

#### 7.1 group by 分组字段

> 注意：
>
> ①、分组之后查询的字段：分组字段、聚合函数
>
> ②、where 和 having的区别？
>
> where在分组之前进行限定，如果不满足这个条件，则不参与分组
>
> having是在分组之后进行限定，如果不满足结果，则不会被查询出来
>
> where后不可以跟聚合函数，having可进行聚合函数的判断。

---



### 8. 分页查询

#### 8.1 limit 开始的索引，每页查询的条数

> 公式：开始的索引=（当前的页面-1）*每页显示的条数
>
> *limit是一个mysql方言

---



### 9. 约束

> 概念：对表中的数据进行限定，保证数据的正确性、有效性和完整性

#### 9.1 主键约束：primary key

> 主键约束：primary key，非空且唯一，一张表只能有一个字段作为主键，主键就是表中记录的唯一标识

##### 9.1.1 创建表时添加主键约束

```mysql
create table 表名称 (列名称 数据类型 primary key);
```

##### 9.1.2 删除主键约束

```mysql
alter table 表名称 drop primary key;
```

##### 9.1.3 创建表完后，添加主键

```mysql
alter table 表名称 modify 列名称 数据类型 primary key;
```

##### 9.1.4 自动增长，某一列数值类型，用auto_increment可以完成值的自动增长

> 创建表的时候，添加主键约束，并且完成主键自增长

```mysql
create table 表名称 ( 列名称 数据类型 primary key auto_increment);
```

##### 9.1.5 修改自动增长

```mysql
alter table 表名称 modify 列名称 数据类型 auto_increment;
```

##### 9.1.6 删除自动增长

```mysql
alter table 表名称 modify 列名称 数据类型;
```



#### 9.2 非空约束：not null

> 非空约束：not null，值不能为空

##### 9.2.1 创建表时添加非空约束

```mysql
create table 表名称 (列名称1 数据类型 not null);
```

##### 9.2.2 删除非空约束

```mysql
alter table 表名称 modify 列名称 数据类型;
```

##### 9.2.3 创建表完后，添加非空约束

```mysql
alter table 表名称 modify 列名称 数据类型 not null;
```



#### 9.3 唯一约束：unique

> 唯一约束：unique，值不能重复（唯一约束限定的列的值可以有多个null）

##### 9.3.1 创建表时添加唯一约束

```mysql
create table 表名称 (列名称1 数据类型 unique);
```

##### 9.3.2 删除唯一约束

```mysql
alter table 表名称 drop index 列名称;
```

##### 9.3.3 创建表完后，添加唯一约束

```mysql
alter table 表名称 modify 列名称 数据类型 unique;
```



#### 9.4 外键约束：foreign key

> 外键约束：foreign key，让表与表之间产生关系，保证数据的正确性

##### 9.4.1 创建表时，添加外键

```mysql
create table 表名称(… constraint 外键名称 foreign key 外键列名称 references 主表名称(主表列名称));
```

> 示例

```mysql
create table department( id int primary key auto_increment, dep_name varchar(20), dep_location varchar(20));
create table employee( id int primary key auto_increment, name varchar(20), age int, dep_id int, constraint emp_dept_fk foreign key (dep_id) references department(id));

insert into department values (null,'研发部','广州'),(null,'销售部','深圳');
insert into employee (name,age,dep_id) values ('张三',20,1);
insert into employee (name,age,dep_id) values ('李四',21,1);
insert into employee (name,age,dep_id) values ('王五',20,1);
insert into employee (name,age,dep_id) values ('老王',20,2);
insert into employee (name,age,dep_id) values ('大王',22,2);
insert into employee (name,age,dep_id) values ('小王',18,2);
```

##### 9.4.2 创建表后，添加外键

```mysql
alter table 表名称add constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称);
```

##### 9.4.3 删除外键

```mysql
alter table 表名称 drop foreign key 外键名称;
```

##### 9.4.4 级联操作

```mysql
alter table 表名称add constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称) on update cascade [on delete cascade];
```

> 级联更新：on update cascade;
>
> 级联删除：on delete cascade;



#### 9.5 默认约束：default

> 该列数据的默认值

---



### 10. 多表之间的关系

#### 10.1 一对一

>  如：人和身份证的关系
>
>  分析：一个人只有一个身份证，一个身份证只能对应一个人
>
>  实现方式：可以在任意一方添加唯一外键指向另一方的主键，也可以合并成一张表。



#### 10.2 一对多（多对一）

> 如：部门和员工
>
> 分析：一个部门有多个员工，一个员工只能对应一个部门
>
> 实现方式：在多的地方建立外键，指向一的一方的主键即可



#### 10.3 多对多

> 如：学生和课程
>
> 分析：一个学生可以选择很多门课，一门课也可以被很多学生选择
>
> 实现方式（联合主键）：多对多关系实现需要借助第三张中间表，中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键。

```mysql
-- 实践途牛网旅游路线案例

-- 添加旅游路线分类表
create table tab_catagory( cid int primary key auto_increment, cname varchar(100) not null unique);

insert into tab_catagory (cname) values ('周边游'),('出境游'),('国内游'),('港澳游');

-- 添加旅游路线表
create table tab_route( rid int primary key auto_increment, rname varchar(100) not null unique, price double, rdate date, cid int, foreign key (cid) references tab_catagory(cid));

-- 添加用户表
create table tab_user(uid int primary key auto_increment, username varchar(100) unique not null, name varchar(100), birthday date, sex char(1) default '男', telephone varchar(11), email varchar(100));

-- 添加联合主键表
create table tab_favorite (rid int, date datetime, uid int, primary key(rid,uid), foreign key (rid) references tab_route(rid), foreign key (uid) references tab_user(uid));
```

---



### 11. 范式

> 概念：设计数据库时需要遵循的一些规范。要遵循后边的范式要求，必须遵循前边的所有范式要求设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。

> 目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。

#### 11.1 分类

##### 11.1.1 第一范式（1NF）

> 每一列都是不可分割的原子项数据项

##### 11.1.2 第二范式（2NF）

> 在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖） 

##### 11.1.3 第三范式（3NF）

> 在2NF基础上，任何非主属性不依赖于其他非主属性（在2NF基础上消除传递依赖）



#### 11.2 几个概念

##### 11.2.1 函数依赖：AB

> 如果通过A属性（或者属性组）的值，可以确定唯一B属性的值，则称B依赖于A。
>
> 例如：
>
> 学号->姓名
>
> （学号，课程名称）->分数

##### 11.2.2 完全函数依赖：AB

> 如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有的属性值。
>
> 例如：（学号，课程名称）->分数

##### 11.2.3 部分函数依赖：A->B

> 如果A是一个属性组，则B属性值得确定只需要依赖于A属性组某一些值即可。
>
> 例如：（学号，课程名称）->姓名

##### 11.2.4 传递函数依赖：A—>B，BC

> 如果通过A属性（属性组）的值，可以确定唯一B属性的值，在通过B属性（属性组）的值，则称C传递函数依赖于A
>
> 例如：学号->系名，系名->系主任

##### 11.2.5 码

> 如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性（属性值）为该表的码
>
> 例如：该表的码为（学号，课程名称）

##### 11.2.6 候选码

> 如果关系中的某一属性组的值能唯一地标识一个元组，则称该属性组为候选码

##### 11.2.7 主码

> 如果一个关系有多个候选码，则选定其中一个为主码

##### 11.2.8 主属性

> 包含在任一候选关键字中的属性

##### 11.2.9 非主属性

> 不包含在主码中的属性



---



### 12. 数据库的备份和还原

#### 12.1 命令行方式

##### 12.1.1 备份语法

> mysqldump -u用户名 -p密码 数据库名称 > 保存的路径

##### 12.1.2 还原语法

> I、登录数据库
>
> II、创建数据库
>
> III、使用数据库
>
> IV、执行文件  .source 文件路径
>
> I、图形化工具方式

---



### 13. 多表查询

> 概念：从多张表进行查询操作
>
> 语法：select 列名称 from 表名称 where 条件…

```mysql
-- 创建部门表
create table dept( id int primary key auto_increment, name varchar(20));

insert into dept (name) values ('开发部'),('市场部'),('财务部');
```

```mysql
-- 创建员工表
create table emp(id int primary key auto_increment, name varchar(10), gender char(1), salary double, join_date date, dept_id int, foreign key (dept_id) references dept(id));

insert into emp(name,gender,salary,join_date,dept_id) values ('孙悟空','男',7200,'2013-02-24',1);
insert into emp(name,gender,salary,join_date,dept_id) values ('猪八戒','男',3600,'2010-12-02',2);
insert into emp(name,gender,salary,join_date,dept_id) values ('唐僧','男',9000,'2008-08-08',2);
insert into emp(name,gender,salary,join_date,dept_id) values ('白骨精','女',5000,'2015-10-07',3);
insert into emp(name,gender,salary,join_date,dept_id) values ('蜘蛛精','女',4500,'2011-03-14',1);
```

> 笛卡尔积
>
> 有两个集合A,B,取这两个集合的所有组成情况
>
> 要完成多表查询，需要消除无用的查询



#### 13.1 多表查询的分类

##### 13.1 内连接查询

###### 13.1.1 隐式内连接

> 使用where消除无用的数据

```mysql
select t1.name,t1.gender,t2.name from emp t1,dept t2 where t1.dept_id = t2.id;
```

###### 13.1.2 显式内连接

```mysql
-- 语法
select 字段列表 from 表名 [inner] join 表名2 on 条件
```

```mysql
select * from emp inner join dept on emp.dept_id = dept.id;
```

```mysql
select * from emp join dept on emp.dept_id = dept.id;
```

> 内连接查询：
>
> ①、从哪些表中查询数据
>
> ②、条件是什么
>
> ③、查询哪些字段



##### 13.2 外连接查询 

###### 13.2.1 左外连接

```mysql
select 字段列表 from 表1 left [outer] join 表2 on 条件;
```

> 查询的是左表所有数据以及其交集部分  

###### 13.2.2 右外连接  

```mysql
select 字段列表 from 表1 right[outer] join 表2 on 条件; 
```

> 查询的是右表所有数据以及其交集部分

---



### 14. 子查询

```mysql
-- 查询工资最高的人
select * from emp where salary = (select max(salary) from emp);
```

#### 14.1 子查询的不同情况

##### 14.1.1 子查询的结果是单行单列的

> 子查询可以作为条件，使用运算符取判断。运算符：> >= < <= =

```mysql
-- 查询员工工资小于平均工资的人
select * from emp where salary < (select avg(salary) from emp);
```

##### 14.1.2 子查询的结果是多行单列的

> 子查询可以作为条件，使用运算符in来判断

```mysql
-- 查询'财务部'和'市场部'所有员工的信息（传统方式）。先查到部门id，然后再根据部门id查找员工信息
select id from dept where name = '财务部' or name ='市场部';
select * from emp where dept_id = 3 or dept_id = 2;
```

```mysql
-- 查询'财务部'和'市场部'所有员工的信息（子查询）
select * from emp where dept_id in (select id from dept where name = '财务部' or name ='市场部');
```

##### 14.1.3 子查询的结果是多行多列的

> 子查询可以作为一张虚拟表参与查询

```mysql
-- 查询员工入职日期是2011-11-11日之后的员工信息和部门信息

-- 子查询
select * from dept t1,(select * from emp where join_date > '2011-11-11') t2 where t1.id=t2.dept_id;

-- 普通内连接
select * from emp t1,dept t2 where t1.dept_id = t2.id and t1.join_date > '2011-11-11';
```

---



### 15. 事务

#### 15.1 事务的基本介绍

> 概念：如果一个包含多个步骤的业务操作，被事务管理，要么这些操作同时成功，要么同时失败。



#### 15.2 开启事务

```mysql
start transaction;
```



#### 15.3 回滚

```mysql
rollback;
```



#### 15.4 提交

```mysql
commit;
```



```mysql
-- 例子
create database db3;
create table account(id int primary key auto_increment, name varchar(10), balance double);

insert into account (name,balance) values ('张三',1000),('李四',1000));

start transaction;
update account set balance = balance -500 where name = '张三';
update account set balance = balance +500 where name = '李四';

commit; -- 确认所有数据操作完毕，提交事务
rollback; -- 当中途出现错误时，可以回滚事务
```

> 在mysql数据库中事务默认自动提交



#### 15.5 事务提交的两种方式

##### 15.5.1 自动提交

> mysql就是自动提交的，一条DML（增删改查）语句会自动提交一次事务

##### 15.5.2 手动提交

> 需要先开启事务，再提交

> oracle数据库默认是手动提交事务



#### 15.6 查看事务的默认提交方式

```mysql
-- 1代表自动提交，0代表手动提交
select @@autocommit;
```



#### 15.7 修改事务的默认提交方式

```mysql
set @@autocommit = 0;
```



#### 15.8 事务的四大特征

##### 15.8.1 原子性

> 是不可分割的最小操作单位，要么同时成功，要么同时失败

##### 15.8.2 持久性

> 当事务提交或回滚后，数据库会持久化的保存数据

##### 15.8.3 隔离性

> 多个事务之间，相互独立。

##### 15.8.4 一致性

> 事务操作前后，数据总量不变。



#### 15.9 事务的隔离级别

> 概念：多个事务是隔离的，相互独立的，但是如果多个事务操作同一批数据，则引发一些问题，设置不同的隔离级别就可以解决这些问题。

##### 15.9.1 存在问题

###### i. 脏读

> 一个事务，督导另一个事务中没有提交的数据

###### ii. 不可重复读（虚读）

> 在同一个十五中，两次读取的数据不一样

###### iii. 幻读

> 一个事务操作（DML）数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。

##### 15.9.2 隔离级别

###### i. read uncommitted：读未提交

> 产生的问题：脏读、不可重复读、幻读

###### i. read commited：读已提交（oracle）

> 产生的问题：不可重复读、幻读

###### iii. repeatable read：可重复读（mysql默认）

> 产生的问题：幻读

###### iv. serializable：串行化

> 可以解决所有问题

> 注意：隔离级别从小到大安全性越来越高，但是效率越来越低

##### 15.9.3 数据库查询隔离级别

```mysql
select @@tx_isolation;
```

##### 15.9.4 数据库设置隔离级别

```mysql
set global transaction isolation level 级别字符串;
```

```mysql
-- 例子
set global transaction isolation level read uncommitted;
```

---



四、DCL：控制用户
---

> 了解下即可，因为有DBA（数据库管理员）会给你新增一个用户自己去弄

### 1. 管理用户或角色

#### 1.1 添加用户

```mysql
create user '用户名' @ '主机名' identified by '密码';
```



#### 1.2 删除用户

```mysql
drop user '用户名' @ '主机名';
```



#### 1.3 修改用户密码

##### 1.3.1 方式一

```mysql
update user set password = password('新密码') where user = '用户名';
```

##### 1.3.2 方式二

```mysql
set password for '用户名' @ '主机名' =password('新密码')；
```

> mysql忘记root用户的密码？
>
> 1.cmd执行 ànet stop mysql停止mysql服务（需要管理员运行cmd）
>
> 2.使用无验证方式启动mysql服务：mysqld –skip-grant-tables
>
> 3.再打开新窗口，输入mysql，直接回车进去修改root密码即可
>
> 4.修改完成后，打开任务管理器，手动关掉mysqld的进程
>
> 5.cmd执行net start mysql服务
>
> 6.登录mysql



#### 1.4 查询用户

```mysql
use mysql;
select * from user;
```



> 通配符：%表示可以在任意主机使用用户登录数据库

---



### 2. 权限管理

#### 2.1 查询权限

```mysql
show grants for '用户名' @ '主机名';
```



#### 2.2 授予权限

```mysql
grant 权限列表 on 数据库.表名 to '用户名' @ '主机名';
```

```mysql
-- 例如：
grant select,delete,update on db1.stu to '李四' @ 'localhost';
```

```mysql
-- 授予所有权限，在任意数据库任意表上
grant all on *.* '用户名' @ '主机名';
```



#### 2.3 撤销权限

```mysql
revoke 权限列表 on 数据库名.表名 from '用户名' @ '主机名';
```

