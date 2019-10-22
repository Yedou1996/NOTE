## 2019.8.18基础.条件.模糊查询

```
	基础查询

SELECT xq FROM t_cxfz          						/*查询单个字段*/

SELECT xq,zm FROM t_cxfz        					/*查询多个字段*/
 
SELECT * FROM t_cxfz            					/*查询所有字段*/

SELECT 52  FROM t_cxfz             					 /*查询常量值*/        

SELECT '抢劫罪' FROM t_cxfz						  /*查询常量值*/ 

SELECT 100%98										/*查询表达式*/

SELECT VERSION()                   					 /*查询函数*/

SELECT xq AS 刑期,zm AS 罪名 FROM t_cxfz          		/*起别名*/

SELECT xq 刑期,zm 罪名 FROM t_cxfz               		/*起别名简写*/

SELECT xq AS "out put" FROM t_cxfz                		/*有特殊符号时起别名*/

SELECT DISTINCT nl FROM t_cxfz						   /*查询所有的年龄，去重*/

SELECT concat(nl,',',xq,zm) AS 刑期罪名 FROM t_cxfz        /*多字段拼接*/


	条件查询
	
SELECT * FROM t_cxfz WHERE nl>60           /*查询年龄大于60的*/

SELECT xl AS 学历,nl AS 年龄 FROM t_cxfz WHERE nl !=60      /*查询学历和年龄不等于60的*/

SELECT xl AS 学历,nl As 年龄,lgf AS 劳改犯 FROM t_cxfz WHERE nl>30 AND nl <40     /*查询年龄大于30小于40的*/

SELECT xl,nl,lgf FROM t_cxfz WHERE  NOT(nl>40 OR nl<30) OR nl =45                /*查询年龄不在40和30岁之间或者等于45的人*/



	模糊查询
	
SELECT * FROM t_cxfz WHERE xq like '%十年%'                  /*查询字段中包含某个关键字*/

SELECT xl,nl,lgf FROM t_cxfz WHERE xl IN('小学教育','半文盲')   /*查询某个字段内有相关关键字的*/


SELECT xl,nl,lgf FROM t_cxfz WHERE lgf is null                   /*查询某个字段为空的内容*/

SELECT * FROM t_cxfz WHERE lgf is not NULL                       /*查询某个字段不为空的*

select ID,POLICE_NO FROM gate_inner_staff where POLICE_NO like 'A10___'   /*模糊查询下划线代表模糊*/

select IFNULL(CREATE_BY,55) from gate_inner_staff                 如果为空就替换

% 任意多个字符     _任意单个字符 


	

SELECT DISTINCT
	c_code,c_name 
FROM
	db_zf.t_zfjbxx AS zf
	LEFT JOIN PUBLIC.t_aty_code AS code1 ON zf.c_mz = code1.c_code
	
	
	
	SELECT
 c_zfbh,
 code1.c_name AS mz,
 code2.c_name AS zzmm,
 code3.c_name AS fgdj,
 code4.c_name AS bqxl,
 code5.c_name AS xb,
 code6.c_name AS zybs,
 c_bmbh,
 c_bmmc 
FROM
 db_zf.t_zfjbxx AS zf
 LEFT JOIN PUBLIC.t_aty_code AS code1 ON zf.c_mz = code1.c_code 
 AND code1.c_pid = '10000002' --民族
 LEFT JOIN PUBLIC.t_aty_code AS code2 ON zf.c_bqzzmm = code2.c_code 
 AND code2.c_pid = '10000023' --政治面貌
 LEFT JOIN PUBLIC.t_aty_code AS code3 ON zf.c_fgdj = code3.c_code 
 AND code3.c_pid = '10000006' --分管等级
 LEFT JOIN PUBLIC.t_aty_code AS code4 ON zf.c_bqxl = code4.c_code 
 AND code4.c_pid = '10000024' --学历
 LEFT JOIN PUBLIC.t_aty_code AS code5 ON zf.c_xb = code5.c_code 
 AND code5.c_pid = '10000001' --性别
 LEFT JOIN PUBLIC.t_aty_code AS code6 ON zf.c_zybz = code6.c_code 
 AND code6.c_pid = '10000008' --在押标识

```

## 	2019.8.18排序查询和常见函数

