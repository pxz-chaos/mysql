
# 0、目录

[TOC]



# 1、mysql 没有权限的解决方案

没有权限的表现形式：

mysql开机以后

输入show databases只显示一个数据库：information_schema

怎么解决？

答：

1.关闭mysql服务器，cmd命令为net stop mysql

2.找到mysql的安装目录下的一个名为“my.ini”的初始化配置文件

3.在my.ini中【mysqld】的后面添加[skip](https://so.csdn.net/so/search?q=skip&spm=1001.2101.3001.7020)-grant-tables 并保存。   目的在于，跳过权限检测

4.重启mysql服务器，cmd命令为net start mysql

5.开机mysql

6.show databases，此时就会显示所有数据库

7.更改权限 mysql> grant all privileges on *.* to 'root'@'%' identified by 'root' with grant option;

8.flush privileges;

9.再重启mysql服务器即可

参考链接：https://blog.csdn.net/weixin_35819549/article/details/113892462

### 1.开关MySQL

#### 开机命令为



```mysql
mysql -h -p3306 -u root -p
enter password:0712
或者mysql -u root -p
```

### 2.退出命令为

```mysql
exit；或者ctrl+c
```

####  进入MySQL后可以输入一下命令

```mysql
show databases;              #打开库名的方法
use mysql;                   #打开mysql库
show tables;                #打开mysql库
use test;
show tables;

show tables from mysql;    #查看mysql库中的数据
show databases;           #查看自己所在哪个库
```

# 一、 操作数据库(CRUD)

	## 1.1. 创建数据库（Create,C）

```mysql
create database 数据库名;#第一种方式，默认utf8编码
create database if not exists 数据库名 ;#第二种方式，默认utf8编码，防止重名而创建失败

create database 数据库名 character set gbk；#创建指定的编码格式的数据库，这里是gbk编码
create database if not exists 数据库名 character set gbk;

```

##  1.2. 查询数据库(Retrieve,R)

```mysql
show databases;#查询所有数据库
show  create database 数据库名称;#查询具体某个数据库

```

## 1.3. 修改数据库(Update,U)

修改数据库的字符集

```mysql
alter database 数据库名 character set 字符集名称;
```

## 1.4. 删除数据库(Delete,D)



```mysql
drop database 库名;-- 删除数据库

drop database if exists 库名;-- 先判断数据库是否存在，存在就删除数据库
```



## 1.5. 使用数据库

Navicat快捷键：

1.打开一个新的查询窗口: Ctrl + Q

2.关闭当前窗口:Ctrl + W

3.运行当前窗口的SQL语句:Ctrl + R

4.运行选中的SQL语句:Ctrl + Shift + R

5.注释选中SQL语句:Ctrl +　/

6.取消注释SQL:Ctrl + Shift + /

7.选中当前行SQL:三击鼠标

8.复制一行内容到下一行:Ctrl + D

9.删除当前行:Ctrl + L

10.在表内容显示页面切换到表设计页面:Ctrl + D

11.在表设计页面，快速切换到表内容显示页面:Ctrl + O

12.打开mysql命令行窗口:F6

13.刷新:F5
————————————————
版权声明：本文为CSDN博主「土豆空心粉」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/b_ingrem/article/details/103304946

### 1.5查询当前正在使用的数据库名称

```mysql
select database();
```

### 1.5.2 使用数据库

```mysql
use 数据库;
```

# 二、操作数据库中的表(CRUD)



### 2.1.新建一个表(Create, C)

```mysql
create table 表名(
  列名1 数据类型1,
  列名2 数据类型2,
  ...
  列名n 数据类型n
);-- 最后一列不需要加逗号

/*数据类型：
	1.整数类型 int 
	2.小数类型 double
	3.日期类型：
	data 包含年月日 yyyy-MM-dd
    datatime 日期，包含年月日时分秒 yyyy-MM-dd HH:mm:ss
    timestamp 时间戳类型 包含年月日时分秒 yyyy-MM-dd HH:mm:ss
    如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值
	4.字符串类型 varchar 
	5.类型 blob 可以存放图片类型
	6.clob
	
*/


```

#### 2.1.1 复制表



```mysql
create table 表2 like 表1;-- 复制一个表，表2复制表1
```

例子：

```mysql
use test；
或者用其他库也可以
create table stuinfo(
id int,
name varchar(20));#创建一个名为stuinfo()的函数，里面的内容有int 类型的id，以及长度为20的字符串类型name
desc 表名;-- 查看具体某个表结构
show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              |
| db                        |
| event                     |
| func                      |
| general_log               |
| help_category             |
| help_keyword              |
| help_relation             |
| help_topic                |
| host                      |
| ndb_binlog_index          |
| plugin                    |
| proc                      |
| procs_priv                |
| proxies_priv              |
| servers                   |
| slow_log                  |
| stuinfo                   |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
| user                      |
+---------------------------+
#说明创建成功
```

### 2.2.查询表

```mysql
show  tables;-- 查询某个数据库中所有表的名称
desc 表名;-- 查询表结构
```

### 2.3.修改表

#### 2.3.1.修改表名

```mysql
alter table 表名 rename to 新的表名；
```

#### 2.3.2.修改表的字符集

```mysql
show create table 表名;-- 查看具体某个表的字符集
alter table 表名 character set 字符集名称;-- 修改表的字符集
```

#### 2.3.3.添加一列

```mysql
alter table 表名 add 列名 数据类型;
```

#### 2.3.4修改列名称 类型

```mysql
alter table 表名 change  列名 修改后的新列名 新列名数据类型;
例如
alter table student add gender varchar(10);-- 添加一列
alter table student  change gender sex varchar(10);-- 修改gender为sex   

alter table student  modify  sex varchar(20);-- 修改sex这列的数据类型

```

#### 2.3.5 删除列

```mysql
alter table 表名 drop 列名
```

### 2.4. 删除表

```mysql
drop table 表名;
drop table if exist 表名 ;-- 删除之前判断一下是否存在

```

### 2.5.操作表(增删改,DML)

#####  2.5 添加数据



注意事项：

1.列名和值要一一对应

2.如果表名，不定义列名，则默认给所有的列添加值

```mysql
insert into 表名(列名1,列名2，列名3，...，列名n) values(值1，值2，值3，...，值n)；
insert into 表名/*(列名1,列名2，列名3，...，列名n)*/ values(值1，值2，值3，...，值n)；-- 可行，但是不推荐，因为需要写全所有values
```

```mysql
insert into stuinfo(id,name) values(1,'john');#插入id为1名字为john
Query OK, 1 row affected (0.01 sec)
select*from stuinfo;
+------+------+
| id   | name |
+------+------+
|    1 | john |
+------+------+
1 row in set (0.00 sec)

```

##### 2.5.2 删除数据

语法：

```mysql
delete from 表名 [where 条件];

-- 删除表中所有的数据
delete from 表名 ;-- 方式一,不推荐使用，有多少条记录就会执行多少次删除操作，其实就是java中的每一棵中的数据都清空，因为数据库是B-树结构嘛

truncate table 表名;-- 方式二，推荐使用，效率高。该语句的逻辑是先删除表，然后再创建一张一模一样的表。其实就是直接断掉根节点的那根线
```

例如：

```mysql
delete from stuinfo where id=1;
select*from stuinfo;
+------+------+
| id   | name |
+------+------+
|    2 | rose |
+------+------+
```

##### 2.5.3 更改数据

语法 ：

```mysql
update 表名 set 列名1=值1，列名2=值2,...,列名n=值n【where 条件】;
```

注意事项：

​	如果不添加任何条件，则将表中的所有记录全部修改。慎用

```mysql
update stuinfo set name='lilei' where id=1;#将id为1的john改为lilei
select*from stuinfo;
+------+-------+
| id   | name  |
+------+-------+
|    1 | lilei |
|    2 | rose  |
+------+-------+
```

### 2.6. 查询表中的记录(DQL)

下转至章节四，因为这是最核心的部分。

```mysql
select * from 表名;
```



###  2.7.如何查看mysql的版本

```mysql
select version();
+-----------+
| version() |
+-----------+
| 5.5.62    |
+-----------+
# 或者先退出，输入dos命令
exit
mysql--version
```



# 三、mysql的语法规范

1.不区分大小写。但是建议关键字，表名、列表小写

2.每条命令用分号结尾

3.每条命令根据需要，可以进行缩进 或换行

4.注释：

单行注释：

#注释文字

-- 注释文字

多行注释：/*注释文字*/

# 四、查询单表中的记录(DQL)



完整的语法：

```mysql
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段
having
	分组之后的条件
order by
	排序
limit
	分页限定
	
```



###  4.1.进阶1：基础查询

1.查询单个字段

```mysql
select 字段 from 表名;
```

2.查询多个字段

```mysql
select 字段1,字段2,字段3,...from 表名;
```

3.查询所有字段

```mysql
select *from 表名;
```

4.去重查询

```mysql
select distinct 需要去重的字段 from 表名;
```

5.计算列

可以使用四则运算的值

```mysql
#需要打开库名
use 库名；
#进阶1：基础查询
/*语法：
select 查询的东西
from 表名；
特点：
1、查询列表可以是：表中的字段、常亮值、表达式、函数
2、查询的结果是一个虚拟的表格
*/
1.查询表中的单个字段

#比如说要查询员工表的姓名
select last_name from employees;
2.查询表中的多个字段

select last_name,salary,email from employees;

3.查询表中的所有字段
1.select * from from employees; #1和2等价的
2.select 
 * 
from
  employees ;

3.select 
  `job_id``job_title``min_salary``max_salary` 
from
  employees ;
#``这个叫着重符号，防止内置函数与所取名字冲突

4.查询常量值
select 100;
select 'john';

5.查询表达式
select 100*98;

6.查询函数
select version();

7.起别名（也就是取外号）
#select 名1 as 名2
/*
1、便于理解
2、如果要查询的字段有重名的情况，使用别名可以区分开来
*/
#方式一：使用as
select 100*98 as 结果
select last_name as 姓,first_name as 名 from employees;
#方式二：使用空格
select last_name 姓,first_name 名 from employees;
#案例：查询salary，显示结果为out put
select salary as 'out put' from employees;

8.去重
#案例：查询员工表中涉及到的所有部门编号
select department_id from employees;
#上面这段代码结果显示中有重复的内容
select distinct department_id from employees;
#这段代码就没有重复的内容了

9.+号的作用
/*
jave中的+号：
1.运算符，两个操作数都为数值型
2.连接符，只要有一个操作数为字符串
mysql中的+号仅仅只有运算符的功能
select 100+99;      两个操作数都为数值型，则做加法运算
select '123'+90;    其中一方为字符型，试图将字符型转换为数值型，如果
                    转化成功，则继续做加法运算
select 'john'+90;   只要其中一方为字符型，试图将字符型转换为数值型，如果转化失败，则将字符型数据转化为0
select null+10;     只要有一方为null，则结果为null
*/
#案例：查询员工名和姓连接成一个字段，并显示为 姓名
格式: select concat(str1,str2,...) as 名字
SELECT 
  CONCAT(last_name,'.',first_name) AS 姓名 
FROM
  employees ;
# 显示出表的employees的全部列，各个列之间用逗号隔开，列头显示为OUT_PUT
10.补充：ifnull函数
/*SELECT 
  IFNULL(`commission_pct`, 0) AS 奖金率,
  commission_pct
FROM
  employees;
  */
SELECT 
    CONCAT(
      `employee_id`,
      `first_name`,
      `last_name`,
      `email`,
      `phone_number`,
      `job_id`,
      `salary`,
      IFNULL(`commission_pct`,0),
      `manager_id`,
      `department_id`,
      `hiredate`
    ) AS `OUT_PUT` 
  FROM
    employees ;

```



```mysql

```



<div style="page-break-after:always"></div>

### 4.2.进阶2：条件查询

```mysql
/*
语法：
    select 
         查询列表
     from
         表名
      where
         筛选条件；

*/
```

分类：

```mysql
 #### 一、按条件表达式筛选
```

```mysql
条件运算符：> < = != <> <= >=
```

```mysql
 #### 二、按逻辑表达式筛选
```

```java
 逻辑运算符：&&（与）||(或) !（非） 
           and or not
           
           &&和and：两个条件为 true，结果为 true，反之 为 false
           ||或or：只要有一个条件为 true，结果为 true，反之为 false
           !或or：只如果两个连接的条件本身为 false，结果为 true，反之为 true
```

#### 4.2.1、模糊查询

```mysql
/*

​         like 
​         between and
​         in 
​         is null    
*/
```

一、按条件表达式筛选:

```mysql
#案例一：查询员工工资>12000的员工信息
select * from employees 
where 
salary>12000;
#案例二：查询部门编号不等于90号的员工名和部门编号
select last_name,department_id from employees 
where 
department_id<>90;  #用！=也可以
```

二、案逻辑表达式筛选：

```mysql
#案例1：查询工资在10000到20000之间的员工名、工资以及奖金
SELECT 
  `last_name`,`salary`,`commission_pct`
FROM
  employees 
 where
     10000<=`salary` and `salary`<=20000;
#案例二：查询部门编号不是在90到110之间，或者工资高于15000的员工信息
select * from employees
where 
not(department_id>=90 and department_id<=110) or salary >15000;
```

三、模糊查询：

```mysql
/*
 like 
 between and
 in 
 is null 
 */
```

#### 1.like

特点：和通配符搭配使用
通配符：% 代表任意多个字符包含零个字符
       _:任意单个字符
       %:任意多个字符

```mysql
#案例一：查询员工名中包含字符a的员工信息
select * from employees
where 
last_name like '%a%';# %说明任意一个字符 多个判断句时要用逻辑并列语句

#案例2：查询员工名中第三个字符为n，第五个字符为l的员工名和工资
select last_name,salary
from employees
where 
last_name like '__n_l%';

#案例3：查询员工名中第二个字符为_的员工名
select last_name,salary
from employees
where 
last_name like '_\_%';#其中\为转义字符,将_转化为\_，因为_为关键词
或者
last_name like '_$_%' escape '$';
```

#### 2.between and

 特点：1.提高语句的简洁度
            2.包含临界值
            3.两个临界值不能颠倒

```mysql
#案例1：查询员工编号在100到120之间的所有员工信息
select *from employees 
where 
employee_id between 100 and 120;
```

#### 3.in

特点：
    1.提高语句的简洁度
    2.in列表的值必须一致或兼容


```mysql
#案例1：查询员工的工种编号是IT_PROG、AD_VP、AD_PRES中的一个员工名和工种编号

SELECT 
  last_name,
  job_id 
FROM
  employees 
WHERE job_id = 'IT_PROG' 
  OR job_id = 'AD_VP' 
  OR job_id = 'AD_PRES' ; #以前的思维

SELECT 
  last_name,
  job_id 
FROM
  employees 
WHERE job_id IN ('IT_PROG', 'AD_VP', 'AD_PRES') ;#现在的思维
```

#### 4、is null

特点：

​      1.=、<>不能判断null值

​      is null或is not null可以判断null值

​    

```mysql
 #案例一：查询没有奖金的员工名和奖金率
 SELECT 
  `first_name`,
  `salary`,
  `commission_pct` 
FROM
  employees 
WHERE `commission_pct` IS NULL ;
或者安全等于<=>
WHERE `commission_pct` <=> NULL ;
#案例2：查询工资为12000的员工信息
WHERE `salary` <=> 12000 ;
```

#### 5.is null和<=>做比较

is null：仅仅可以判断null值，可读性较高，建议使用。

<=>    ：既可以判断null值，又可以判断普通的数值，可读性较低。



### 4.3.进阶3：排序查询

####  order by

引入：
```mysql
 select*from employees;
```
语法：
```mysql
  select 查询列表
  from 表
  【where 筛选条件】
  order by 排序列表 【asc|desc】#升降序ascend descend的缩写
```

特点：1、asc是升序，desc是降序，如果不写默认是升序

​            2、order by支持单个字段、多个字段、表达式、函数、别名排序

​            3、order by一般放在查询语句的最后面，limit子句除外

​              

```mysql
案例1：查询员工信息，要求工资从高到低排序
select *from employees order by salary desc;
案例2：查询部门编号大于等于90的员工信息，按入职时间的先后进行排序[添加一个筛选条件]
select *from employees
where department_id >=90
order by `hiredate` asc;
案例3：按年薪的高低显示员工的信息和年薪【按表达式排序】，也支持别名排序
select *,salary*12*(1+ifnull(commission_pct,0)) as 年薪 from employees
order by 年薪 desc;
案例4：按姓名的长度显示员工的姓名和工资【按函数排序】
SELECT length(last_name) as 字节长度,last_name,salary
FROM employees
ORDER BY LENGTH(last_name) DESC;
#结果显示如下：
          11  Mikkilineni     2700.00
          10  Philtanker      2200.00
          10  Colmenares      2500.00
          10  Livingston      8400.00
           9  Dellinger       3400.00
           9  Cambrault       7500.00
           9  Cambrault      11000.00
案例5：查询员工信息，先按员工工资升序，再按员工编号降序【按多个字段排序】。
select* from employees
order by salary asc,employee_id desc;

```

<div style="page-break-after:always"></div>

### 4.4 进阶4：常见函数的学习

概念：类似于Java语句中方法，将一组逻辑语句封装在方法体中，对外暴露方法名；

好处：1、隐藏了实现细节   2、提高代码的重用性 ；

调用：select 函数名（实参列表）【from表】；

特点：1、叫什么（函数名）；2、干什么（函数功能）

分类：

​         1、单行函数：如concat、length、ifnull等

​         2、分组函数：做统计使用，又称为统计函数、聚合函数、组函数；

##### 4.4.1、字符函数

###### 1.length()

功能：获取参数的字节个数，一个汉字占三个字节。

补充：查看字符集

```mysql
select length('张培新abc');
#答案是：12
show variables like '%char%';#查看字符集
答案是：
Variable_name             Value                                                    
/*------------------------  ---------------------------------------------------------
character_set_client      utf8                                                     
character_set_connection  utf8                                                     
character_set_database    utf8                                                     
character_set_filesystem  binary                                                   
character_set_results     utf8                                                     
character_set_server      utf8                                                     
character_set_system      utf8                                                     
character_sets_dir        F:\Program Files\MySQL\MySQL Server 5.5\share\charsets\  
*/
```

###### 2.concat()

功能：拼接字符串

```mysql
select concat(last_name,'_',first_name) from employees;
```

###### 3.upper()、lower()

功能：大小写

```mysql
#案例：将姓变大写，名变小写，然后拼接
select concat(upper(last_name),'_',lower(first_name)) as 姓名 from employees;
/**
姓名             
-------------------
K_ING_steven       
KOCHHAR_neena      
DE HAAN_lex        
HUNOLD_alexander   
ERNST_bruce        
AUSTIN_david /
```

###### 4.substr()

功能：截取字符

格式：

```mysql
1.substr(str,pos);

2.substr(str FROM pos);

3.substr(str,pos,len);

4.substr(strstr FROM pos for len);
```

注意：与java不同的是，他的索引是从1开始的，不是从零开始；

```mysql
#案例：截取“李莫愁爱上了陆展元“中的”陆展元“
select substr('李莫愁爱上了陆展元',7);
#案例：姓名中首字母大写，其他字符小写然后用下划线拼接，显示出来；
SELECT 
  CONCAT(
    UPPER(SUBSTR(last_name, 1, 1)),
    '_',
    LOWER(SUBSTR(last_name, 2))
  ) AS 姓名 
FROM
  employees ;

```

###### 5.instr()

功能：返回子串第一次出现的索引，如果找不到返回零

###### 6.trim([{BOTH|LEADING|TRAILING}[remstr] From] str)

功能：去前后空格；

```mysql
select trim('  张翠山  ') as out_put from employeee;
select trim('a' from 'aaa张翠山aaaa') as out_put;
```

###### 7.lpad(str，len,padstr)、rpad

格式：

功能：用指定的字符实现左、右填充指定长度

```mysql
SELECT LPAD('殷素素',10,'*') AS ou_put;
/*
ou_put            
------------------
*******殷素素 
*/
```

###### 8.replace() 替换

格式：replace(str,from_str,to_str)

```mysql
select replace('阿猫爱上了阿狗','阿狗','阿猪');
#阿猫爱上了阿猪  
```

##### 4.4.2：数学函数

###### 1.round()

格式：round(X)

​           round(X,D)

功能：四舍五入

例子：

```mysql
SELECT ROUND(1.78),ROUND(1.78,1);
```

###### 2.ceil(X)、floor()、truncate(X,D)、mod(n,m)

ceil 天花板 、floor地板、truncate 截断

ceil()的功能： 向上取整，返回大于等于该参数的最小整数。

floor()的功能：向下取整，返回小于等于该参数的最小整数。

truncate(X,D)的功能：取整

mod(n,m)的功能：取余数

mod的运算机制：n-n/m*m,被除数为负，值就为负，与除数的正负号无关

例子：

```mysql
select truncate(1.699999,1);#答案是：1.6
select mod(10,3);#10-10/3*3
与10%3的效果一样
```

##### 4.4.3：日期函数

###### 1.now()、curdate()、curtime()

now()的功能：返回当前系统日期加时间

curdate()的功能：返回当前系统日期，但是不包含时间

curtime()的功能：返回当前时间，不包含日期



###### 2.可获取指定的部分：年、月、日、小时、分钟、秒

year(date);

month(date);

monthname(date);

```mysql
select year(now());
#或者
select year('1998-1-1');
SELECT *,YEAR(hiredate) 入职年份 FROM employees;

```

###### 3.str_to_date(str,format)、date_format()

![image-20210630173001543](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20210630173001543.png)

str_to_date():将日期格式的字符转换成指定格式的年、月、日的日期，注意区分Y的大小写

str_to_date('9-13-1999',''%m-%d-%Y);

date_format():将日期转化为字符

date_format('2018/6/6','%Y年%m月%d日')

datediff(str1,str2):求取时间差函数，其结果为天数，其中str1为时间1，str2为时间2

```mysql
select str_to_date('9-13-1999',''%m-%d-%Y);#1999-09-13,注意区分Y的大小写
select date_format('2018/6/6','%Y年%m月%d日');
SELECT DATE_FORMAT(NOW(),'%Y年%m月%d日');

案例：查询有奖金员工名和入职日期（xx月/xx日 xx年）
SELECT last_name,DATE_FORMAT(hiredate,'%m月%d日  %Y年') 入职日期 FROM employees
WHERE commission_pct IS NOT NULL;
```

##### 4.4.4 其他函数

###### 1.version()、database()、user()

version():查看当前的版本号;

database():查看当前的所在库;

user():查看当前的用户;

##### 4.4.5、流程控制函数

###### 1.if(exp1,exp2,exp3)

if()函数的功能：if else 的效果

```mysql
SELECT IF(10>5,'大','小') AS out_put;
select last_name,commission_pct,if (commission_pct is null,'没奖金 哈哈','有奖金 牛逼') as 备注
from employees;#注意IF前有逗号
```

###### 2.case()

case()函数的功能一：switch case 的效果



```java
//Java中的写法
switch(变量或者表达式){

​           case  常量1：语句1;break;

​            .....

​            default:语句n;break

}

```

```mysql
#mysql中
case 要判断的变量或表达式
	when 常量1 then 要显示的值1或者是语句1;
	when 常量2 then 要显示的值2或者是语句2;
	....
	else 要显示的值n或者是语句n;
end
    
```

案例1：查询员工的工资，要求

​	部门号=30，显示的工资为1.1倍

​	部门号=40，显示的工资为1.2倍

​	部门号=50，显示的工资为1.3倍

​	其他部门，显示的工资为原工资

```mysql
SELECT last_name,department_id,salary,
CASE department_id
    WHEN 30 THEN salary*1.1
    WHEN 40 THEN salary*1.2
    WHEN 50 THEN salary*1.3
ELSE salary
END AS 新工资
FROM employees; 
```

case()函数的功能二：类似于Java中的 多重if

```java
//Java中的写法
if(条件1){语句1：}
else if(条件2)
		{语句2}
...
else{语句n;}

```

```mysql
#mysql中
case 
	when 条件1 then 要显示的值1或语句1
	when 条件2 then 要显示的值2或语句2
	...
	else 要显示的值n或语句n;
end
```

案例：查询员工的工资情况

如果工资>20000，显示A级别

如果工资>15000,显示B级别

如果工资>10000,显示C级别

否者，显示为D级别,并按工资高低降序

```mysql
SELECT last_name,salary,
CASE
	WHEN salary>20000 THEN 'A'
	WHEN salary>15000 THEN 'B'
	WHEN salary>10000 THEN 'C'
	ELSE 'D'
END
AS 工资评级
FROM employees
ORDER BY salary DESC;
```

#### 4.5 常见函数总结

###### 4.5 字符函数：

length()、concat()、substr()、instr()、trim()、upper()、lower()、lpad()、rpad()、replace()

###### 4.5.2 数学函数：

round()、ceil()、floor()、truncate()、mod()

###### 4.5.3 日期函数：

now()、curdate()、curtime()、year()、month()、hour()、day()、minute()、second()、str_to_date()、date_format()

###### 4.5.4 其他函数：

version()、database()、user()

###### 4.5.5 流程控制函数：

if()、case()

 ###### 4.5.6：分组函数(聚合函数)

功能：用作统计使用，又称为聚合函数或统计函数或组函数

分类：

sum 求和、avg 求平均值、 max、 min 、count计数

案例：

```mysql
#sum、avg、max 、min|、count简单使用
select sum(salary)from employees;
select avg(salary)from employees;
select max(salary)from employees;
select min(salary)from employees;
select count(salary)from employees;
或者
select sum(salary),round(avg(salary),2),max(salary),min(salary),count(salary) from employees;
```

上面五个函数支持哪些参数类型？

1.sum 、avg一般处理数值型；

2.max、min支持任何类型、count支持非空任何类型；

3.是否忽略null值；

以上分组函数均忽略了null值；

4.以上可以和distinct（去重）搭配使用、count用的较多；

###### 4.5.6 count函数的详细介绍

count(*):统计所有行

count(常量值)：相当于在表中加入了一列常量值，用于统计所有行

效率：

MYISAM存储引擎下，count(*)的效率最高

INNODB存储引擎下，count(*)和count(1)效率差不多

```mysql
select count(*) from employees;#统计employees中的所有行的个数
select count(1) from employees;
select count('张培新')from employees;#效果和上面的一样

```

6、和分组函数一同查询的字段有限制

和分组函数一同查询的字段要求是group by后的字段，其他都不行，没有意义

```mysql
select avg(salary),employee_id from employees; #无意义
```

<div style="page-break-after:always"></div>

### 4.5 进阶5：分组查询

 语法：

​		select 分组函数，列（要求该列一定要出现在group by的后面）

​		from 表

​		【where 筛选条件】

​		group by 分组列表

​		【order by 子句】

注意：查询列表必须特殊，要求是分组函数和group by 后出现的字段

特点：

#### 4.5.1 分组查询中的筛选条件分为两类

1. |  名称   |   数据源   | 关键字      | 位置          |
   | :---: | :-----: | -------- | ----------- |
   | 分组前筛选 |   原始表   | where 子句 | group by子句前 |
   | 分组后筛选 | 分组后的结果集 | having子句 | group by子句后 |

2. 分组函数做条件一定是放在having子句中

3. 能用分组前做条件优先选用，为了提高性能

4. group by子句支持的单个字段分组，多个字段分组（多个字段之间用逗号隔开没有顺序要求），表达式或函数（用的较少）

5. 也可以添加排序（排序放在整个分组查询的最后）

6. 两者的区别在于where在分组之前进行限定，如果不满足条件，则不参与分组。having在分组之后进行限定，如果不满足条件，则不会被查询出来。where后不可以跟分组函数（聚合函数），having可以进行聚合函数的判断。


```mysql
案例1：查询每个工种的最高工资
SELECT MAX(salary),job_id FROM employees GROUP BY job_id;


案例2：查询每个位置上的部门个数
SELECT  COUNT(*),location_id FROM departments GROUP BY location_id;#语法解读 的部门个数，对应语法为count(*)，查询每个位置 对应语法为 location_id,每个什么就GROUP BY什么


案例3：查询邮箱中包含a字符的，每个部门的平均工资
SELECT 
  AVG(salary),
  department_id
FROM
  employees 
WHERE email LIKE '%a%' #这个是筛选条件
GROUP BY department_id ;


案例4：查询有奖金的每个领导手下员工的最高工资
SELECT 
  MAX(salary),
  last_name,first_name
  manager_id 
FROM
  employees 
WHERE commission_pct IS NOT NULL 
GROUP BY manager_id ;
或者
SELECT 
  MAX(salary),
  CONCAT(last_name,',',first_name),
  manager_id 
FROM
  employees 
WHERE commission_pct IS NOT NULL 
GROUP BY manager_id ;


案例5：查询那个部门的员工个数大于2
#1.查询每个部门的员工个数
#2.根据1的结果进行筛选，查询哪个部门的员工个数>2 用having语句
SELECT COUNT(employee_id),department_id FROM employees GROUP BY department_id
HAVING COUNT(employee_id)>2 ;

以下为错误语法！
SELECT COUNT(employee_id),department_id FROM employees
 WHERE COUNT(employee_id)>2 
 GROUP BY department_id ;
 #因为employees表中没有COUNT(employee_id)字段


案例6：查询每个工种有奖金的员工，该类员工的最高工资大于12000的工种编号和最高工资
SELECT MAX(salary),job_id,commission_pct FROM employees
GROUP BY job_id 
HAVING commission_pct IS NOT NULL AND MAX(salary)>12000;

案例7：查询领导编号>102的每个领导手下的最低工资>5000的领导编号第哪一个，以及最低工资
/*
#1.查询每个领导手下的员工最低工资
select min(salary),manager_id
from employees
group by manager_id
#2.添加筛选条件一：领导编号>102
select min(salary),manager_id
from employees
group by manager_id
having manager_id>102
或者：
select min(salary),manager_id
from employees
where manager_id>102  #因为manager_id在employees表中
group by manager_id
#3.添加筛选条件2：最低工资>5000
having min(salary)>5000
*/
SELECT MIN(salary),manager_id,employee_id FROM employees GROUP BY manager_id
HAVING manager_id>102 AND MIN(salary)>5000;

```

#### 4.5.2 按表达式或函数分组

```mysql
案例：按员工姓名的长度分组，查询每一组的员工个数，筛选员工个数>5有哪些
SELECT 
  COUNT(*),
  last_name AS 姓名的长度 
FROM
  employees 
GROUP BY LENGTH(姓名的长度) 
HAVING COUNT(*) >5 ; #按 什么分组就GROUP BY 什么
```

#### 4.5.3 按多个字段分组

案例：查询每个部门每个工种的员工的平均工资

```mysql
SELECT AVG(salary),department_id,job_id
FROM employees
GROUP BY department_id,job_id;
```

 

添加排序

案例：查询每个部门每个工种的员工的平均工资，并且按照平均工资的高低显示出来

```mysql
SELECT AVG(salary),department_id,job_id
FROM employees
GROUP BY department_id,job_id
ORDER BY AVG(salary) DESC;
```

<div style="page-break-after:always"></div>

#### 4.6.4分页查询

```mysql
-- 语法：
select 列名 from 表名 limit 开始的索引,显示的条数;
```

例子：

```mysql
select * from stu limit 0,3;-- 第一页
select * from stu limit 3,3;-- 第二页
select * from stu limit 9,3;-- 第三页

-- 公式：开始的索引=（当前页码-1）*每页显示的条数
```

注意limit只在mysql语法中，其他sql语法不支持，比如oracal语法就是rownum

# 五、 连接查询（多表查询）（sql92、sql99）

```mysql
select * from 表1,表2

-- 笛卡尔积
-- 消除无用的数据
```

含义：又称为多表查询，当查询的字段来自于多个表时们就会用到连接查询

笛卡尔乘积现象：表1有m行，表2有n行，结果=m*n行

发生原因：没有添加有效的连接条件

如何避免：添加有效的连接条件

分类：

​		按年代分类：

​		sql92标准：仅仅支持内连接

​		sql99标准【推荐】：支持内连接+外连接（左外连接和右外连接）+交叉连接

​		按功能进行分类：

​				内连接：等值连接、非等值连接、自连接

​				外连接：左外连接、右外连接、全外连接

​				                                                                                                                                                         

## sql92标准

## 5、等值连接

特点：

​	1、多表连接的结果为多表连接的交集部分

​	2、n表连接，至少需要n-1个连接条件

​	3.、多表的顺序没有要求

​	4.一般为表取别名

​	5、可以搭配前面介绍的所有子句使用，比如排序、分组、筛选

案例1：查询女神名和对应的男神名

```mysql
SELECT name,boyName FROM boys,beauty
WHERE beauty.boyfriend_id=boys.id;
```

案例2：查询员工名和对应的部门名

```mysql
SELECT last_name,department_name
FROM employees,departments
WHERE employees.`department_id`=departments.`department_id`;#为连接条件
```

案例3：查询员工名、工种号、工种名

```mysql
select last_name,job_id,job_title from employees,jobs
where employees.job_id=jobs.job_id;
/*查询：select last_name,job_id,job_title from employees,jobs where employees.job_id=jobs.job_id LIMIT 0, 1000
Column 'job_id' in field list is ambiguous

执行耗时   : 0 sec
传送时间   : 0 sec

*/
原因是：job_id存在着歧义，因为job_id在employees和jobs表中均存在
正确的写法为：
select last_name,employees.job_id,job_title from employees,jobs
where employees.job_id=jobs.job_id;
当每次都需要用表名时，可以为表取别名,但是要值得注意的就是，取完别名后，原表名就不能用了
例如这里的代码可以这样写：

SELECT last_name,e.job_id,job_title 
FROM employees AS e,jobs AS j
WHERE e.job_id=j.job_id;
```

案例4：查询有奖金的员工名和部门名（加筛选）

```mysql
select last_name, department_name,commission_pct
from employees as e,departments as d
where e.department_id=d.department_id and commission_pct is not null
```

案例5：查询城市名中第二个字符为o的部门名和城市名

```mysql
SELECT city,department_name FROM `departments`,`locations`
WHERE departments.`location_id`=locations.`location_id`
AND city LIKE '_o%';
```

案例5：查询每个城市的部门个数（可以加分组）

```mysql
SELECT COUNT(*) as 个数,department_name,city FROM departments AS d, locations AS l
WHERE d.location_id=l.`location_id`
GROUP BY l.city
```

案例6：查询有奖金的，每个部门的部门名和部门领导的领导编号和该部门的最低工资

```mysql
SELECT MIN(salary),department_name,e.manager_id FROM  departments AS d, employees AS e
# where d.`manager_id`=e.`manager_id` 其中manager_id 在departments表中不齐全，所以用下面那个连接条件
WHERE d.`department_id`=e.`department_id`
AND commission_pct IS NOT NULL
GROUP BY `department_name`,d.`manager_id`
```

案例7：查询每个工种的工种名和员工的个数，并且按照员工的个数降序

```mysql
SELECT COUNT(*) AS 员工个数,job_title 
FROM `employees` AS e ,`jobs` AS j
WHERE e.`job_id`=j.`job_id`
GROUP BY job_title
ORDER BY COUNT(*) DESC;
```

案例8：（实现三表连接）查询员工名、部门编号和所在的城市

```mysql
SELECT last_name,department_name,city
FROM employees AS e,departments AS d,locations AS l
WHERE e.`department_id`=d.`department_id`
AND d.`location_id`=l.`location_id`;
```

## 5.2、非等值连接

案例1：查询员工的工资和工资级别

```mysql
SELECT salary,grade_level FROM employees AS e,job_grades AS j
WHERE salary BETWEEN j.`lowest_sal` AND j.`highest_sal`;
```

## 5.3、自连接：一张表重复使用

![image-20210712153000122](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20210712153000122.png)



案例：查询员工名和该员工对应的领导名

```mysql
SELECT 
e.employee_id, e.last_name,m.employee_id, m.last_name
FROM employees AS e,employees AS m
WHERE e.`manager_id`=m.`employee_id`;
employee_id  last_name    employee_id  last_name  
-----------  -----------  -----------  -----------
        101  Kochhar              100  K_ing      
        102  De Haan              100  K_ing      
        103  Hunold               102  De Haan    
        104  Ernst                103  Hunold     
        105  Austin               103  Hunold     
        106  Pataballa            103  Hunold     
        107  Lorentz              103  Hunold     

```

###### sql99标准

```mysql
语法：

​			select 查询列表
​			from 表1 别名 【连接类型】
​			join   表2 别名
​			on     连接条件

​			【where 筛选条件】
​       	【group by 分组】
​			【having 筛选条件】
​			【order by 】
分类：
内连接：inner
外连接：
		左外：left  【outer】
		右外：right 【outer】
		全外：full  【outer】
交叉连接：cross
```

##  5.1、内连接

```mysql
语法：
/*
select 查询列表
from 表1 别名
inner join 表2 别名
on 连接条件；
分类：
等值连接
非等值连接
自连接
特点:
1、可以添加排序、分组、筛选
2、inner可以省略
3、筛选条件放在where 后面，连接条件放在on后面，提高了分离性，便于阅读
4、inner join连接和sql92语法中的等值连接效果是一样的，都是查询多表的交集部分

*/
```

## 5.2、等值连接

案例1：查询员工名、部门名

```mysql
SELECT	`department_name`,`last_name`
FROM `departments` d
INNER JOIN `employees` e
ON d.`department_id`=e.`department_id`;

```



案例2：查询名字中包含e的员工和工种名（添加筛选）

```mysql
SELECT  last_name,job_title
FROM `employees` e
INNER JOIN `jobs` j
ON e.`job_id`=j.`job_id`
WHERE e.`last_name` LIKE '%e%'
```



案例3：查询部门个数>3的城市名和部门个数（分组+筛选）

```mysql
SELECT city,COUNT(*) 部门个数
FROM departments d
INNER JOIN locations l
ON d.`location_id`=l.`location_id`
GROUP BY city
having count(*)>3;
```



案例4：查询哪个部门的部门员工个数>3的部门名和员工个数，并按照个数进行排序（排序）

```mysql
SELECT  department_name,COUNT(*) 员工个数
FROM departments d
INNER JOIN employees  e
ON e.`department_id`=d.`department_id`
GROUP BY department_name
HAVING COUNT(*)>3
ORDER BY COUNT(*) DESC;
```



案例5：查询员工名、部门名、工种名，并按照部门名降序（三表连接）

```mysql

SELECT last_name,department_name,job_title
FROM `employees` e
INNER JOIN `departments` d ON e.`department_id`=d.`department_id`
INNER JOIN `jobs` j ON j.`job_id`=e.`job_id`
ORDER BY department_name DESC
```

## 5.3、非等值连接

案例1：查询员工的工资级别

```mysql
select salary,grade_level
from employees e
inner join job_grades j 
on salary between lowest_sal and highest_sal;#(非等值连接的条件)
```



案例2：查询每个工资级别个数>2的个数，并且按照工资的级别降序

```mysql
SELECT  COUNT(*),salary,grade_level
FROM employees
JOIN job_grades ON  salary BETWEEN lowest_sal AND highest_sal
GROUP BY grade_level
HAVING COUNT(*)>2
ORDER BY grade_level DESC;
```

## 5.4、自连接

案例：查询员工的名字和他对应的上级的名字

```mysql
SELECT e.last_name,m.last_name 领导名字
FROM employees e
JOIN employees m ON e.`manager_id`=m.`employee_id`;
```

<div style="page-break-after:always"></div>

## 5.5、外连接（分主从表）

### 5.5.1 左外连接（(A∩B)∪A）
```mysql
	语法：查询的是左表连接以及其交集部分
	select 字段列表 
	from 表1 
	left outer join 表2
	on 连接条件
```

```mysql
/*
应用场景：用于查询一个表有，另一个表没有的记录
特点：
1、外连接的查询为主表中的所有记录
		如果从表中有和它匹配的，则显示匹配的值
		如果从表中没有和它匹配的，则显示null
		外连接查询结果=内连接结果+主表中有而从表没有的记录
2、左外连接，left	join 左边的是主表
   右外连接，right join右边的主表
3、左外和右外交换；两个表的顺序，可以实现同样的结果
一般来说，你要查询的哪个表的的主要信息，就把该表当做主表
4、全外连接=内连接的结果+表1有表2没有+表2中没有但是表1中有的
*/
```

###  5.5.2 右外连接((A∩B)∪B）
语法：	查询的是右表所有数据以及其交集部分
```mysql
select 字段列表 
from 表1 
right outer join 表2
on 连接条件
```


引入：查询男朋友 不在男神表的女神名

```mysql
#左外连接
SELECT b.name,bo.*
FROM beauty b
LEFT OUTER JOIN boys bo
ON b.`boyfriend_id`=bo.`id`
WHERE bo.id IS NULL

#右外连接
SELECT b.name,bo.*
FROM boys bo
RIGHT OUTER JOIN beauty b
ON b.`boyfriend_id`=bo.`id`
WHERE bo.id IS NULL
```

![image-20210713111821696](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20210713111821696.png)

![image-20210713111849787](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20210713111849787.png)

这里的id不能完全对应，因为boys表中的id只有1-4，而beauty表中boyfriend_id有除了1-4以外的数字

![image-20210713112130913](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20210713112130913.png)

案例1：查询哪个部门没有员工（部门表是主表）

```mysql
SELECT d.*,e.employee_id
FROM departments d #主表
LEFT OUTER JOIN employees e #从表
ON e.`department_id`=d.`department_id`#连接条件
WHERE e.employee_id IS NULL#筛选条件
```

## 5.6、交叉连接

语法：

```mysql
select 表1，表2
from 表1
cross  join 表2
```

## 总结

## 练习题

案例1：查询编号大于3的女神男朋友信息，如果有则列出详细，如果没有，用null填充

```mysql
SELECT beauty.name,beauty.id,boys.*
FROM beauty
LEFT OUTER JOIN boys
ON beauty.`boyfriend_id`=boys.`id`
WHERE beauty.id>3
name           id      id  boyName    userCP  
---------  ------  ------  ---------  --------
热巴              4       2  鹿晗              800
周冬雨             5  (NULL)  (NULL)       (NULL)
周芷若             6       1  张无忌             100
岳灵珊             7  (NULL)  (NULL)       (NULL)
小昭              8       1  张无忌             100
双儿              9  (NULL)  (NULL)       (NULL)
王语嫣            10       4  段誉              300
夏雪             11  (NULL)  (NULL)       (NULL)
赵敏             12       1  张无忌             100
```

案例2：查询哪个城市没有部门

```mysql
SELECT city,department_name
FROM locations l
LEFT OUTER JOIN departments d
ON l.location_id=d.`location_id`
WHERE d.`department_id` IS NULL;
```

案例3：查询部门名为SAL或IT的员工信息

```MYSQL 
SELECT e.*,d.`department_name`
FROM departments d
LEFT JOIN  employees e
ON e.`department_id`=d.`department_id`
WHERE d.`department_name` IN('SAL','IT')
```



## 5.7 进阶7：子查询

```mysql
含义：
出现在其他语句中的 select语句，称为子查询或内查询
外部的查询语句，称为主查询或外查询

分类：
按照查询出现的位置：
		select 后面：
				仅仅支持标量子查询
		from 后面：
				支持表子查询
		where或 having后面：       🔑
				支持标量子查询（单行）★
				列子查询	（多行） ★
				行子查询（用的较少）
		exists后面（相关子查询）
按结果集的行列数不同：
		标量子查询（结果集只有一行一列）
		列子查询（结果集只有一列多行）
		行子查询（结果集有一行多列）
		表子查询（结果集一般为多行多列）
```

查询单行单列信息

```mysql
语法：
select * from 表名 where 查询字段(select 聚合函数 from 表名);
案例：查询工资最高的员工信息
select * from emplyees where salay=(select max(salary) from employees)

select *,max(salary) from employees;
```

查询多行单列信息
语法：


查询多行多列信息

```mysql
案例：查询员工入职的日期是2011-11-11之后的员工信息和部门信息
#1.select * from employee where join_data>'2011-11-11';
#2.select* from dept
一句写完
select * from dept as t1, select*from employee where join_data>'2011-11-11' as t2
where t1.id=t2.dept_id; -- 连接条件

-- 普通的内连接
select *from employee as t1,dept as t2 where t1.id=t2.dept_id and t1.join_data>'2011-11-11'
```



### 4.8.1 where或having后面

1、标量子查询（单行子查询）

2、列子查询（一列多行也叫多行子查询）

3、行子查询（用的较少）

特点：

- 放在小括号内

- 子查询一般放在条件的右侧

- 标量子查询，一般搭配操作符使用

   < ,>,<=,>=,<>

- 列子查询，一般搭配多行操作符使用

  IN、ANY/SOME、ALL


### 4.8.2 标量子查询（单行子查询）

案例1：谁的工资比Abel高

# 六、约束

概念：对表中的数据进行限定，保证数据的正确性、有效性和完整性

分类：

​	1.主键约束：primary key

​	2.非空约束：not null

​	3.唯一约束: unique

​	4.外键约束: foreign key

### 6.1 主键约束：primary key

1.注意：

​	1.1.含义：非空且唯一 

​	1.2.一张表只能有一个字段为主键

​	1.3.主键就是表中记录的唯一标识

2.在创建表时，添加主键约束

```mysql
create table 表名(
  id int primary key,-- 给id添加主键约束
  name varchar(20)
);
```

3.删除主键约束

```mysql
-- 错误写法 alter table 表名 modify id int;
alter table 表名 drop primary key;-- 正确写法,为什么不用写出id呢？因为一张表只能有一个字段为主键
```

4.在创建表完成后，添加主键约束

```mysql
alter table 表名 modify id int primary key;
```

5.自动增长

​	5概念：如果某一列是数值类型的，只用auto_increment 可以来完成值的自动增长

​	5.2创建表的时候，添加主键约束，并且完成主键自动增长

5添加自动增长

```mysql
  create table /*if not EXISTS */表名(
    id int PRIMARY key auto_increment ,
    name VARCHAR(20));
```

例如：

```mysql
insert into stu1 VALUES(1,"12");
insert into stu1 VALUES(null,"12");

id	name
1	12
2	12

```

5.2删除自动增长

```mysql
alter table 表名 modify id int;
```

5.2添加自动增长

```mysql
alter table 表名 modify id int primary key auto_increment;
```



### 6.2.非空约束：not null

1.创建表时添加约束

```mysql
create table 表名(
  id int,
  name varchar(20) not null-- name为非空
);
```

2.创建表后添加约束

```mysql
alter table 表名 modify name varchar(20) not null;
```

3.删除非空约束

```mysql
alter table 表名 modify name varchar(20) ;
```



### 6.3.唯一约束: unique

1.创建表时添加唯一约束

```mysql
create table 表名(
  id int unique,-- id唯一
  phone_number varchar(20) unique-- phone_number唯一
);
-- 注意mysql中，唯一约束限定的列的值可以有多个null值
```

2.创建表后添加约束

```mysql
alter table 表名 modify name varchar(20) unique ;
```

3.删除唯一约束

```mysql
alter table 表名 drop index 列名;
```



### 6.4.外键约束: foreign key

1. 在创建表时，可以添加外键

使用环境：一般使用在两个表之间的关联

```mysql
create table 表名(
  ...
  外键列
  constraint 外键名称 foreign key (外键列的名称) references 主表名称(主表/*主键*/列名称)-- 写在主表上
);
-- ”constraint 外键名称“可以省略的，若不写，系统会自动分配一个外键名称
```

例子：创建一员工表和一个部门表，员工表(主表)的部门id，关联部门表的id

```mysql
drop table if EXISTS employee;
drop table if EXISTS department;
-- 一定要先创建主表（部门表），也就是没有外键的那张表

-- 主表：部门表

CREATE TABLE if not EXISTS department ( 
  id INT PRIMARY KEY auto_increment, 
  dep_name VARCHAR ( 20 ), 
  dep_location varchar(20)
);
-- 从表：员工表
create table employee(
  id int primary key auto_increment,
  name varchar(20),
  age int,
  dep_id int, -- 外键对应主表的主键 
  constraint emp_dept_id foreign key (dep_id) references department(id)-- 其中emp_dept_id 为外键的名字
);


insert into department values(null,"研发部","广州"),(null,"销售部","深圳");
INSERT into employee VALUES(null,'张三',20,1),(null,'李四',21,1),(null,'王五',20,1),(null,'老王',20,2),(null,'大王',22,2),(null,'小王',18,2);

select *from employee;
select *from department;
-- 注意：先执行主表再执行从表，因为从表中含有department表名
```

2. ##### 删除外键

   ```mysql
   -- 删除外键
   alter table 从表表名 drop foreign key 外键名;
   -- 例如上个表中的employee表
   alter table employee drop foreign key emp_dept_id ;
   ```

3. 已经创建好了，再添加外键

   ```mysql
   alter table 从表表名 add constraint 外键名（自己取一个别名就行了） foreign key (外键对应主表的主键 ) references 主表名(主表主键列名称);
   alter table employee add constraint emp_dept_id  foreign key (emp_dept_id) references department(id);
   ```


#### 5.4.1 级联操作

主表的一个数据更改以后，从表的数据也要更改，比如department主表的部门编号改变了，从表被外键关联的部门编号也会自动更改

##### 5.4.1.添加级联操作(慎用！！！)

```mysql
alter table 表名 add constraint 外键名称 foreign key (外键字段名称) references 主表名称(主表列名称) on update cascade on delete cascade;-- on update cascade on delete cascade这行代码就是级联更新操作;
-- 注意的是在创建表的时候没有创建外键
```

##### 5.4.2.分类

1. 级联更新 on update cascade
2. 级联删除 on delete cascade

# 七、数据库的设计

## 7.1 多表之间的关系

## 7.1.1 分类

* 一对一(了解)-->例如：一个人和身份证的关系
* 一对多(多对一)-->例如：如部门和员工,一个部门有多个员工，一个员工只能对应一个部门
* 多对多-->例如：如学生和课程的关系

### 7.1.2 实现关系

​	1.实现一对多

​	方案：在“一”的那个表搞一个主键，“多”的一方建立外键

​	2.实现多对多

​	方案：多对多的实现需要借助一张中间表，其中中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键

![_多表关系_多对多](Typora图片文件区/_多表关系_多对多.png)

3.一对一

![_多表关系_一对一](Typora图片文件区/_多表关系_一对一.png)

案例：

![_多表关系_旅游案例图](Typora图片文件区/_多表关系_旅游案例图.png)

```mysql

  -- 表一：旅游线路分类表tab_category
  -- 创建旅游线路分类表tab_category
  -- cid 旅游线路分类主键，自动增长
  -- cname 旅游线路分类名称非空，唯一，字符串100 
		
-- DROP TABLE tab_route;
-- DROP TABLE tab_category;
-- DROP TABLE tab_user;
  CREATE TABLE tab_category(
    cid INT  PRIMARY KEY AUTO_INCREMENT, 
    cname VARCHAR(100) NOT NULL UNIQUE
  );


/*
表二：旅游线路表tab_route 
 	创建旅游线路表tab_route 
	rid 旅游线路主键，自动增长
	rname 旅游线路名称非空，唯一，字符串100
 	price 价格
	rdate 上架时间，日期类型 
	cid 外键，所属分类
*/
create table tab_route(
  rid int primary key auto_increment,
  rname varchar(100) not null unique, 
  price double,
  rdate date, 
  cid int,
  foreign key(cid) references tab_category(cid)
);

   -- 表三：用户表tab_user 
   /*
   创建用户表tab_user 
	uid 	用户主键，自增长
	username	用户名长度100，唯一，非空 
	password	密码长度30，非空 
	name真实姓名长度100 
	birthday生日
	sex性别，定长字符串1
	telephone手机号，字符串11
    email邮箱，字符串长度100
    */

 create table  tab_user(
      uid int primary key auto_increment,
      username varchar(100) unique not null,
      password  varchar(30) not null,
      birthday date,
      sex varchar(1)default "男",
      telephone varchar(11),
      email varchar(100)
    );
-- 创建收藏表tab_favorite
/*
	rid 旅游线路 id，外键 
	date 收藏时间 
	uid 用户 id，外键
	rid和uid不能重复，设置复合主键，同一个用户不能收做同一个线路两次
*/
create table tab_favorite(
  rid int, -- 线路id
  date datetime,
  uid int, -- 用户id
  -- 创建复合主键
  primary key(rid,uid),-- 联合主键，rid,uid这两个外键合起来作为tab_favorite的主键
  foreign key (rid) references tab_route(rid), 
  foreign key (uid) references tab_user(uid)
);
```

## 6.2 数据库设计的范式

* 概念：设计数据库时，需要遵循的一些范式。

  设计关系[数据库](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%BA%93/103728)时，遵从不同的规范[要求](https://baike.baidu.com/item/%E8%A6%81%E6%B1%82/3598753)，设计出合理的[关系型数据库](https://baike.baidu.com/item/%E5%85%B3%E7%B3%BB%E5%9E%8B%E6%95%B0%E6%8D%AE%E5%BA%93/8999831)，这些不同的规范要求被称为不同的范式，各种范式呈<u>递次规范</u>，越高的范式数据库冗余越小。

  [关系数据库](https://baike.baidu.com/item/%E5%85%B3%E7%B3%BB%E6%95%B0%E6%8D%AE%E5%BA%93/1237340)有六种范式：第一范式（1NF）、第二范式（2NF）、[第三范式](https://baike.baidu.com/item/%E7%AC%AC%E4%B8%89%E8%8C%83%E5%BC%8F/3193798)（3NF）、巴斯-科德范式（BCNF）、[第四范式](https://baike.baidu.com/item/%E7%AC%AC%E5%9B%9B%E8%8C%83%E5%BC%8F/3193985)(4NF）和[第五范式](https://baike.baidu.com/item/%E7%AC%AC%E4%BA%94%E8%8C%83%E5%BC%8F/5025271)（5NF，又称完美范式）。

* 分类

  1 、第一范式（1NF）：每一列都是不可分割的原子数据项，就是不能是合并的列可以合并行

  2 、第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖）

  3、[第三范式](https://baike.baidu.com/item/%E7%AC%AC%E4%B8%89%E8%8C%83%E5%BC%8F/3193798)（3NF）：在2NF基础上，任何非主[属性](https://baike.baidu.com/item/%E5%B1%9E%E6%80%A7)不依赖于其它非主属性（在2NF基础上消除传递依赖）

  ​

### 6.2.1 第一范式

​	第一范式（1NF）：每一列都是不可分割的原子数据项

​	第一范式存在的缺点：

1.出现严重的数据冗余

2.数据添加出现问题，数据不合法。

3.删除数据出现问题。

![第一范式的缺点](Typora图片文件区/第一范式的缺点.png)



### 6.2.2 第二范式

​	第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖）.就是要消除部分依赖，只会解决第一范式中的数据冗余问题



* 几个概念：

  1、函数依赖：A-->B,如果通过A属性（<u>属性组</u>）的值，可以确定唯一B属性的值。则称B依赖于A

  ​	例如：学号-->姓名  （<u>学号，课程名称</u>）-->分数

  2、完全函数依赖：A-->B，如果A是一个属性组，则B属性值的确定需要依赖A属性组中所有属性值。

  ​	例如：（<u>学号，课程名称</u>）-->分数

  3、部分函数依赖:A-->B，如果A是一个，则B属性值的确定需要<u>**只**</u> 需要依赖A属性组中<u>**某一些**</u>属性值。

  ​	例如：（<u>学号，课程名称</u>）-->姓名

  4、传递函数依赖：A-->B,B-->C	如果通过A属性（<u>属性组</u>）的值，可以确定唯一B属性的值，在通过B属性值唯一确定C属性值，则C传递函数依赖于A

  ​	例如：学号-->系名，系名-->系主任

  5、码：如果在一张表中，一个属性或属性组，被其他所有属性完全依赖，则称这个属性（属性组）为该表的码                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          

  ​	例如：该表中码为：（<u>学号，课程名称</u>）

  ​	*主属性：<u>码属性组中的所有属性</u>

  ​	*非主属性：除过<u>码属性组的属性</u>


### 6.2.2 第三范式

[第三范式](https://baike.baidu.com/item/%E7%AC%AC%E4%B8%89%E8%8C%83%E5%BC%8F/3193798)（3NF）：在2NF基础上，任何非主[属性](https://baike.baidu.com/item/%E5%B1%9E%E6%80%A7)不依赖于其它非主属性（在2NF基础上消除传递依赖）解决了第2和第3个问题

# 八、数据库的备份和还原

1..命令行：

*语法：mysqldump -u用户名 -p密码  数据库的名称 >  保存的路径

*还原：

​	1.登录数据库

​	2.创建数据库

​	3.使用数据库

​	4.执行文件。source 文件路径

2.图形化工具：右击数据库名，选择备份，选择备份到本地文文件。

# 









