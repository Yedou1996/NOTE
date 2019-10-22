## 2019.10.11

- ```
  linux下安装mysql5.6：
  	
  	1.下载包，只需要下载server和client包，也可以直接下载bundle包解压。
  	2.tar -xvf 解压
  	3.rpm -qa|grep mariadb   查看是否安装mariadb
  	4.rpm -e --nodeps mariadb-libs-1:5.5.56-2.el7.x86_64     强制删除mariadb
  	5.rpm -ivh  安装客户端和服务端，先安装server包
  	6.mysql --version ，ps -ef|grep mysql  查看是否安装成功  
  	7.service mysql start    启动mysql
  	8.service mysql stop     停止mysql
  	9.netstat -lnp|grep 3306    查看是否监听端口3306
  	10.find / -name .mysql_secret   查找随机密码文件地址并查看
  	11.mysql -u root -p             登录mysql
  	12.SET PASSWORD=password('password');   修改密码
  	13.FLUSH PRIVILEGES;               刷新权限
  	14.chkconfig mysql on              设置开机自启动  checonfig --list
  	
  	/var/lib/mysql                 数据库文件存放路径
  	/usr/share/mysql               配置文件目录
  	/usr/bin                       相关命令目录
  	/etc/init.d/mysql              启停相关脚本
  	
  	show variables like '%character%';  查看字符集
      set character_set_database=utf8;   设置数据库字符集
      set character_set_server=utf8;     设置服务器字符集
       	
  mysql连接被拒绝问题：
  1.	在装有MySQL的机器上登录MySQL mysql -u root -p密码
  2.	执行use mysql;
  3.	执行update user set host = '%' where user = 'root';这一句执行完可能会报错，不     用管它。
  4.	执行FLUSH PRIVILEGES;   刷新权限表
  
  ```

  存储引擎：

| 对比项 |                       myISAM                       |         InnoDB         |
| :----: | :------------------------------------------------: | :--------------------: |
| 主外键 |                       不支持                       |          支持          |
|  事务  |                       不支持                       |          支持          |
| 行表锁 | 表锁，即时操作一条数据也会锁住整个表，不适合高并发 |  只锁一行，适合高并发  |
|  缓存  |             只缓存索引，不缓存真实数据             | 都缓存，对内存要求较高 |
| 表空间 |                         小                         |           大           |
| 关注点 |                        性能                        |          事务          |
|        |                                                    |                        |

## 2019.10.12

```
性能下降sql慢：
​	1.查询语句写的烂
​	2.索引失效
​	3.关联查询太多
​	4.服务器调优及各个参数设置（缓冲、线程数等）


join：
​	内连接：共有部分
​		select <select list>
​		from tableA A
​		inner join table b
		on A.key = b.key
	左连接：全A
	    select <select list>
​		from tableA A
​		left join table b
		on A.key = b.key
	全连接：
	    select <select list>
​		from tableA A
​		full outer join table b
		on A.key = b.key
	除了共有部分的全部：
	    select <select list>
​		from tableA A
​		full outer join table b
		on A.key = b.key
		where A.key is null or b.key is null
		
		
索引：
	定义：
		索引是一种帮助mysql高效获取数据的数据结构。
		可以理解为排好序的快速查找数据结构。
			在数据之外，数据库还维护着满足特点查找算法的数据结构，这些数据结构以某种方              式指向数据，这样就可以早这些数据结构的基础上实现高级查找算法，这种数据结构              就是索引。
            索引也很多，一般也以文件的形式存在磁盘。
            一般索引都指B树，B+树
	目的：
		在于提高查询效率，可以类比字典。
	优势：提高数据查找的速率，降低数据的排序成本
	劣势：索引也是一个表，也会占用磁盘空间，大量更新数据时索引也需要更新。
	分类：
		单值索引：即一个索引只包含单个列，一个表可以有多个单值索引。
		唯一索引：索引值的列必须唯一，但允许为空。
		复合索引：即一个索引包含多个列。
			语法：
				创建：
				create[unique]index indexName on mytable(columnname(length))
				删除：droup index [indexname] on mytable;
				查看：show index from table_name\G
	结构：
		BTree：
			三层结构，根和树干节点都是虚假的数据，所有数据全都存在叶子节点。根和树干
			都有三个快分别指向大于最大数，小于最小数，在两数中间的节点。
	
	哪些情况需要索引：
		1.主键自动建立唯一索引
		2.频繁作为查询条件的字段应该创建索引
		3.外键关系建立索引
		4.频繁更新的不适合创建索引。
		5.where条件里用不到的字段不创建索引
		6.高并发下倾向组合索引
		7.排序字段，通过索引访问将提高访问速度
		6.查询中统计或者分组字段

```

## 2019.10.14

```
explain：
	作用：
		1.表的读取顺序
		2.数据读取操作的操作类型
		3.哪些索引可以使用
		4.哪些索引被实际使用
		5.表之间的引用
		6.每张表有多少行被优化器查询

案列：
	explain select * from class left join book on class.id=book.id;
	结论：type有all
	create  index Y on book(card)
	左右连接创建索引相反加
```

