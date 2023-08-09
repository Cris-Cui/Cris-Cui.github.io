# MySQL

## 名词

属性: 特征

表中某一列: 字段

元组: 表的一行叫一个元组

码： 表中唯一确定一个元组的属性，码不止一个, 叫这些为候选码

主码：从候选码挑选一个主要确定这一行的属性

主属性：只要在任何一个候选码中出现过，这个属性叫主属性

非主属性: 没有在任何一个候选码中出现过。

## 数据定义语言(DDL)&数据控制语言(DCL)

1. 创建数据库
	- create schemas
	- create database

2. 设置用户权限

	```mysql
	
	management > users and priviledges
	localhost 127.0.0.1 % 本机任意网卡
	创建用户
	# 临时授权
	grant select on 表名 to user;
	# 取消授权
	revoke select on 表名 from user:
	```

3. 创建&删除数据表

	- 选定数据库

		```mysql
		use 数据库名;
		```

	- 创建表

		```mysql
		create table 表名(列名 类型 约束, 列名 类型 约束, 列名 类型 约束···);
		```

	- 建表约束

		```mysql
		1.主键约束	primary key	   值唯一的列,不允许重复,不允许为空主键列是唯一列，其他列不可以是主键
		2.唯一	 unique			可以有多列唯一，值不重复，但可以为空
		3.默认值	default
		4.非空	 not null
		5.自增	 auto increment
		6.外键约束  foreign key
		7.check校验(mysql不支持)
		```

	- 删除表

		```mysql
		drop table 表名;
		```

	- 修改表

		```mysql
		alter table 表名 add column 列名 类型;	# 增加一列
		alter table 表名 modify 列名 属性;		# 修改列属性
		alter table 表名 drop column 列名;		 # 删除列
		```

	- 拷贝表

		```mysql
		create table 表1 like 表2;				# 拷贝表结构, 但是没有数据
		create table 表1 as (select * from 表2);	# 拷贝数据, 但是不会有主键和索引
		create table 表1 like 表2; insert into 表1 select * from 表2;	# 又要有表结构, 又要有数据
		```

## 范式

### 第一范式(1NF)

> 属性不可分

### 第二范式(2NF)

> 在1NF基础上, 不存在组合关键字中的某些字段决定非关键字段

(组合关键字不可拆)

例子: 选课课表SelectCourse(学号, 姓名, 年龄, 课程名称, 成绩, 学分)

组合关键字(学号, 课程名称) --> (姓名，年龄，成绩，学分)

但存在组合关键字的部分关键字决定非关键字字段：

- (学号) --> （姓名, 年龄)
- (课程名称) --> (学分)

问题：

1. 数据冗余
2. 更新异常
3. 插入异常

### 第三范式(3NF)

> 在2NF基础上,  不存在传递依赖

例子：学生关系表(学号, 姓名, 年龄, 所在学院, 学院地点, 学院电话)

(学号) --> 

### BCNF(鲍依斯-科得范式)

> 在前三个范式的基础上, 不存在关键字段决定关键字段

![image-20230805182747390](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230805182747390.png)

## 数据操作语言(DML) -- CRUD

### 数据查询

#### 高级查询

##### 条件查询

1. 相等 

	```mysql
	where 列名 = 值
	```

2. 不相等

	```mysql
	where 列名 != 值 (只有MySQL支持) 或者 where 列名 <> 值
	```

3. 或者 

	```mysql
	or 连接两个条件
	```

4. 并且

	```mysql
	and 连接两个条件
	```

5. 介于之间

	```mysql
	between and
	```

	



##### 模糊查询

##### 查询不重复记录

##### 排序和限制(分页查询)

##### 聚合

##### 表连接(多表查询)

###### 内连接

内联

```mysql
表1 + inner join + 表2 + on + 连接条件
```



###### 外连接

##### 子查询

##### 记录联合

### 插入数据

### 修改数据

