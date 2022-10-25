---
title: MySQL基础
date: 2021-10-05 13:52:25.092
updated: 2021-10-26 21:07:08.027
url: /archives/mysqlbasic
categories: MySQL
tags: 基础 | MySQL
---

# 数据库
---
### 程序员写的MySQL顺序
![程序员MySQL](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic//image_1635252930687.png)
### 机读的MySQL顺序
![机读MySQL](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic//image_1635253125793.png)
---
### SQL的连接
![不同的连接SQL](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic//image_1635253577948.png)
**数据库： 存放不同类型的数据**

数据库--->表---->数据  字段名称 字段数据类型

从存储位置：

1. 基于磁盘(文件)  数据可以永久保存 IO 效率较慢  mysql/oracle/sqlserver 
2. 基于内存 效率比较快 数据有可能丢失  redis/mogodb

从关系上：

1. 关系型数据库:mysql/oracle/sqlserver 

2. 非关系型数据库:NOSQL redis/mogodb

# MySQL数据库

### MySQL简介

特点: 开源 小而易用

使用MySQL步骤:

1. 下载安装MySQL的server(服务端程序)

   用户名:root 

2. 使用客户端程序进行连接

   1. CMD

       mysql -u用户名 -hip地址 -p密码

3. SQL操作

   SQL:结构化查询语言

​       DDL: Data Definition Language 数据定义语言 create drop alter

   ​	 DML:Data Manipulation Language 数据操作语言 INSERT、UPDATE和DELETE

   ​	 DCL:Data Control Language 数据控制语言 GRANT、DENY、REVOKE

​        DQL:Data Query Language 数据查询语言 SELECT

4. 查看MySQL服务的内容

   MySQL服务--->MySQL的数据库-->表--->数据  字段名称 字段数据类型

   - 4.1  查看MySQL服务所有的数据库: SHOW DATABASES;
   
   - 4.2  选择使用哪一个数据库:USE 数据库名称;
   
   - 4.3  查看数据库里面所有的表: SHOW TABLES;
   
   - 4.4  查看表的所有数据 SELECT * FROM 表名;
   
   - 4.5  查看表的表结构 DESC 表名;
   
### SQL语句详细

5. DDL语句

   - 5.1  创建数据库: CREATE DATABASE 数据库名;(一旦创建数据库名无法更改)

   - 5.2  查看当前使用的哪一个数据库: SELECT DATABASE();

   - 5.3  查看创建的数据库的基本信息:  SHOW CREATE DATABASE 数据库名称;

   - 5.4  创建表语法: 

     ```mysql
     create table 表名{
           字段名1(属性名) 字段数据类型 约束;
           字段名2(属性名) 字段数据类型 约束;
           字段名3(属性名) 字段数据类型 约束;
           字段名4(属性名) 字段数据类型 约束
           ***
     };
     ```

     ```mysql
     //创建用户表 tb_userinfo 
     //用户id 用户姓名 
     CREATE TABLE tb_userinfo (
     	uid INT,
     	uname VARCHAR ( 20 ),
     	upass VARCHAR ( 64 ),
     	uage TINYINT ( 2 ) UNSIGNED,
     	birthday date,
     	introduce text,
     	createdtime datetime,
     updatetime datetime 
     );
     ```

     - 5.5  修改表结构

       - 5.5.1 新增字段

         ```mysql
         ALTER TABLE 表名 ADD 新的字段名 字段类型;
         ```

       - 5.5.2 修改字段名称

         ```mysql
         ALTER TABLE 表名 CHANGE 新的字段名 字段类型;
         ```

       - 5.5.3 修改字段数据类型

         ```mysql
         ALTER TABLE 表名 MODIFY 字段名 新的字段类型;
         ```

       - 5.5.4 删除某个字段

         ```mysql
         ALTER TABLE 表名 DROP 字段名;
         ```
         
       - 5.5.5 修改表名
         
         ```mysql
         ALTER TABLE 表名 RENAME 新表名;
         ```
         
       - 5.5.6 删除表
       
         ```mysql
         DROP TABLE 表名;
         ```
   
6. DML语句

   - 6.1 新增语句

     ```mysql
     INSERT INTO 表名 VALUES ();//按照顺序给定值 每个字段都要给值
     ```

     ```mysql
     INSERT INTO tb_userinfo VALUES(1,'jim','1234',20,'2019-01-01',null,'2020-02-12',null);
     ```

     指定字段新增

     ```mysql
     INSERT INTO 表名 (字段名1,字段名2,字段名3,...)VALUES(给定值)
     ```

     ```mysql
     INSERT INTO tb_userinfo (uid,uname,upass,uage,birthday,createdtime) VALUES(3,'lucy','1234',20,'1997-07-04','2020-01-01 12:30:30');
     ```

     

   - 6.2  删除数据语句

     ```mysql
     DELETE FROM 表名;//清空表数据
     DELETE FROM 表名 WHERE 字段名 = 符合条件字段值 ;//根据条件删除指定数据
     ```

     ```mysql
     DELETE FROM tb_userinfo WHERE uid = 1 && uname = 'jim';
     ```

   - 6.3 修改数据语句

     ```mysql
     UPDATE 表名 SET 字段名 = 新的字段值;
     UPDATE 表名 SET 字段名 = 新的字段值 WHERE 字段名 = 符合条件字段值;//根据条件修改指定记录数据
     ```

     ```mysql
     UPDATE tb_userinfo SET uage = 18,upass = '1111';//所有数据都进行了修改
     UPDATE tb_userinfo SET uage = 18,upass = '1234' WHERE uid = 1 ;//修改特定记录的值
     ```

7. DQl语句

   ```mysql
   SELECT * FROM 表名;// * 通配所有表字段
   SELECT 字段名1,字段名2,字段名3,... FROM 表名;//查询特定字段
   ```

   ### 数据约束与关系
   
   >行级约束: 主键 唯一 默认 非空
   >
   >表级约束: 主键 外键
   
   1. 主键约束(PRIMARY KEY)
      - 1. 每张表都有唯一的一个主键列
      - 2. 任意类型的字段都可以充当主键列
      - 3. 主键列的数据非空且不可重复(NOT NULL+UNIQUE)
      

**当主键列是整型时,一般都会结合自增特性进行使用  anto_increment(主键列的数据自增)**

自增:初始值从1开始(可以结合语句进行修改) 每次自增加1

```mysql
CREATE TABLE tb_f (
	eid VARCHAR(255) PRIMARY KEY AUTO_INCREMENT,
	ename VARCHAR(20) NOT NULL UNIQUE
);
ALTER TABLE TB_F AUTO_INCREMENT = 1000;//设置自增初始值为1000
SELECT * FROM tb_f;
INSERT INTO tb_f (ename) VALUES('ZHANGBAOLEI');
UPDATE tb_e SET eid = 1001 WHERE ename = '陈刚';
```

MySQL中提供了随机生成字符串的方法:

```mysql
SELECT UUID();//字符串充当主键使用 不会重复
```

```mysql
INSERT INTO tb_f (tb_f.eid,tb_f.ename)VALUES(UUID(),'tom');
```

2. 外键约束 foreign key 表级约束

   **两张表有关联关系:主外键有关联关系**

用户表: tb_userinfo  从表数据 用户参照角色表的角色数据

角色表: tb_role  主表数据

外键列:用户表的roleid是外键列

主键列:角色表的roleid是主键列

```mysql
CREATE TABLE tb_role( 
rid INT PRIMARY KEY AUTO_INCREMENT,
rname VARCHAR(10) NOT NULL UNIQUE,
createtime datetime,
updatetime datetime
);
ALTER TABLE tb_role AUTO_INCREMENT = 1000;
```

**用户有角色:1-1的关联关系**

建立关联关系

```mysql
ALTER TABLE tb_userinfo ADD CONSTRAINT FOREIGN KEY(rid)REFERENCES tb_role(rid);
```

在从表中插入数据

```mysql
INSERT INTO  tb_userinfo(uid,uname,rid)VALUES(4,'LUCY',1001);//插入数据是必须插入外键对应的主表中已经存在的字段值
```

删除主表数据

```mysql
DELETE FROM tb_role WHERE rid = 1000;
```

```
> 1451 - Cannot delete or update a parent row: a foreign key constraint fails (`learnmysql`.`tb_userinfo`, CONSTRAINT `tb_userinfo_ibfk_1` FOREIGN KEY (`rid`) REFERENCES `tb_role` (`rid`))
```

**当存在主外键关系时,子表的数据必须严格参照主表的数据,对于主表数据,在删除记录时要先确定子表中没有与该记录对应的外键值,否则将会删除失败**

>在企业级开发中禁止使用外键,在代码的层面上维护表与表的关联关系(弱外键)

#### 一对多的表设计

角色和权限

**使用中间表维护两张表的数据****

![image-20210216125848526](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210216125848526.png)

![image-20210216125913915](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210216125913915.png)

创建中间表

![image-20210216130325908](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210216130325908.png)

新增角色

```mysql
INSERT INTO tb_role (rname,createtime) VALUES ('admin',now());
SELECT * FROM tb_p;//遍历全部权限
INSERT INT tb_role_p(rid,pid)VALUES (rid,选择的权限);
```

#### 多对多的表设计

老师和学生

![image-20210216131239586](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210216131239586.png)

![image-20210216131313523](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210216131313523.png)![image-20210216131336224](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210216131336224.png)

![image-20210216131406332](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210216131406332.png)

解决方法:

使用中间表维护:

![image-20210216131835900](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210216131835900.png)

![image-20210216131850095](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210216131850095.png)

### 约束

1. null

2. not null

3. unique

4. 主键约束  primary key

   整数充当主键列+自增 auto_increment 

   修改自增初始值		

```mysql
ALTER TABLE 表 AUTO_INCREMENT = 初始数据;
```

​		字符串充当主键列 + uuid()

5. 外键约束 foreign key

   涉及两张表 主表 从表(外键列)

   **从表(外键列)的数据要严格参照主表主键列的数据**
### DQL数据查询语言

```mysql
SELECT 字段 from 表1,表2 (WHERE 条件 GROUP BY 字段 HAVING 条件 ORDER BY 字段 LIMIT 每页的记录数);
```

```mysql
 SELECT selection_list /*要查询的列名称*/
 FROM table_list /*要查询的表名称*/
[WHERE condition /*筛选数据行的条件*/
 GROUP BY grouping_columns /*对结果分组*/
 HAVING condition /*分组后的筛选行的条件*/
ORDER BY sorting_columns /*对结果排序*/
LIMIT offset_start, row_count ]/*结果限定*/
```

%通配任何内容 _通配一个字母或数字

去除重复数据: ==DISTINCT==关键字(只能出现一次,对某个字段去重)

```mysql
SELECT DISTINCT 字段名 FROM 表名;
```

null进行算数运算时,结果都为null;

MySQL判断字段名是否为null:

```mysql
ifnull(字段名,设定的数据);// 当字段名对应的字段值为null时,使用设定的数据进行替换,否则就用自身的字段值
```

排序:ORDER BY 字段名 ASC(升序/默认)/DESC(降序)

```mysql
//根据价格升序
SELECT * FROM products ORDER BY prod_price ASC;//只针对数值型
//根据价格升序然后根据id降序排列
SELECT * FROM products ORDER BY prod_price ASC,vend_id DESC;
```

分组查询 GROUP BY

聚合函数:(返回字段只有一个记录)

```mysql
// 查询表的总记录数 
SELECT COUNT(字段名\主键列\*) FROM 表名; 
// 查询字段的最值/和/平均数
SELECT MAX(字段名)/MIN(字段名)/SUM(字段名)/AVG(字段名)  FROM 表名; //查询出的一张临时表
// 字段/表 起别名查询  AS '新的字段名';
```

HAVING关键字:对分组之后的数据进行过滤时使用

##### WHERE&&HAVING

共同点:

1. 都是用来过滤掉不符合条件的数据或记录

不同点:

1. WHERE 在 GRUOP BY之前 HAVING 在 GROUP BY 之后
2. WHERE 不能与组函数一起使用 HAVING可以

### 关联查询

#### 1. 等值查询

连接的条件是一个等号

**两张表关联查询至少有一个条件**

```mysql
SELECT empno,ename,sal,dname FROM emp,dept;
```

产生了笛卡尔积的数据

![image-20210222112835635](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222112835635.png)

过滤不正确的记录

```mysql
SELECT empno,ename,sal,dname FROM emp,dept WHERE  emp.deptno= dept.deptno;
```

![image-20210222113022425](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222113022425.png)

```mysql
-- 查询员工信息，要求显示：员工号，姓名，月薪，薪水的级别
SELECT e.empno,e.ename,e.sal,s.GRADE FROM emp e ,salgrade s WHERE e.sal BETWEEN s.LowSAL AND s.HISAL;
```

### 连接查询

#### 1. 内连接   inner join on(where)

等同于等值连接

```mysql
SELECT d.deptno,d.dname,COUNT(*) FROM dept d INNER JOIN emp e on d.deptno = e.deptno GROUP BY e.deptno;
```

![image-20210222114439532](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222114439532.png)

#### 2. 左外连接left join on 

==以左表为基准,右表没有的数据使用null/0进行填充==

```mysql
SELECT d.deptno,d.dname,COUNT(E.empno) FROM dept d LEFT  JOIN emp e on d.deptno = e.deptno GROUP BY e.deptno;
```

![image-20210222114822476](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222114822476.png)



#### 3. 右外连接 right join on

==以右表为基准,左表没有的数据使用null/0进行填充==

```mysql
SELECT d.deptno,d.dname,COUNT(E.empno) FROM emp e RIGHT JOIN dept d on d.deptno = e.deptno GROUP BY e.deptno;
```

![image-20210222114938162](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222114938162.png)

### 自连接

==通过别名，将同一张表视为多张表==

```mysql
-- 查询员工姓名和员工的老板的名称
SELECT
	e1.empno,
	e1.ename,
	e1.mgr bossnum,
	e2.ename bossname
FROM
	emp e1 LEFT JOIN emp e2
ON
	e1.mgr = e2.empno;
```

### 子查询

**子查询的作用：查询条件未知的事物**

```mysql
-- 查询工资为20号部门平均工资的员工信息
-- 查询的结果当成另外一个查询的条件
SELECT AVG(sal) FROM emp  WHERE deptno = 20;
SELECT * FROM emp WHERE sal = (SELECT AVG(sal) FROM emp  WHERE deptno = 20);
```

![image-20210222120646067](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222120646067.png)

![image-20210222120657811](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222120657811.png)

### 集合查询

**Union** **和union all**

>使用并集运算，查询20号部门或30号部门的员工信息
>
>select * from emp where deptno = 20
>
>union
>
>select * from emp where deptno = 30;
>
>注意：
>
>union：二个集合中，如果都有相同的，取其一
>
>union all：二个集合中，如果都有相同的，都取

###  分页查询 LIMIT

```mysql
-- 分页查询
-- SELECT * FROM emp LIMIT ?; //从第一条数据开始查询 查size条数据  只查第一页数据
-- SELECT * FROM emp LIMIT START(索引/与用户请求查看第几页的数据有关联关系/当前页的数据,使用户提交的请求),SIZE;//从第START页开始查询,查询Size条数据
SELECT * FROM emp LIMIT 5;
SELECT * FROM emp LIMIT 5,5;
SELECT * FROM emp LIMIT 10,5;
```

![image-20210222121930150](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222121930150.png)

![image-20210222121941246](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222121941246.png)

![image-20210222121956776](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/javabasic/image-20210222121956776.png)

### 函数

#### 1. 字符串函数

| **函数**                     | **功能**                                                |
| ---------------------------- | ------------------------------------------------------- |
| concat(str1,str2,…)          | 连接字符串                                              |
| insert(str,pos,len,newstr)   | 字符串str从第pos位置开始的len个字符替换为新字符串newstr |
| lower(str)                   | 转成小写                                                |
| upper(str)                   | 转成大写                                                |
| length(str)                  | 返回字符串str的长度                                     |
| char_length(str)             | 返回字符串str的长度                                     |
| lpad(str,ien,padstr)         | 返回字符串str，其左边由字符串padstr填补到len字符串长度  |
| rpad(str,len,padstr)         | 返回字符串str，其左边由字符串padstr填补到len字符串长度  |
| trim(str)                    | 去掉字符串str前缀和后缀的空格                           |
| repeat(str,count)            | 返回str重复count次的结果                                |
| replace(str,from_str,to_str) | 用字符串to_str替换字符串str中所有的字符串from_str       |
| substring(str,pos,len)       | 从字符串str的pos位置起len个字符长度的子串               |

#### 2. 日期和时间函数

| **函数**                                          | **功能**                                  |
| ------------------------------------------------- | ----------------------------------------- |
| CURDATE()                                         | 返回当前日期                              |
| CURTIME()                                         | 返回当前时间                              |
| NOW()                                             | 返回当前的日期和时间                      |
| WEEK(date)                                        | 返回指定日期为一年中的第几周              |
| YEAR(date)                                        | 返回日期的年份                            |
| HOUR(time)                                        | 返回time的小时值                          |
| MINUTE(time)                                      | 返回time的分钟值                          |
| MONTHNAME(date)                                   | 返回date的月份名                          |
| DATEDIFF(expr,expr2)                              | 返回起始时间expr和结束时间exrp2之间的天数 |
| DATE_FORMAT(date,fmt)                             | 返回按字符串fmt格式化日期date值           |
| from_unixtime(unix_timestamp,”%Y-%m-%d %H:%i:%S”) | 常用来将毫秒数转换为时间格式              |

### char和varchar的区别
    CHAR(M)定义的列的长度为固定的，M取值可以为0～255之间，当保存CHAR值时，它们的右边填充空格以达到指定的长度。
    
    VARCHAR(M)定义的列的长度为可变长字符串，VARCHAR值保存时只保存需要的字符数，另加一个字节来记录长度(如果列声明的长度超过255，则 使用两个字节)。VARCHAR值保存时不进行填充。
    
    从空间上考虑，用varchar合适；从效率上考虑，用char合适，关键是根据实际情况找到权衡点。
### 谈谈你对索引的理解
    索引的出现是为了提高数据的查询效率.对于数据库的表而言，索引其实就是它的“目录”。
    同样索引也会带来很多负面影响：创建索引和维护索引需要耗费时间，这个时间随着数据量的增加而增加；索引需要占用物理空间，不光是表需要占用数据空间，每个索引也需要占用物理空间；当对表进行增、删、改、的时候索引也要动态维护，这样就降低了数据的维护速度。
- 建立索引的原则： 在最频繁使用的、用以缩小查询范围的字段上建立索引； 在频繁使用的、需要排序的字段上建立索引。 
- 不适合建立索引的情况： 对于查询中很少涉及的列或者重复值比较多的列，不宜建立索引； 对于一些特殊的数据类型，不宜建立索引，比如：文本字段（text）等。
### MySQL的索引的数据结构
    InnoDB 存储引擎的默认索引实现为 B+ 树索引
### 为什么 InnoDB 存储引擎选用 B+ 树而不是 B 树呢？

用 B+ 树不用 B 树考虑的是 IO 对性能的影响，B 树的每个节点都存储数据，而 B+ 树只有叶子节点才存储数据，所以查找相同数据量的情况下，B 树的高度更高，IO 更频繁。数据库索引是存储在磁盘上的，当数据量大时，就不能把整个索引全部加载到内存了，只能逐一加载每一个磁盘页（对应索引树的节点）

---
# 查询
---
```sql
# 子查询
/*1. 放在where或者having后
1. 标量子查询(单行子查询)
2. 列子查询(多行子查询)
3. 行子查询(多列一行)
特点:
① 子查询放在小括号内
② 子查询一般放在条件的右侧
③ 标量子查询 一般搭配单行操作符使用:> < >= <= = <>
列子查询 一般搭配着多行操作符使用:in/any/some,all
	#④子查询的执行先于主查询
	/*
	非法使用标量子查询
			1. 多行子查询使用单行比较符
			2. 子查询不返回任何行
	*/
  任一 --->小于max
	所有 ----> 小于min 
```
---

```sql
SQL的limit分页字句:
		limit [offset],siza;
			offset 要显示条目的起始索引  (起始索引从0开始)
		  	size 要显示条目数
	语法和执行顺序都在最后 
  SELECT * from employees limit 10,15
    公式:
  limit (page-1) * size,siza

```

```sql
SQL查询语句总结:
			SELECT
				查询列表 
			FROM
				表
				连接类型
				JOIN 表 2 ON 连接条件 
			WHERE
				筛选条件 (分组前筛选) 
			GROUP BY
				分组条件 
			HAVING
				分组后的筛选条件 
			ORDER BY
				排序列表 
				LIMIT 偏移,条目数
	各个子句在 SQL 语句中的位置，可以不出现，但不得越位，否则就会报语法错误。首先是 from 语句，查出表的所有数据，接着是 select 取指定字段的数据列，然后是 where 进行条件筛选，得到一个结果集。接着 group by 分组该结果集并得到分组后的数据集，having 再一次条件筛选，最后才轮到 order by 排序。
```

### SQL的基本增删改查


```sql
#插入数据
INSERT INTO [TABLE_NAME] (column1, column2, column3,...columnN) VALUES (value1, value2, value3,...valueN);  
INSERT INTO [TABLE_NAME] SET column1=value1,column3=value3,***,column n = value n;
						方式一支持插入多行,方式二不支持
						方式一支持子查询,方式二不支持
# 删除数据
DELETE FROM [table_name]WHERE [condition];
TRUNCATE TABLE;
# TRUNCATE删除和DELETE删除的区别
				1. delete删除可以有限定条件,truncate删除不能有限定条件,直接表清空
				2. truncate删除效率高一点
				3. 如果被删除的表中有自增列,delete删除后,再插入数据时,自增长的列的值从断点开始,truncate删除后,再插入数据时,增长的列值从1开始
				4. truncate删除无返回值,delete删除返回受影响的行数
				5. truncate删除不能回滚,delete删除可以回滚
# 修改数据
UPDATE [table_name]SET column1 = value1, column2 = value2...., columnN = valueN WHERE 筛选条件
修改多表语法：
																# sql92语法
																update 表1 别名1,表2 别名2
																set 字段=新值，字段=新值
																where 连接条件
																and 筛选条件
																
																#sql99语法
																update 表1 别名1 inner|left|right join 表2 别名2
																on 连接条件
																set 字段=新值，字段=新值
																where 筛选条件						
```

---

# 函数
---

| SQL 聚合函数 |       说明       |             NULL             |
| :----------: | :--------------: | :--------------------------: |
|    AVG()     |  返回某列平均值  |             忽略             |
|   COUNT()    |  返回某列的行数  | COUNT(*)不忽略<br />其他忽略 |
|    MAX()     | 返回某列的最大值 |             忽略             |
|    MIN()     | 返回某列的最小值 |             忽略             |
|    SUM()     |   返回某列之和   |             忽略             |
|    特点：    |                  |                              |

1、以上五个分组函数都忽略null值，除了count(*)* 

*2、sum和avg一般用于处理数值型 max、min、count可以处理任何数据类型*

 *3、都可以搭配distinct使用，用于统计去重后的结果*

 *4、count的参数可以支持： 字段、*、常量值，一般放1

```
   建议使用 count(*)
```
聚合函数group_concat（X，Y），其中X是要连接的字段，Y是连接时用的符号，可省略，默认为逗号。
此函数必须与GROUP BY配合使用。

### 字符函数
```sql
-- LENGTH(str) ---> 字节数   
-- CHAR_LENGTH(str) ---> 字符数    一个汉字占3个字节
SELECT ASCII('a'),LENGTH('HELLO'),LENGTH('我们'),CHAR_LENGTH('hello'),CHAR_LENGTH('我们') FROM DUAL;
-- CONCAT ---> 字符串拼接 CONCAT_WS(separator,str1,str2,...) 以separator分隔符拼接字符串
--  INSERT(str,idx,len,replacestr) 字符串索引从1开始
-- REPLACE(str,from_str,to_str) 替换str中的from_str为to_str
-- UPPER(str)---> 转大写 
-- LOWER(str)--->转小写
SELECT CONCAT('hello','world','i','am') as "chardetail",CONCAT_WS('-','hello','world'),INSERT('hello',2,3,'aaaaaaaaa'),REPLACE('hello','ll','mm'),UPPER('abc'),LOWER('AAA') FROM dual;
-- LEFT(str,len) 取左边的len个字符
-- RIGHT(str,len) 取右边的len个字符
-- LPAD(str,len,padstr) 实现右对齐
-- RPAD(str,len,padstr) 实现左对齐
-- LTRIM(str)去除左空格
-- RTRIM(str) 去除右空格
-- TRIM(str) 去除左右空格
-- TRIM([remstr FROM] str) 去除str首尾的remstr
-- REPEAT(str,count)  str重复count次
-- SPACE(N) 提供n个空格
-- TRCMP(expr1,expr2) 比较两个字符串大小
-- SUBSTR(str,pos,len) 从字符串str第pos位开始取len位
-- LOCATE(substr,str) str中substr首次出现位置,没找到为0
	SELECT LEFT('hello',2),RIGHT('hello',3),LPAD('abc',5,'*'),RPAD('abc',5,'*'),LTRIM('          你'),RTRIM(' 好         '),CONCAT(TRIM('        你好        '),'啊'),TRIM('o' FROM 'ooooohelooo'),REPEAT('hello',4),LENGTH(SPACE(2)),STRCMP('abc','abd'),SUBSTR('abcde',2,2),LOCATE('z','hello')from DUAL;
-- ELT(N,str1,str2,str3,...) 返回参数中第n个字符串超出范围返回空  
-- FIELD(str,str1,str2,str3,...) 返回字符串str在字符串列表中第一次出现的位置 没有返回0
-- FIND_IN_SET(str,strlist) 返回字符串 str 在字符串列表 strlist 中首次出现的位置 没有返回0 strlist是一个以逗号分割的字符串
-- REVERSE(str) 翻转字符串
-- NULLIF(expr1,expr2) 比较两个 expr 若expr1与expr2相等 则返回null 否则返回expr1
SELECT ELT(100,'abc','def','ghi'),FIELD('z','b','c','a'),FIND_IN_SET('dsfs','gg,mm,张宝磊,mm') FROM DUAL;
```


### 数学函数

> round 四舍五入  <br>
> rand 随机数(0-1)无限趋近于1 <br>
>  floor向下取整  <br>
> ceil向上取整 <br>
>  mod取余<br>
>  truncate截断<br>

### 日期函数

> now当前系统日期+时间 <br>
>  curdate当前系统日期  <br>
> curtime当前系统时间 <br>
>  str_to_date 将字符转换成日期 <br>
>  date_format将日期转换成字符<br>
>  datediff 返回两个日期相差的天数<br> 
> monthname 以英文形式返回月<br>

### 流程控制函数

> if 处理双分支<br>
> case语句 处理多分支 <br>
>        情况1：处理等值判断  
>        情况2：处理条件判断

### 其他函数

> version版本<br>
> database当前库 <br>
>  user当前连接用户<br> password(‘字符’)
> 返回该字符的密码形式 后期版本已经移除该方法
> md5(“字符”);将字符以md5加密算法加密并输出<br>

---
### 创建函数

学过的函数：LENGTH、SUBSTR、CONCAT等
语法：

	CREATE FUNCTION 函数名(参数名 参数类型,...) RETURNS 返回类型
	BEGIN
		函数体
	
	END

调用函数
	SELECT 函数名（实参列表）





函数和存储过程的区别

| 名称     | 关键字    | 调用语法        | 返回值          | 应用场景                                                 |
| -------- | --------- | --------------- | --------------- | -------------------------------------------------------- |
| 函数     | FUNCTION  | SELECT 函数()   | 只能是一个      | 一般用于查询结果为一个值并返回时，当有返回值而且仅仅一个 |
| 存储过程 | PROCEDURE | CALL 存储过程() | 可以有0个或多个 | 一般用于更新                                             |



# DDL语句
---

```sql
CREATE TABLE IF NOT EXISTS stuinfo(
	stuId INT,
	stuName VARCHAR(20),
	gender CHAR,
	bornDate DATETIME
);
DESC studentinfo;
#2.修改表 alter
语法：ALTER TABLE 表名 ADD|MODIFY|DROP|CHANGE COLUMN 字段名 【字段类型】;
```

```sql
#①修改字段名
ALTER TABLE studentinfo CHANGE  COLUMN sex gender CHAR;
#②修改表名
ALTER TABLE stuinfo RENAME [TO]  studentinfo;
#③修改字段类型和列级约束
ALTER TABLE studentinfo MODIFY COLUMN borndate DATE ;
#④添加字段
ALTER TABLE studentinfo ADD COLUMN email VARCHAR(20) first;
#⑤删除字段
ALTER TABLE studentinfo DROP COLUMN email;
#3.删除表
DROP TABLE [IF EXISTS] studentinfo;

#复制表
#1.复制表的结构
create table 表名 like 旧表;
#2.复制表的结构+数据
create table 表名 select 查询列表 from 旧表[where 筛选条件]
 常见类型
**原则:所选择的类型约简单越好,能保存数值的类型越小越好**
	整型：
		      类型 | 字节 | 范围
		      ---|---|---
		      
			Tinyint  	 	 1      0-255	
	     Smallint		 2		0-65535
	     Mediumint		 3 
	     Int integer	     4
	      Bigint			 8
		
	小数：
		浮点型 double float 
		定点型   精确度高  要求插入数值的精度较高,则优先考虑
		    dec(M,D)====decimal(M,D)    M默认为10,D默认为0
		    M:整数部位+小数部位  D: 小数部位   
		    如果超过精度,则插入临界值
	字符型：
	   保存较短的文本: M:表示该类型能保存最大字符数
	   		char(M)  固定长度的字符  性能高
	   		varchar(M)  可变长度的字符
	   	保存较长的文本:
	   		text
	   		blob (较大的二进制)
	日期型：
	   date/datetime/timestamp/time/year
	   
	   datetime和timestamp的区别:
	   	  1、Timestamp支持的时间范围较小，取值范围：
	        19700101080001——2038年的某个时间
	        Datetime的取值范围： 1000-1-1 ——9999—12-31
	      2、timestamp和实际时区有关，更能反映实际的日
	        期，而datetime则只能反映出插入时的当地时区
	      3、timestamp的属性受Mysql版本和SQLMode的影响
	        很大
```

---

# 索引
---

### 索引的底层结构？

> 

---

### MySQL索引失效的几种情况：

- 如果条件中有or，即使其中有条件带索引也不会使用(这也是为什么尽量少用or的原因)要想使用or，又想让索引生效，只能将or条件中的每个列都加上索引
- 对于多列索引，不是使用的第一部分，则不会使用索引
- like查询以%开头
- 如果列类型是字符串，那一定要在条件中将数据使用引号引用起来,否则不使用索引
- 如果mysql估计使用全表扫描要比使用索引快,则不使用索引

---
```sql
1. 创建索引
	1.1 使用Alter创建索引
				1 添加主键索引
					特点：数据列不允许重复，不能为null,一张表只能有一个主键；Mysql主动将该字段进行排序
						ALTER TABLE 表名 ADD Primary key (col);
						添加唯一索引
						特点：索引列是唯一的，可以null；Mysql主动将该字段进行排序
						ALTER TABLE 表名 ADD unique <索引名> (col1, col2, ...col3);
						添加普通索引
						特点：添加普通索引， 索引值不唯一，可为null
						Alter table 表名 ADD index <索引名> (col1, col2, ...,);
						添加全文索引
						特点：只能在文本类型CHAR，VARCHAR, TEXT类型字段上创建全文索引；
						ALTER TABLE 表名 ADD Fulltext <索引名> (col)
						添加多列索引
						特点：多列是唯一的
					ALTER TABLE 表名 ADD UNIQUE (col1, col2, ..., )
					1.2 使用Create创建索引
					语法：create index 索引名 on 表名(字段)		
			添加唯一索引
				create index 索引名 on table 表名(col1, col2, ..., )
				添加普通索引
				create unique index 索引名 on table 表名(col1, col2, ..., )
				1.3 两种创建索引方式的区别
				Alter可以省略索引名。如果省略索引名，数据库会默认根据第一个索引列赋予一个名称；Create必须指定索引名称。
				Create不能用于创建Primary key索引；
				Alter允许一条语句同时创建多个索引；Create一次只能创建一个索引	
				ALTER TABLE 表名 ADD Primary key (id), ADD index <索引名> (col1, col2, ...,)
				1.4 索引执行效率分析
				主键索引 > 唯一性索引 > 普通索引
				2 删除索引
				第一种方式
				drop index 索引名 on 表名;
				第二种方式
				Alter table 表名 drop index 索引名;
				第三种方式	
				Alter table 表名 drop primary key
分析：

				第三种方式只在删除primary key中使用。因一个表只能存在一个primary key索引，则不需要指定索引名；
				对于第三种方式，若没有创建primary key索引，但表中具有一个或多个unique索引，则默认删除第一个unique索引；
				若删除表中的某列，索引会受到影响。对于多列组合的索引，如果删除其中的某一列，则该列会从对应的索引中被删除(删除列，不删除索引)；多删除组成索引的所有列，则索引将被删除(不仅删除列，还删除索引)。</索引名></索引名></索引名></索引名>
```

---
# 存储过程
---
含义: 一组预先编译好的SQL语句集合(批处理语句)
语法:
```sql
create procedure 存储过程名(in|out|inout 参数名  参数类型,...)
begin
	存储过程体

end
```
---

# 约束
---
### 常见约束

	NOT NULL    不为空
	DEFAULT     默认值
	UNIQUE      唯一
	CHECK       检查约束(MySQL不支持)
	PRIMARY KEY 主键  保证该字段的值具有唯一性,且不为空
	FOREIGN KEY 外键  限制两个表的关系 (从表设置 类型一致或兼容 主表中的关联列必须是key:主键/唯一键)

---
###### 添加约束的时机:创建表时/修改表时(表中没有数据时)
---
### 主键和唯一的对比
1. 主键和唯一都保证了字段的唯一性
2. 主键非空,唯一可以有一个null
3. 主键一个表中至多有一个,唯一一个表中可以有多个
4. 都支持组合,但不推荐
---

# 事务
---

### 事务的四大特性

> （ACID）<br>
> 原子性：要么都执行，要么都回滚<br>
> 一致性：保证数据的状态操作前和操作后保持一致<br>
> 隔离性：多个事务同时操作相同数据库的同一个数据时，一个事务的执行不受另外一个事务的干扰<br>
> 持久性：一个事务一旦提交，则数据将持久化到本地，除非其他事务对其进行修改<br>
---
### 开启显式事务
---
```sql
    #开启显式事务
    # 前提:必须先设置自动提交功能为禁用
    set autocommit = 0;
    START TRANSACTION; # 可选
    # 2. 编写事务中的SQL语句(SELECT||INSERT||UPDATE||DELETE)
    语句1;
    语句2;
    ...
    # 3. 结束事务
    commit;提交事务 OR
    rollback;回滚事务
```
### 事务的隔离级别
事务并发问题如何发生？

	当多个事务同时操作同一个数据库的相同数据时
事务的并发问题有哪些？

	脏读：一个事务读取到了另外一个事务未提交的数据
	不可重复读：同一个事务中，多次读取到的数据不一致
	幻读：一个事务读取数据时，另外一个事务进行更新，导致第一个事务读取到了没有更新的数据
> 如何避免事务的并发问题？

	通过设置事务的隔离级别
	1、READ UNCOMMITTED 读未提交
	2、READ COMMITTED   读已提交  可以避免脏读
	3、REPEATABLE READ  可重复读 可以避免脏读、不可重复读和一部分幻读(MySQL默认)
	4、SERIALIZABLE     序列化 可以避免脏读、不可重复读和幻读

![image](https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/java/image/clipboard.png)
设置隔离级别：

```sql
set session|global  transaction isolation level 隔离级别名;
```
查看隔离级别：

```sql
select @@tx_isolation;
```

---

---

# 视图
---
### 视图的概念
含义：理解成一张虚拟的表

视图和表的区别：
	
| 类型 | 使用方式 | 占用物理空间                |
| ---- | -------- | --------------------------- |
| 视图 | 完全相同 | 不占用，仅仅保存的是sql逻辑 |
表|    完全相同|	占用

视图的好处：


	1、sql语句提高重用性，效率高
	2、和表实现了分离，提高了安全性

#### 视图的创建
```sql
语法：
CREATE VIEW  视图名
AS
查询语句;
```
---

### 表和视图的区别和联系/常见应用场景

1. 区别:

    > 数据都是存储在表里面的，而不是视图表是创建好的，真实存在的，而视图只是一段已经编译好的sql语句视图只是窗口，而表则是里面的数据表占有真实的物理空间，而视图只是一个逻辑概念表是内模式，视图是外模式（内模式又称存储模式，对应于物理级，它是数据库中全体数据的内部表示或底层描述。外模式又称子模式，对应于用户级。它是某个或某几个用户所看到的数据库的数据视图，是与某一应用有关的数据的逻辑表示。）视图是查看数据表的一种方法，可以查询到表中用户想知道的数据，并且还不知道表结构，比较安全表可以增删改查，视图只能查视图的创建和删除不影响表

2. 联系:

     视图一般都是建立在表的基础之上的，它的所有内容全部都来自于表，可能是一个表，也可能是多个表，视图是基本表的抽象和在逻辑意义上建立的新关系。

3. 视图的常见应用场景
    - 应用场景1：保密工作，比如有一个员工工资表，如果你只希望财务看到员工工资这个字段，而其他人不能看到工资字段，那就用一个视图，把工资这个敏感字段过滤掉
    - 应用场景2：有一个查询语句非常复杂，大概有100行这么多，有时还想把这个巨大无比的select语句和其他表关联起来得到结果，写太多很麻烦，可以用一个视图来代替这100行的select语句，充当一个变量角色

---

# 流程控制
---
### if函数
```sql
语法：if(条件，值1，值2)
特点：可以用在任何位置
```
### case语句

语法：

```sql
情况一：类似于switch
case 表达式
when 值1 then 结果1或语句1(如果是语句，需要加分号) 
when 值2 then 结果2或语句2(如果是语句，需要加分号)
...
else 结果n或语句n(如果是语句，需要加分号)
end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）

情况二：类似于多重if
case 
when 条件1 then 结果1或语句1(如果是语句，需要加分号) 
when 条件2 then 结果2或语句2(如果是语句，需要加分号)
...
else 结果n或语句n(如果是语句，需要加分号)
end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）
```

特点：
	可以用在任何位置
### if elseif语句

语法：

```sql
if 情况1 then 语句1;
elseif 情况2 then 语句2;
...
else 语句n;
end if;
```

特点：
	只能用在begin end中！


三者比较：
| 名称 | 应用场合 |
| ---- | -------- |
if函数	|	简单双分支
case结构|	等值判断的多分支
if结构	|区间判断的多分支

---
### 循环

语法：


	【标签：】WHILE 循环条件  DO
		循环体
	END WHILE 【标签】;

特点：

	只能放在BEGIN END里面
	
	如果要搭配leave跳转语句，需要使用标签，否则可以不用标签
	
	leave类似于java中的break语句，跳出所在循环！！！


# 存储引擎
---

# MySQL的存储引擎

**InnoDB**：InnoDB是一个健壮的事务型存储引擎。支持外键、支持自动增加列AUTO_INCREMENT属性。

**MyISAM**：它不支持事务，也不支持外键，尤其是访问速度快，对事务完整性没有要求或者以SELECT、INSERT为主的应用基本都可以使用这个引擎来创建表

**MEMORY**：速度快，采用的逻辑存储介质是系统内存

**MERGE**：MERGE存储引擎是一组表结构完全相同的MyISAM表的组合

**ARCHIVE**：仅仅支持最基本的插入和查询两种功能，Archive拥有很好的压缩机制
### MySQL字符串函数

### MySQL日期/时间类型的函数
```sql
# 日期和时间函数
## 获取日期时间 
# 当前日期 CURDATE(),CURRENT_DATE() 当前时间 CURTIME() 当前日期+时间 NOW(),SYSDATE() 格林威治日期 UTC_DATE() 格林威治时间UTC_TIME()
SELECT CURDATE(),CURRENT_DATE(),CURTIME(),NOW(),SYSDATE(),UTC_DATE(),UTC_TIME()
FROM dual
# 日期与时间戳的转换
-- UNIX_TIMESTAMP() 获取当前时间戳
-- FROM_UNIXTIME(unix_timestamp) 将指定时间戳转换为普通格式的时间
-- UNIX_TIMESTAMP(date) 获取指定时间的的时间戳
SELECT UNIX_TIMESTAMP() ,FROM_UNIXTIME(1640761045),UNIX_TIMESTAMP('2021-10-01 12:12:21')FROM DUAL;
# 结论: SELECT中出现的非组函数的字段必须声明在group by中,反之,GROUP BY中声明的字段可以不出现在select中
# 结论: GROUP BY 声明在 FROM 后面/WHERE 后面,ORDER BY 语句前面 LIMIT语句前面
# 结论 MySQL中在group by中使用with ROLLUP 将整个表数据看成一个组
```
## 覆盖索引
- 什么是覆盖索引
   >查询的内容和条件都在联合索引树里，因为联合索引树的叶子节点包含「索引列+主键」，所以查联合索引树就能查到全部结果了，这个就是覆盖索引

## 执行一条查询语句的流程

-  MySQL 是基于 TCP 协议进行传输的 经过TCP三次握手完成与**连接器**的连接,连接器负责用户名密码的效验,在用户名密码无误之后,连接器就会获取该用户的权限，然后保存起来，后续该用户在此连接里的任何操作，都会基于连接开始时读到的权限进行权限逻辑的判断
- MySQL 就会先去**查询缓存器（Query Cache）**里查找缓存数据，看看之前有没有执行过这一条命令，这个查询缓存是以 key-value 形式保存在内存中的，key 为 SQL 查询语句，value 为 SQL 语句查询的结果. 缓存本身作用不大 MySQL 8.0 版本直接将查询缓存删掉
- MySQL 会将 SQL 交到**解析器**完成解析,解析器完成词法分析(识别关键字,构建SQL语法树) --> 语法分析(判断 SQL 语句是否满足 MySQL 语法)
- MySQL将SQL交到**预处理器**完成预处理操作,检查 SQL 查询语句中的表或者字段是否存在并且将 `select *` 中的 `*` 符号，扩展为表上的所有列
- 经过预处理阶段后，还需要为 SQL 查询语句先制定一个执行计划，**优化器主要负责将 SQL 查询语句的执行方案确定下来** 如查询使用哪个索引 
- **执行器**与存储引擎交互,根据执行计划执行 SQL 查询语句，从存储引擎读取记录，返回给客户端