```
	排序查询
	
	
select * from db_test.t_cxfz order by nl asc         			  顺序查询         

select * from db_test.t_cxfz ORDER BY nl desc					 倒序查询

select * from db_test.t_cxfz where nl>40 order by nl asc           升序查询年龄大于40的罪犯

SELECT * from db_test.t_cxfz order by length(xq)                   按字符长度排序

select * from db_test.t_cxfz order by nl asc, nf desc;             按年龄排序完再按年份排

select nl,xl from db_test.t_cxfz WHERE nl>=30 and nl<=40 ORDER BY nl        查询年龄在30和40岁之间按年龄排序

select * from db_test.t_cxfz where xl like '%小学%' order by nl         查询学历中包含‘小学’的并按年龄排序


	常见函数
一、字符函数
	select length('jeff')                               长度函数                   
	select concat(nl,'zm',zm) from db_test.t_cxfz       拼接函数
	select upper('lxl')								 小写转大写
	select lower('LXL')								 大写转小写
	select substr('我最喜欢吃东西了',5)                  截取函数         
	select substr('我最喜欢吃东西了',1,4)			    截取函数
	select instr('我最炫吃东西了','东西') as output      返回子串出现的索引
	select trim('         loubaba        ')            去除前后的内容
	select trim ('baba' from  'bababababababaLXLbababababbaba')    去除前后相同的重复内容
	select LPAD('娄爸爸',10,'*')                      用指定的字符实现左填充指定长度
	select RPAD('娄爸爸',10,'*')                      用指定的字符实现右填充指定长度
	select replace('娄爸爸喜欢玩游戏','玩游戏','吃东西')		替换
	



```

## 2019.8.23数学.日期.if.case函数

```
	数学函数
select round(1.5)          						 四舍五入

select round(1.234,2)        					 小数点后保留几位

select ceil(1.45)          						 向上取整

select floor(-9.99)     						 向下取整

select truncate(1.69999,1)       				  截断

select mod(10,3)								 取余
SELECT FLOOR(7 + (RAND() * 6));           在7 到 12 的范围（包括7和12）内得到一个随机整数

round


	日期函数
select now()                   返回当前的时间+日期

select curdate()               返回当前的日期不包含时间

select curtime()			  返回当前的时间，不包含日期

select year(now())              返回年

select year('1997-7-8')       	返回年	

SELECT monthname(now())         返回月的英文名


	流程控制函数if，case
select ext1,if(ext1 is null,'6666','777') from gate_foreign_car_collect
   相当于三元表达式
   
   相当于switch ， case。。
  select nl as 年龄,case nl 
 when 20 then nl+20
 when 30 then nl+20
 when 60 then nl+20
 else nl
 end as 新年龄
 from db_test.t_cxfz
 
 select nl,
 case
 when nl>60 then '年轻'
 when nl>40 then  '一般般'
 when nl>20 then '老了'
 else 'hahah'
 end as 哈哈
 from db_test.t_cxfz
 
  
```

##  2019.8.26分组函数

```
	分组函数
 select count(nl),sum(nl),round(avg(nl),2),min(nl),max(nl)from t_cxfz  求和，计数
 
  select count(zm),count(nl),count(*) from db_test.t_cxfz     count(*)统计行数
  
  select count(*) from db_test.t_cxfz where nl >60     		统计年龄大于60的个数
  
  select max(nl),zm  FROM db_test.t_cxfz GROUP BY zm       查询每个罪名的最大年龄
   
  select count(*),zm from db_test.t_cxfz GROUP BY zm       查询每个罪名的人数
    
 select ceil(avg(nl)),zm from db_test.t_cxfz where xl like '%小%' group by zm
 
  select count(*),zm from db_test.t_cxfz GROUP BY zm HAVING count(*)>2   查询罪名人数大于2的
  
   select ceil(avg(nl)),zm from db_test.t_cxfz WHERE zm is not null GROUP BY zm HAVING avg(nl)>40    查询每个平均年龄>40罪名不为空的罪犯
   分组函数做条件肯定放在having语句中
   
```

## 2019.8.27连接查询（多表查询）



```
		等值连接：
	多表等值连接的结果为多表的交集部分
	多表的顺序没有要求
	一般需要为表取别名
select name,boyname from boys,beauty where beauty.boyfriend_id=boys.id   id关联
	
		非等值连接：
select salary,grade_level from employees e,job_grades g where salary between g.lowest_sal and g.highest_sal             比较两个字段给出等级。

		自连接：
select e.employee_id,e.last_name,m.employee_id,m.lastname from employees e,employees m where e.manager_id=emplorees_id;    同一个表内的不同字段
	 
78
```



## 2019.9.9 sql99语法