### 删除数据

## 视图

> 视图是一个表或多个表导出的虚表，不是真实存在的

### 语法

1. 创建视图

	```mysql
	CREATE view 视图名 as （SELECT 语句）
	```

2. 删除视图

	```mysql
	drop view 视图名
	```

### 优缺点

- 优点: sql语句在网络传输中，使用视图可以减少流量，保证sql语句不会有问题，更安全
- 缺点: 执行效率并不会提高，还是执行原来的语句

## 函数&存储过程

### 函数

#### 语法

1. 创建函数

	```mysql
	delimiter // # 定义结束标志
	create function 函数名(参数1 类型, 参数2 类型...)
	returns 返回值类型
	begin 
		declare 变量名 类型 default 值; # 声明局部变量并赋初值
		set 变量 = 值; # 赋值操作
		return 值;
	end //
	delimiter;
	```

2. 删除函数

	```mysql
	drop function 函数名;
	drop function if exists 函数名;
	```

3. 变量

	- 局部变量

	- 会话变量: 本次连接会话有效, 不需要定义声明, 直接使用

		```mysql
		@变量名
		```

	- 系统变量(全局变量): root 才能使用，一直都有效

		```mysql
		@@变量名
		```

4. 选择语句

	```mysql
	if (a > b) then 执行语句;
	elseif (a = b) then 执行语句; # 判断相等用"="
	else 执行语句;
	end if;
	```

5. 选择语句

	```mysql
	case 变量 when 值 then ...;
			 when 值 then ...;
			 when 值 then ...;
			 else 执行语句;
	end case;
	
	```

6. 循环语句

	```mysql
	while 条件
	do 
		执行语句;
	end while;
	```

### 存储过程

> 存储过程(Stored Procedure) 是在大型数据库系统中， 一组为了完成特定功能的SQL语句集, 它存储在数据库中, 一次编译永久有效, 用户通过指定存储过程的名字并给出参数（如果该存储过程带有参数) 来执行它。
>
> 存储过程是数据库中的一个重要对象， 在数据量特别庞大的情况下利用存储过程能达到倍速的效率提升.

#### 优点

1. 减少网络流量 存储过程直接在服务器端运行，减少了与客户机的交互
2. 增强了代码的重用性和共享性
3. 加快系统运行速度
4. 使用灵活
5. 作为一种安全机制来充分利用

#### 语法

1. 定义存储过程

	```mysql
	delimiter //
	create procedure 过程名(参数1 类型, 参数2 类型...)
	begin 
		执行语句1;
		执行语句2;
	end //
	delimiter;
	```
	
2. 执行存储过程
	
```mysql
	
```

### 存储过程和函数的区别

1) 返回值: 函数必须返回一个值，而存储过程可以不返回值或者返回多个结果集。

2) 调用方式:函数可以像其他表达式一样被调用，可以作为 SELECT 语的一部分使用而存储过程则需要通过 CALL 语来调用执行

3) 数据处理: 存储过程可以包含数据定义语句 (如 CREATE、ALTER、DROP)，以及事务控制语句。(如 COMMIT、ROLLBACK)函数否能包含这些语句。

4) 执行权限:存储过程需要执行时具有执行权限的用户或角色，而函数可以由任何具有访问权限的用户或角色使用

5) 灵活性:存储过程通常用于执行复杂的操作，可以包含条件判断、循环等控制结构，因此更加灵活。而函数主要用于计算和返回单个结果，适合用于简单的数学计算、字符串处理等场景。

## 触发器

> 是一种特殊的存储过程, 当指定事件发生时，系统自动调用

触发条件:

- 修改表数据
- 插入表数据
- 删除表数据

### 语法

1. 创建触发器

	```mysql
	delimiter //
	create trigger 触发器名
	after/before 操作 # 在发生某某操作之后/之前
	on 表名 # 产生触发的表
	for each row # 影响每一行
	begin 
		执行操作;
	end;
	delimiter;
	```