2019.10.16索引优化：

![1571187715166](C:\Users\LXL\AppData\Roaming\Typora\typora-user-images\1571187715166.png)

```
如何进行sql调优？
	1.慢查询的开启并捕获
	2.explain+慢sql分析
	3.show profile查询在mysql服务器里面的执行细节和生命周期
	4.sql数据库服务器的参数调优
优化原则：
	1.小表驱动大表：
		select * from A where id in (select id from b)
		等价于：
		for select id from b
		for select * from A where A.id=b.id
		当b表的数据集必须小于A表时用in优于exists。
		select * from A where exits(select 1 from b.id=a.id)
		等价于
		for select * from A
		for select * from B where B.id=A.id
		当a表的数据集小于b表的数据集时，用exists优于in。
		
	exists：
	select ... from table where exists(sybquery)
	语法理解为：将主查询的数据，放到子查询中做条件验证，根据验证结果，来决定主数据是否 保留。
	
	

```

## 2019.10.18慢日志

```
	记录响应时间超过阈值的日志。
show variables like 'slow_query_log'              查看慢查询日志状态
show variables like 'slow_query_log_file'         查看慢日志路径
set global slow_query_log=1                       开启慢查询日志
show variables like 'long_query_time'             查看慢日志时间，默认10s
set global long_query_time=3                      设置阈值时间
show global status like '%slow_queries%'          查看慢日志条数

```

## 2019.10.21批量插入

```
创建函数，假如报错 this functon has none of DETERMINISTIC...
由于开启过慢日志查询，因为我们开启了bin-log，我们就必须为我们的function指定一个参数。
show variables like 'log_bin_trust_function_creators'
set global log_bin_trust_function_creators=1;


创建两个函数，两个存储过程：

1.随机字符串函数：
CREATE  FUNCTION `rand_string`( n INT ) RETURNS varchar(255) CHARSET latin1
BEGIN
	DECLARE
		chars_str VARCHAR ( 100 ) DEFAULT 'abcdefghijklmnopqlstuvwxyz';
	DECLARE
		return_str VARCHAR ( 255 ) DEFAULT '';
	DECLARE
		i INT DEFAULT 0;
	WHILE
			i < n DO
			
			SET return_str = concat(
				return_str,
				substr( chars_str, floor( 1+rand ( ) * 26 ) , 1 ));
			
			SET i = i + 1;
			
		END WHILE;
		RETURN return_str;
	
END

2.随机数字函数
CREATE FUNCTION rand_num ( ) RETURNS INT ( 5 ) BEGIN
	DECLARE
		i INT DEFAULT 0;
	
	SET i = floor( 100+rand ( ) * 10 );
	RETURN i;
	
	END
3.插入存储过程1：
CREATE PROCEDURE `insert_emp` ( IN START INT ( 10 ), IN max_num INT ( 10 ) ) BEGIN
	DECLARE
		i INT DEFAULT 0;
	
	SET autocommit = 0;
	WHILE
			i < max_num DO
			INSERT INTO emp ( empno, ename, job, mgr, hiredate, sal, comm, depno )
		VALUES
			(
				( START + i ),
				rand_string ( 5 ),
				'SALESMAN',
				0001,
				curdate( ),
				2000,
				400,
				rand_num ( ) 
			);
		
		SET i = i + 1;
		
	END WHILE;
	COMMIT;

END
4.插入存储过程2：
CREATE  PROCEDURE `insert_dept` ( IN START INT ( 10 ), IN max_num INT ( 10 ) ) BEGIN
	DECLARE
		i INT DEFAULT 0;
	
	SET autocommit = 0;
	WHILE
			i < max_num DO
			INSERT INTO dept ( deptno, dname, loc )
		VALUES
			( ( START + i ), rand_string ( 4 ), rand_string ( 4 ) );
		
		SET i = i + 1;
	END WHILE;
	COMMIT;

END

死锁怎么办：
	show processlist;
	kill 进程号；


show profile 分析：
	show variables like 'profiling'   查看是否开启
    set profiling=on；                 开启
    
    
锁：
	锁是计算机协调多个进程或线程并发访问某一资源的机制。
	读锁（共享）写锁（排他锁）
	
	读锁会阻塞写，但是不会阻塞读，而写锁则会把读和写都阻塞。
	myisam（偏读）表锁，会造成更新线程永久堵塞。
	show status like 'table%'        查看表锁的次数和锁后争用等待的次数。
	show open tables , 有1的就代表有锁。
	
	innoDB和MyISAM最大的不同点是支持事务，采用行锁。
	事务ACID，原子，一致，隔离，持久。
	
间隙锁的危害：
	锁定范围键值时，即使不存的额数据也会被锁，造成在锁定时无法插入锁定键值范围内的任何数据。
	
如何锁定一行：
	begin；
	select * from test_innodb_lock where a=8 for update;
	
	
show status like 'innodb_row_lock%'  查看行锁的争用等待次数

```

## 2019.10.22，主从复制

```

```