```
	等值连接		内
select last_name,department_name from employees e inner join departments d on e.department_id = d.pepartment_id  				 查询员工名，部门名

select last_name,job_title from employees e inner join jobs j on e.job_id = j.job_id where e.last_name like '%e%'			查询名字中包含e的员工名和工种名

 
select conunt(*) city 部门个数 from departments d inner join locations l on d.location_id = l.location_id group by city having count(*)>3 查询部门个数>3的城市名和部门个数

select count(*),deparment_name from employees d inner join departments d on e.department_id = d.department_id group by department having conut(*)>3 order by count(*) desc   查询每个部门的员工个数>3的部门和员工个数，并按个数排序

	非等值连接
select salary,grade_level from employees e inner join job_grades g on e.salary between g.lowest_sal and g.highest_sal  查询员工的工资级别

	自连接
select e.last_name,m.last_name from employees e inner join employees m on e.manager_id = m.employee_id    查询员工的名字和上级的名字

	
	
	
	外连接
外连接查询的结果为主表中的所有结果，有匹配的就显示匹配的结果，没有匹配就显示null

	左外连接
select b.name,bo.*
from boys bo
left outer join beauty b
on b.boyfriend_id =bo.id
where bo.id is null
	全外连接
select b.*,bo.*
from beauty b
full outer join boys bo
on b.boyfriend_id = bo.id
	交叉连接
selelct b.*,bo.*
from beauty b
cross join boys bo   笛卡尔乘积

	sql92和sql99
99支持多，可读性好 



	子查询
含义：出现在其他语句中的select语句
特点：1.放在小括号内，2，条件右侧

	标量子查询
select salary from employees where last_name = 'Abel'    查询员工Abel的工资

select * from employees where salary>(
	select salary
	from employees
	where last_name = 'Able'
)        查询员工信息，满足salary大于Abel


select MIN(salary) from employees 查询最低工资

select last_name,job_id,salary 
from employees
where salary=(
	select MIN(salary)
	from employees
)               查询工资最低人的信息
	
	
	列子查询
select distinct department_id from departments where location_id in(1400,1700)
查询员工location_id是1400或1700的部门编号

 select last_name
 from employees
 where department_id in(
 	select distinct department_id
 	from departments
 	where location_id in(1400,1700)   返回location_id是1400或1700的部员工姓名
 )
 
 
 select d.*,(
 	select count(*)
 	from employees e
 	where e.department_id = d.department_id
 )
 from departments d; 查询每个部门的员工个数
 
 
 select avg(salary),department_id 
 from employees
 group by department_id  查询每个部门的平均工资
 
 
 select ag_dep.*,g.grade_level
 from(
 	select avg(salary),department_id
 	from employees
 	group by department_id
 ) avg_dep
 inner join job_grades g
 on ag_dep.ag between lowest_sal and highest_sal   查询每个部门平均工资的等级
 
 
 exists(完整的查询语句)
 
 
```

## 	2019.9.10分页查询

```
	分页查询
SELECT * FROM db_test.t_zfjbxx  limit 10 OFFSET 5 查询10条从第5条开始 postgre
select * from db.test.t_zfjbxx limit 10,15   查询10到15条 mysql 15是每页显示的条数
放在执行语句的最后

	分页sql
select * from test limit (page-1)*size,size



select substr(email,1,instr(email,'@'-1)) from stuinfo 
查看所有邮箱的用户名 mysql


		查询语句中涉及的所有关键字，以及先后执行顺序
select 查询列表
from 表
连接类型 join 表2
on 连接条件
where 筛选条件
groupby 分组列表
having 分组后的筛选
order by 排序的列表
limit 偏移，条目数

```

## 2019.9.11联合查询

```
	联合查询
union：将多条查询语句的结果合并成一个结果
	要查询的内容来自多个表，多个表没有关联时
	特点：
	查询列数要求一致
	联合查询会去除重复项，使用union all 不去重
```

## 2019.9.11插入语句

```
	插入语句
insert into db_test.t_culture(code,name) VALUES('12','你爹地')

insert into db_tst.t_culture SET code='66',name='小老鼠'  		mysql适用

insert into stu(id,name,sex)
select id,name,sex from boys    		支持插入查询出来的语句
```

## 2019.9.12修改语句