2. 删除触发器

	```mysql
	drop 
	```

## 索引

> 形象的表达, 索引是数据的目录

### 语法

1. 创建索引

	```mysql
	# 建表时
	index(列名)
	# 单独创建索引
	create index 索引名 on 表名(列名);
	```

2. 删除索引

	```mysql
	drop index 索引名 on 表名;
	```

	

### 索引的分类

#### 按数据结构分类

> B+树 -- 多叉树， 叶子结点存放数据，非叶子结点存放索引 (叶子结点之间用双向链表连接)
>
> B+树相比B树，二叉树，哈希表的优势?
>
> 

![image-20230805200907830](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230805200907830.png)

![image-20230805201006345](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230805201006345.png)

![image-20230805201324813](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230805201324813.png)

![image-20230805201611094](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230805201611094.png)

![image-20230805201927567](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230805201927567.png)

![image-20230805202304815](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230805202304815.png)

![image-20230806182725787](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230806182725787.png)

![image-20230806183247648](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230806183247648.png)

![image-20230806184156009](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230806184156009.png)

![image-20230806184438856](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230806184438856.png)

![image-20230806185300556](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230806185300556.png)

![image-20230806185940307](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230806185940307.png)

![image-20230806190542678](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230806190542678.png)

![image-20230806190804380](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230806190804380.png)

##### B+树索引

##### Hash索引



##### Full-text索引

#### 按物理存储分类

##### 聚簇索引(主键索引)

##### 二级索引(辅助索引)

#### 按字段特性分类

##### 主键索引

##### 唯一索引

##### 普通索引

##### 前缀索引

#### 按字段个数分类

##### 单例索引

##### 联合索引(复合索引)

### 索引的使用场景

> 使用索引是为了提高查询效率, 如果不使用索引，执行查询时要**全表逐个查询**

#### 缺点

1. 占用物理空间，数量越大，占用空间越大，会影响表的最大存储量

2. 创建索引和维护索引要耗费时间

3. 降低表的增删改效率，需要维护B+树

#### 不需要创建索引的场景

1. 表数据太少，不需要创建索引

2. 经常更新的字段不需要创建索引，避免维护B+树的有序性降低数据库的性能

3. 字段存在大量重复数据，比如性别不需要创建索引
4. WHERE条件，GROUP BY，ORDER BY 里用不到的字段不需要创建索引，索引的价值是快速定位

### 优化索引的方法

### 如何知道索引是否触发?

> 使用关键字explain 

```mysql
explain select * from t_user where id;
```

显示10列信息:

- id 选择标识符
- select_type 表示查询的类型
- table 输出结果集的表
- type 表示表的连接类型
- possible_kyes 表示查询时，可能使用的索引
- **key** 表示实际使用的索引
- **key_len** 索引字段的长度
- ref 列与索引的比较
- **rows** 扫描出的行数(估算的行数)
- extra 执行情况的描述和说明

## 事务

> 数据库事务是作为单个逻辑工作单元执行的一系列sql操作， 要求**要么都执行,要么都不执行**

### 事务四大特性ACID

- A Atom 原子性，事务是最小工作单元,不可再分,要么都执行,要么都不执行
- C consistency 一致性,数据库的完整性约束不能被破坏
- l  isolusion 隔离性 并发执行的事务是隔离的，保证多个事务互不影响
- D durability 持久性 持久保存到本地 事务-->提交 数据发生改变是永久的

### 语法

```mysql
start transaction;	# 开启事务
执行语句1;
执行语句2;
...
commit; 	# 提交
rollback;	# 回滚
```

### 事务日志- 重做日志和回滚日志

- 重做日志。采用的方式是预写日志方式。也就是写数据之前先写日志。存储的是执行语句。

	(重写日志缓存 和 重写日志文件 )当开始一个事务的时候。会记录该事务日志序列号当事务执行时，先往InnoDB引擎日志缓存里插入事务日志(执行语句)当事务提交时，必须先将写入磁盘