```
	修改语句
update stu set name='dadaada' WHERE id=1   更新id为1的名字为dadaada

	删除语句
delete from stu where id=1          删除id为1的行
delete from stu where phone='%9'    删除手机号以9结尾的行
truncate table boys                 不能加连接条件，清空表
truncate 和 delete的区别：
	1.truncate不能加where条件
	2.truncate效率更高
	3.数据表中如果有自增长列，delect删除后会从断点开始，truncate删除会从1开始
	4.truncate删除不可以回滚，delete删除可以回滚

	DDL数据定义语言

	库的管理
create database books                      创建books数据库
ALTER database books CHARACTER set gbk     改变数据库books的编码为gbk
drop database books                        删除库

	表的管理
	create table books(
		id int,
		bname char                         创建
	)
alter table 表名 add|drop|modify|change column 列名 列类型

	drop table book                         删除表
	
	
	表的复制
create table copy like book                               复制表的结构
create table copy2 select * from book                     辅助结构和数据
create table copy3 select id,name from book               辅助部分数据
create table copy4 select id,name from book where 1=2     辅助部分结构


```

## 2019.9.19函数类型

```
	常见的数据类型
整型：
	1.如果不设置无符号还是有符号，默认有符号，如果要设置无符号，要添加unsigned，正数
	2.如果插入的数值超过了整型范围，会插入临界值
	3.如果不设置长度，会有默认长度
	4.长度代表显示的最大宽度，如果不够，会用0填充，但不许搭配zorefill来使用
	5.mediumint，比INT小，比SMALLINT大，取值范围为：-8388608到8388607，无符号的范围是0到        16777215。
浮点型：
	float（M，D）
	double（M，D）
定点型：
	decimal(M,D)  
    
	M表示总的位数，D代表小数点后的位数，M和D都可以省略,decimal省略的话默认是（10,0）
字符型：
	较短的文本：
		char（M）     M代表字符数可以省略，默认1                 固定长度     性能更好
		vachar（M）   M代表字符数，不可以省略                    可变长度
	较长的文本：
		test


```

## 2019.9.25约束

```
查询语句的结构：DQL  数据库查询语言
	select 查询列表                          7
	from 表1 别名                            1
	连接类型 join 表2                        2
	on 连接条件                              3
	where 筛选                               4
	group by 分组列表                         5
	having 筛选                              6
	order by 排序列表                        8
	limit 起使条目索引，条目数；               9

数据操作语言DML：
	insert into 表名（字段名...） values(值.....)
	insert into 表名 set 字段=值，字段=值
	update 表名 set 字段=值，字段=值 【where筛选条件】
	update 表1 left|right|inner join 表2 别名 set 字段=值，字段=值【where筛选】
	delete from 表名 【筛选条件】
	delete 表1，表2 from 表1 别名 inner|left|right join 表2  on 连接条件
	truncate table 表名
数据定义语言DDL：
	create database 库名 [if not exists]
	drop database 库名
	create table 表名{
        字段名 字段类型 【约束】，
        字段名 字段类型 【约束】
	}
	alter table 表名 add column 列名 类型；   添加列
	
约束：
	not null
	default               保证有默认值
	primary key
	unique                唯一性，可以为空
	check                 mysql不支持，
	foreign key           用于限制两个表的关系，用于保证该字段的值必须来自于主表的关                           联列的值，在从表添加限制约束，用于引用主表中某列的值
	
	
	
```

## 2019.9.27约束和事务