- 回滚日志 存储都是执行语句存储引擎的日志缓存，存储与重做日志相反的语句比如 重做日志里面是 insert --> delete update --> 相反的update

数据库的持久性除了文件缓存到本地之外。 有一套机制可以恢复数据库mysql InnoDB引擎 通过回滚日志,将所有已经完成 或存储在磁盘没完成的事务进行回滚然后redo 重做日志里面的全部重新执行一遍,恢复数据

### 数据发生并发问题产生错误的类型

#### 脏读

> 读取数据是错误的，不能用。两个事务在执行时，事务B读到了事务A未成功提交的数据

#### 不可重复读

> 读取的数据两次不一样.-->不一定算错误

#### 幻读

> 读取的数据不一致

#### 解法方法

>  数据库并发事务采用隔离等级

数据库隔离等级 - 读未提交 读已提交 可重复读 行化

1.**读未提交**-- read uncommited 在读数据时不会检查或使用任何锁在隔离等级中,可以读取没有提交的数据 会发生脏读 相当于没有并发控制

2.**读已提交 ** read  commit (大多数的数据库默认的隔离等级 sql server oracle只读取提交的数据并等待其他事务释放排他锁,读数据的共享锁在读操作之后会立即释可能出现的问题->不可重复读 可以解决脏读

3.**可重复读** repeatable read (rr)( mysql 默认的隔离等级)只读取提交的数据，并等待其它事务释放排它锁读取数据的共享锁在事务结束时释放( 共享锁是行锁 

4.**串行化**  可以用来解决幻读问题 强制事务排列执行 不可能发生冲突 解决幻读实质是锁定受影响的数据范围，阻止新数据插入查询所涉及的范围

![image-20230809124556228](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230809124556228.png)

#### mysql 默认的隔离等级

mysql 默认的隔离等级是可重复读,通过多版本并发控制MVCC

对于快照读InnoDB使用MVCC 解决幻读

对于当前读,InnoDB 通过间隙锁(锁一个范围)解决幻读

杂记:
1.MyISAM InnoDB 区别 行锁关于 InnoDB 使用的行锁:
用索引去查数据的时候才会用行锁 锁定 key，其他默认的情况使用 表锁2.页面锁，是特定的引擎使用的，锁定多行
3.排他锁是在事务提交之后释放

#### MVCC 多版本并发控制

对应的 基于锁的并发控制 Lock-based concurrent control LBCC
MVCC 最大优势:读不加锁，认为读写不冲突，在读多写少的情况下,读写不冲突十分重要

可以极大的增强系统的并发能力

思路 MVCC 是一个乐观锁，处理并发的时候,比较乐观的看待并发问题

认为一般情况没有数据冲突,只有在数据修改的时候才会出现冲突,只需要判断先后有没有修改如果没有修改，不需要管理,如果有修改,就认为有并发的冲突,此次读写失败，尝试下一次读写乐观锁---种实现的方式 CAS机制 ( compare and swap )比较然后交换，就是空间没有改写，可以操作，被其他的线程改写过，此次操作失败进入下一次操作,再继续判断 -- 无锁编程

CAS机制包含三个操作数:1)内存位置V 2) 预期原值A 3) 新值B 如果内存位置上的值与预期原值相等，则将该位置的值更新为新值.CAS操作是原子性的，因此在多线程环境下使用能保证线程安全， 如果多个线程执行CAS操作，只有其中一个线程能够成功执行，其他线程需要重新执行操作。 如果内存位置上的值与预期原值不相等，说明共享数据已经被修改，放弃所做的操作，然后重新执行刚才的操作，直到重试成功。