```
创建表时添加约束
	create table stuinfo(
		id int PRIMARY KEY,
		stuName VARCHAR(20) not null,
		gender CHAR(1) check(gender='男' or gender='女'),
		seat int UNIQUE,
		age int DEFAULT 18,
		majorId int,
	CONSTRAINT fk_stuinfo_major foreign key(majorId) references major(id)
)
外键一般在表后创建

auto_increment  自增长

主键和唯一的对比：
	1.都能保证唯一性，
	2.唯一允许为空
	3.主键一个表只能有一个，唯一可以有多个
	
	
修改表时添加约束：
	列：alter table 表名 modify column 字段名 字段类型 新约束；
	表: alter table 表名 add constraint 约束名；
	

标识列：
	create table tab_identity(
  		id int PRIMARY KEY auto_increment,     自动增长
		name VARCHAR(20)
)一个表只能有一个自增长列，类型只能是数值型


TCL（transaction Control language）事务控制语言：
	事务：
		一个或一组sql语句组成一个执行单元，这个执行单元要么全部执行，要么全部不执行。
		特点：
		1.原子性，要么都发生，要么都不发生。
		2.一致性：数据可靠，
		3.隔离性：一个事务的执行不能被被其他事务干扰
		4.持久性：一个事务一旦被提交，它对数据库中的数据改变就是永久性的
	事务的创建：
		如果要创建显示事务，必须先把set autocommit = 0；
	开启事务：
		步骤1：开启事务
		set autocommit=0;
		start transaction;可选的
		步骤2：编写事务中的sql语句（select insert update delete）
		语句1；
		语句2；
		。。。
		步骤3；结束事务
		commit；提交事务
		rollback；回滚事务
		

事务并发：
	脏读：
		对于两个事务t1和t2，t1读取了已经被t2更新的数据但还没提交的数据后，t2回滚，t1          读取的数据就是临时且无效的。
	不可重复读：
		对于两个事务t1和t2，t1读取了一个字段，然后t2更新了该字段之后，t1再次读取同一          个字段值就不同了。
	幻读：
		对于两个事务t1,t2,t1从一个表中读取了一个字段，然后t2在该表中插入了一些新的行
		之后，如果t1再次读取同一个表，就会多出几行。
	避免这些问题可以设置事务的隔离级别，mysql支持4种，默认的是REPEATABLER READ
	
事务的隔离级别
	read uncommitted 都会出现
	read committed	 可以避免脏读          oracle默认隔离级别
	repeatable read   可以避免脏读和重复读   mysql默认
	serializable      都可以避免
查看隔离级别
	select @@tx_isolation
设置隔离级别
	set session|global transaction islolation level 隔离级别
	savepoint 设置回滚点，搭配rollback使用。
		savepoint a；
		rollback to a;
	
```

## 2019.9.29视图

```
视图定义：
	一种虚拟存在的表，只保存了sql逻辑，不保存查询结果。
	create view myv1
	as
	select last_name,department_name,job_title
	from employees e
	join departments d on e.department_id = d.department_id
	join jobs j on j.job_id = e.job_id
		查询姓名中包含a字符的员工、部门名和工种信息
	select * from myv1 where last_name like '%a%';
	
	
	查询部门最高工资高于12000的部门信息：
	1.查询部门最高工资并且大于12000
	create or replace view emp_v2
	as
	select max(salary),departmen_id
	from employees
	group by department_id
	having salary >12000;
	2.select d.*,max(salary)
	from departments d
	join(
	select max(salary),departmen_id
	from employees
	group by department_id
	having salary >12000
	) m
	on m.department_id = d.depaertment_id
	视图法:
	select d.*,e.max(salary)
	from departments d
	join emp_v2 e
	on e.department_id = d.department_id
	
	视图可以插入，更新，删除，也会在真实表更新。真实表默认其他字段为null
	
```

## 2019.10.8变量

```
事务执行时，delete可以回滚，truncate不能回滚。

系统变量：
	1.查看所有的系统变量：
		show global|【session】 variables;
	2.查看指定的某个系统变量的值：
		select @@global|【session】.系统变量名；
	3.为某个系统变量赋值：
		set global|【session】 系统变量名 = 值；
		set @@session.tx_isolation='read-uncommitted';
	4.查看隔离级别：
		select @@global.tx_isolation
		
自定义变量：
	只针对会话变量
	1.声明并初始化
		set @用户变量名=值；或
		set @用户变量名:=值；或
		select @用户变量名:=值；
	2.赋值
		 通过 set或select
			set @用户变量名=值；或
			set @用户变量名:=值；或
			select @用户变量名:=值；
		通过
		
	案例：
	声明并初始化：
	set @name='join'
	set @name=100;  
	set @count=1;
	赋值：
	select count(*) into @count
	from employees;
	查看：
	select @count；
	 
	
```

## 2019.10.9存储过程和函数