> 在学习MVCC之前，必须先了解一下，什么是MySQL InnoDB下的当前读和快照读?当前读像select lock in share mode(共享锁), select for update ; update, insert ,delete(排他锁)这些操作都是一种当前读，为什么叫当前读? 就是它读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁快照读 -致性非锁定读取，使用普通select 语读取读取快照版本,通过MVCC的ReadView实现(快照读的前提是隔离级别不是串行级别，串行级别下的快照读会退化成当前读，之所以出现快照读的情况，是基于提高并发性能的考虑，在很多情况下，避免了加锁操作，降低了开销;
> 快照读可能读到的并不一定是数据的最新版本，可能是之前的历史版本



##### MVCC 怎么实现?

实现MVCC依靠隐藏列，undo log 以及 Read View来实现隐藏列
DB TRX ID 记录插入或更新该行，最后一个事务的事务ID,每执行一个事务,+1DB ROLLPTR 指向修改该行对应的回滚日志的指针
DBROWID 单调递增的行IDundo log 改动的数据都保存在undo log 中,同时里面有保存事务的id，可以用于生成快照读，同样回滚可以实现数据恢复
Read View SQL语句执行前都可以得到一个ReadView,它是一个副本,主要用来知道哪些事务可以看到数据，哪些不可以

## 锁

> 保证并发访问的一致性和有效性

Mysql 涉及的三种锁
1.表级锁: 锁表, 开销小 加锁快,   不会死锁，锁定粒度大 并发效率低 锁冲突概率较高

2.行级锁: 锁行, 开销大,加锁慢，可能死锁，锁粒度小     并发效率高,锁冲突概率较小

3.页面锁: 粒度介于表锁和行锁,并发一般,开销和速度也是介于表和行的



### 不同引擎关于锁的模式

**MyISAM** -- 只有表锁
有两种模式:表共享锁( table Read Lock ) 表独占写锁(Table write lock)
读锁锁定，不会影响其他读的，但是会影响写写锁，是又影响其他写，又影响其他读.
共享锁 -- 读
排他锁 --写

MyISAM-- 不支持 事务的 显式的锁定表 LOCK - UNCLOCK

```mysql
LOCK tables orders read local, order detail read local; //显式表锁
select SUM(total) from orders;
select SUM(subtotal) from order detail;
UNCLOCK tables;
```

两个表,如果想同一个时刻查询查看是否相等,可以显式使用表锁
MyISAM 锁的调度
读和写是互斥的，假如一个进程请求MyISAM表的读锁，另一个进程请求同一个MyISAM表的写锁，MYSQL是怎么处理的?

**写优先.**即使在所等待队列里边读的进程排在前面。写的进程排在后边。也会先去执行写锁

**InnoDB** 锁问题
nnoDB和MyISAM最大的不同 1.支持事务 2.采用行锁

事务
sql1;
sql2;
只能一个一个执行.可能需要关联，sql2执行在sql1执行成功的前提下,去执行sql1 没有正常执行,sgl2 就不执行了你要怎么做?图书管理 sql1 插入书籍 sql2 cout +1可以使用事务 -- 要么都执行，要么都不执行。理解，可以近似看成线程

### 



## 面试常见问题

> Q: mysql 一张表可以有多少行数据?
>
> A: 极限情况是 **500万条**

![image-20230804204837287](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230804204837287.png)

> Q: 一条SQL语句在数据库中的执行流程?
>
> A: ![image-20230805185519086](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230805185519086.png)

> Q: SQL语句中每种关键字的先后执行顺序?
>
> A: 
>
> 1. from子句组装来自不同数据源的数据(笛卡尔积计算)
> 2. 应用on筛选器
> 3. 执行join
> 4. where子句基于指定条件对记录行进行筛选
> 5. group by子句将数据划分为多个分组
> 6. 聚集函数进行计算
> 7. 使用having子句筛选分组(二次筛选)
> 8. select的字段
> 9. distinct去除重复行
> 10. order by 对结果集进行排序
> 11. limit 查询指定范围

> Q:
>
> A: 