```
存储过程：
	一组预先编译好的sql语句集合，理解成批处理语句。
	提高代码重用性，简化操作，减少编译和连接数据库服务器的次数，提高效率。
创建语法：
	create procedure 存储过程名(参数列表)
	begin
		存储过程体（一组合法的sql语句）
	end
	注意：
	1.参数列表包含三部分
	参数模式 参数名 参数类型
	举例：
	in   stuname  varchar(20)
	参数模式：
	in：改参数作为输入，需要调用方传入值
	out：作为输出，作为返回值
	inout：该参数起可以作为输入，也就是参数既需要传入值，又可以返回值
	2.如果存储过程体只有一句话，begin end 可以省略，存储过程sql语句每条结尾加分号，存      储过程的结尾可以使用delimiter重新设置。
	语法：
	delimiter 结束标记。
	
	调用语法：
	call 存储过程名(实参列表);
	
	
	练习：
	1.插入到admin表中3条记录； 空参
	delimiter $                 结束标记
	create procedure myp1()
	begin
		insert into admin(username,'password')
		valuses('join','0000'),('aasd','0000'),('tom','0000');
	end $
	调用：
		call myp1()$
	
	
	2.创建带in模式的存储过程： 
	(1).创建存储过程实现根据女神名，查询对应的男生信息：
	create procedure myp2(in beautyName varchar(20))
	begin
		select bo.*
		from boys bo
		right join beauty b on bo.id = b.boyfriend_id
		where b.name=beautyName;
	end $
	调用：
	call myp2('小昭')$
	
	(2)创建存储过程实现用户是否登录成功：
		create procedure myp3(in username varchar(20),in passwed varchar(20))
		begin
			declare result int default default 0; 声明并初始化变量
			selsect count(*) into result           赋值
			from admin
			where admin.username=username
			and admin.password = password;
			select if(result>0,'成功'，'失败')；     使用
		end $
		调用：
		call myp3('张飞'，'8888'）$
		

	函数创建：
	create function 函数名（参数列表）returns 返回类型
	begin
		函数体
	end
	注意：
	参数列表包含两部分：
	参数名，参数类型
	调用方法：
	select 函数名（参数列表）
```

## 2019.10.10函数和分支结构.循环

```
案列：返回公司的员工个数
	create function myf1() returns int
	begin
		declare c int default 0;定义变量
		select count(*) into c  赋值
		from employees;
		return c;
	end $

案列：
	创建一个函数，实现传入两个float，返回二者之和
	create function test_fun1(floa1 float,floa2 float) returns float
	befin
		declare sum double default 0;
		set sum=float1+float2;
		return sum;
	end $
	select test_fun1(1,2)$
	

分支：
	if（表达式1，表达式2，表达式3）
执行顺序：
	如果表达式1成立，则返回表达式2的值，否则返回表达式3的值。

	case
	1.类似于java的switch语句，一般用于实现等值判断
	语法：
		case 变量|表达式|字段
		when 要判断的值 then 返回的值1
		when 要判断的值 then 返回的值2
		。。。
		else  要返回的值n
		end
	2.类似于java中的多重if语句，一般用于实现区间判断
	语法：
		case 
		when 要判断的条件1 then 返回的值1
		when 要判断的条件2 then 返回的值2
		。。。
		else  要返回的值n
		end

创建存储过程，根据传入的成绩，来显示等级
create procedure test_case(in score int)
begin
	case
	when score>=90 and score<=100 then select 'A';
	when score>=80 then select 'B';
	when score>=60 then select 'C';
	else select 'D';
	end case;
end 
call test_case(90)


if结构：实现多重分支
	语法：
	if 条件1 then 语句1；
	elseif 条件2 then 语句2；
	。。。
	【else 语句n；】
	end if；
	应用在begin end中；
	创建函数，根据传入的成绩，来显示等级
create function test_if(score int) returns char
begin
	if score>90 and score<=100 then return 'A';
	elseif score>=80 then return 'B';
	elseif score>=60 then return 'C';
	else return 'D';
	end if;
end
call test_if(80)


循环：
	语法：
		【标签：】while 循环条件 do
				循环体；
		end while【标签】；
	案列：批量插入，根据次数插入到admin表中多条数据,大于20次跳出
	create procedure test_while(in num int)
     begin
     	declare i int default 1;
     	a:while i<num do
     		insert into admin(username,password) values(concat('join',i),'3423');
     		if i>=20 then leave a;
     		end if;
     		set i=i+1;
     		end while;
     end
     call test_while(100)
  
	
案例：已知表stringcontent
	其中字段：
	id 自增长
	content varchar（20）
	向该表插入指定个数的随机的字符串
	
	create procedure test_randstr_insert(in insertCount int)
begin
	declare i int default 1; #定义一个循环变量i，表示插入次数
	declare str varchar(26) default 'abcdefjhijklmnopqlstuvwxyz';
	declare startIndex int default 1;#代表起使索引
	declare len int default 1; #代表截取的字符的长度
	while i<insertCount do
		set len = FLOOR(RAND()*(26-startIndex+1)+1);#产生一个随机的整数，代表截取的长度，1-（26-stratIndex+1）
		set startIndex= FLOOR(RAND()*26+1);#产生一个随机整数，代表起使索引1-26
		INSERT into stringcontent(content) values(substr(str,startIndex,len));
		set i=i+1;  #循环变量更新
	end while;
end

call test_randstr_insert(100)

```









