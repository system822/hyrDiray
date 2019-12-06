##数据库的操作（ORACLE）
**12/5/2019 5:05:53 PM**
####常用的数据类型

* Java中数据类型有：byte、short、int、long、float、double、boolean、char、String等
* Oracle数据中数据类型：  
	* number(n) - 表示具有n位数字的整型数值，无数字相当于38位。
	* number(n,m) - 表示该数值具有n位数字，其中小数点后面有m位数字。eg:number(3,2)
	* char(n) - 表示描述长度为n的定长字符串，若没有存满则使用空格补齐。
	* varchar2(n) - 表示描述长度为n的变长字符串，若没有存满则自动缩小。
	* date - 表示描述年月日时分秒的日期数据。

####SQL结构化查询语言<font color="red">******</font>
#####基本分类
* DDL数据定义语言（Data Definition Language）- 主要用于实现对表格的创建、修改以及删除操作。
* DML数据库操作语言（Data Manipulation Language）- 主要用于实现对表格中数据的插入、修改（更新）以及删除操作。
* DQL数据查询语言（Data Query Language）- select
* TCL事务控制语句(Transaction Control Language) - 主要用于实现对数据更改的提交和回滚(撤销)等操作。  commit(提交数据)   rollback(回滚数据)  update(修改数据)
* DCL数据控制语言(Data Control Language) - grant（授权）  revoke（回收权限）

######DDL数据定义语句
1. 创建表格的语法格式  

		create table 表名（  
			字段名称1  字段类型，  
			字段名称2  字段类型，  
			...   
		）；
2. 修改表格的语法格式  

	* alert table 表名 drop column 列名;    
		eg:--删除person表中名字为sex的列  
			alert table person drop column sex;  
	* alert table 表名 add 列名 类型;      
		eg:--向person表中添加名字为gender的列  
			alert table person add gender char(3);  
	* alert table 表名 rename to 新的表名;    
		eg:--实现将person表的名字更改为people  
			alert table person rename to people  
	* alert table 表名 rename column 列名 to 新的列名  
		eg:--实现将people表中名字为gender的列名更改sex  
			alert table people rename column gender to sex
3. 删除表格的语法格式  
	drop table 表名  
######DML数据操作语言
1. 插入数据的语法格式  
	insert into 表名[(列名1，列名2，...)] values(数值1，数值2，...)
2. 更新/修改数据的语法格式  
	update 表名 set 列名 = 值 [where条件]
3. 删除数据的语法格式  
	delete [from] 表名 [where条件]
######DQL数据查询语言（select查询语句）
1. 查看表结构  
	desc 表名;  
2. 字段的查询  
	select 字段名1,字段名2,... from 表名
3. 常用的算数运算符  
	加法 +   减法 -   乘法 *   除法  /   取余  mod()  

4. 列的别名  
	select 字段名1 [as] 别名1，字段名2 [as] 别名2，...	from 表名;  
	>> 当别名中有空格或者需要区分大小写时，就需要使用" "括起来作为整体；  
	>
	>sql语句本身不区分大小写，但' '和" "中的内容是区分大小写的  

5. 字符串的拼接  
	|| 用于实现字符串的拼接，相当于Java语言中 + 运算符。  
如：  -- ' 就是转义字符  '' 表示打印一个'  
   select last_name||''''||first_name 姓名 from s_emp;
6. where子句  
	select 字段名1 [as] 别名1，字段名2 [as] 别名2，... from 表名 [where 条件];
7. 常用的比较运算符  
	大于 >   大于等于 >=   小于 <   小于等于 <=    等于 =   不等于 <>  
	>>在sql语句中没有赋值的概念，只有=表示等于的含义。  

8. 逻辑运算符  
	and 并且   or 或者   not 非   
	>>and运算符的优先级高于or,因此若希望先执行or运行时可以使用()来提高优先级
	>where子句的执行次序是从右向左执行的，因此建议如下：
	>>>对于and运算符来说，应该将为假的可能性更高的条件放在右边；
	>>>对于or运算符来说，应该将为真的可能性更高的条件放在右边；

9. 按照范围进行查询  
	between ... and ...   表示在...和...之间，闭区间  
	如：  
	-- 查询员工表中薪水范围在800到1400之间的员工名称以及员工薪水  
  	 	select first_name, salary from s_emp  where salary between 800 and 1400;  
   -- 等价的写法  
   		select first_name, salary from s_emp where salary >= 800 and salary <= 1400;  
   -- 非等价的写法  
   		select first_name, salary from s_emp where salary > 800 and salary < 1400;
10. 空值的处理  
	is null  -  判断是否为空  
	is not null - 判断是否不为空  
	由于空值和任何数据运算的结果还是空值，因此需要使用nvl函数加以处理，具体：  
		nvl(参数1，参数2)  - 若参数1不为空则使用参数1的数值，否则使用参数2的数值   
	如：  
	-- 查询员工表中加上奖金后的员工名称、员工薪水以及奖金信息  
   select first_name, salary, commission_pct,salary+nvl(commission_pct,0)  from s_emp;   

11. 数据的去重  
	select [distinct] 字段名1 [as] 别名1，字段名2 [as] 别名2, ... from 表名 [where 条件]

12. 按照列表进行查询  
	in 表示在...里  
	not in 表示不在...里  
	如：   
	-- 查询员工表中部门编号为41或42的员工名称、薪水以及部门编号信息  
	select first_name, salary, dept_id from s_emp where dept_id in(41, 42);    
 	
	-- 查询员工表中部门编号不为41和42的员工名称，薪水以及部门编号信息    
	select first_name, salary, dept_id from s_emp where dept_id not in(41, 42); 
13. 模糊查询  
	like ...  表示按照指定的条件进行模糊查询  
		_  表示可以出现任意一个字符  
		%  表示可以出现任意多个字符
	
14. 字段的排序  
	select [distinct] 字段名1 [别名1], 字段名2 [别名2], ... from 表名  [where 条件] [order by 字段名/别名/位置编号 asc/desc];
15. 常用的字符串函数   
	* -- 查询字符串的长度
   		select length('hello') 计算字符串的长度 from dual;   -- 5
   	* -- 实现将字符串所有字符串转换为大写
   		select upper('hello') 将字符串转换为大写 from dual;  -- HELLO
   	* -- 实现将字符串所有字符转换为小写
   		select lower('HELLO') 将字符串转换为小写 from dual;  -- hello
   	* -- 实现将字符串中首字母转换为大写
   		select initcap('hello') 首字母转换为大写 from dual; -- Hello
   	* -- 实现字符串中内容的截取，表示从下标2开始截取3个字符，下标从1开始
   		select substr('hello', 2, 3) 截取字符串 from dual;  -- ell
   	* -- 实现两个字符串之间的拼接
   		select concat('hello', 'world') 拼接字符串 from dual; -- helloworld
   	* -- 实现字符串中内容的替换，表示将'e'替换为'E'
   		select replace('hello', 'e', 'E') 替换字符串 from dual; -- hEllo
   	* -- 实现字符串中内容的替换，全部替换
   		select replace('hehe', 'e', 'E') 替换字符串 from dual;  -- hEhE
   	* -- 实现字符串中子字符串第一次出现的索引位置
   		select instr('hello', 'e') 查找下标 from dual;  -- 2  
   	* -- 实现字符串中前后两端空白字符的去除
   		select trim('     hello     ') 去除两端空白 from dual;  -- hello  
   		select length(trim('     hello     ')) 去除两端空白 from dual; -- 5  
16. 常用的数值函数  
	保留小数3位小数,四舍五入 -- select round(18.56789,3) - 18.568  
	保留三位小数并进行截取功能 -- select trunc(18.56789,3)  -18.567  
17. 常用的日期函数  
	* to_char函数主要用于将数据中的日期类型数据获取出来并转换为字符串类型显示出来。
	* to_date函数主要用于将字符串类型的日期数据转换为日期类型数据再插入到数据库
	* -- 查询当前的系统时间并按照年月日时分秒的格式显示出来  
   		select to_char(sysdate, 'yyyy-mm-dd hh24:mi:ss') 当前系统时间 from dual;  
	* -- 向员工表中插入'张衡'的信息并指定入职日期为当前系统时间  
   		insert into s_emp (id, last_name, first_name, start_date) values(28, '张', '衡', to_date('2019-08-30', 'yyyy-mm-dd'));  
18. 常用的聚合(多行)函数  
	* 单行函数 - 主要是将单条数据内容传入给该函数处理后的结果还是单条数据内容。  
	* 聚合(多行)函数 - 主要将多条数据内容输入该函数处理后的结果是单条数据内容。
		* 常用的聚合函数：
			* sum() - 主要用于实现多条数据累加和的计算，只能用于数值类型。
			* avg() - 主要用于实现多条数据平均值的计算，只能用于数据类型。
			* max() - 主要用于实现多条数据最大值的计算，可以处理任意类型的数据。
			* min() - 主要用于实现多条数据最小值的计算，可以处理任意类型的数据。
			* count() - 主要用于实现多条记录统计个数的计算，通常使用count(*)来实现。
19. 分组查询  
	select [distinct] 字段名1 [别名1]，字段名2[别名2], ... from 表名 [where 条件] [group by 字段名] [order by 字段名/别名/位置编码 asc/desc];

20. 分组之后的再次过滤  
	select [distinct] 字段名1 [别名1]，字段名2 [别名2]，... from 表名 [where 条件] [group by 字段名] [having 条件] [order by 字段名/别名/位置编号 asc/desc];
 	
	eg:查询每个年级的总人数且总人数要超过18人的才显示出来  
		错误方案:select gradeid 年级编号, count(*) 总人数 from student where count(*)>18 group by gradeid;  
		错误原因:where子句中不能出现聚合函数  
		正确方案:select gradeid 年级编号,count(*) 总人数 from student group by gradeid having count(*)>18;

>>>总结:select查询语句一般的执行流程:  
>
	from 表名 => where子句进行过滤 => group by子句进行分组 => having子句进行再次过滤 => select将数据取出来 => order by子句对取出来进行排序

21. 子查询  
	子查询就是指将一个查询语句的结果作为另外一个查询语句条件的方式，又叫做多次/多重查询，也就是查询语句中嵌套着另外一个查询语句。  
	eg:  查询学生表中与'崔今生'在同一个年级的学生信息  
	select studentname, gradeid from student where gradeid = ( select gradeid from student where studentname = '崔今生' );  

22. 多表查询
	* 单表查询 - 主要指select查询语句中from关键字后面只跟一个表名的形式
	* 多表查询 - 主要指select查询语句中from关键字后面跟着多个表名的形式，也就是说同事查询的数据来与多张表。
		1. 内连接
			* 所谓内连接就是指使用两张表都有的字段和关系运算符进行匹配，从而得到两张表都有数据内容组成的结果集。
			* 内连接的语法
				1. 内连接的语法格式一(老版):
					* select [表名1.]字段名1,[表名2.]字段名2,...from 表名1[别名1], 表名2 [别名2], ... where 表之间的关联条件; 
				2. 内连接的语法格式二(新版):  
					* select [表名1.]字段名1, [表名2.]字段名2, ...from 表名1 [别名1] inner join 表名2[别名2] on 表之间的关联条件;
			>>>当内连接忘记指定表之间的关联条件时，得到5456条记录  笛卡尔成绩
			>就是该方式会使用第一张表中的每一条数据与第二张表中的每一条数据都组合一次
		
		2. 左外连接
			* 所谓左外连接就是指将满足关联条件且两张表都有的数据显示之后，将左表中存在但右表中没有匹配的数据也会显示出来，只是将右表中没有的数据使用null填充。(也就是说以左表为主，将左表的所有数据显示出来)
				1. 语法格式(旧版)：
					select [表名1.]字段名1 别名1, [表名2.]字段名2, ...  from 表名1 [别名1], 表名2 [别名2] where [表名1.]字段名 关联 [表名2.]字段名(+);
				2. 语法格式(新版):
					select [表名1.]字段名1 别名1, [表名2.]字段名2, ... from 表名1 [别名1] left outer join 表名2 [别名2] on 两张表的关联条件;
		3. 右外连接
			* 所谓右外连接就是指将满足关联条件且两张表都有的数据显示之后，将右表中存在但左表中没有匹配的数据也会显示出来，只是将左表中没有数据使用null填充。(也就是说以右表为主，将右表的所有数据显示出来)
				1. 语法格式(旧版)：
					select [表名1.]字段名1 别名1, [表名2.]字段名2, ... from 表名1 [别名1], 表名2 [别名2] where [表名1.]字段名(+) 关联 [表名2.]字段名;
				2. 语法格式(新版):
					select [表名1.]字段名1 别名1, [表名2.]字段名2, ... from 表名1 [别名1] right outer join 表名2 [别名2] on 两张表的关联条件;
		3. 全外连接
			* 所谓全外连接就是指将满足关联条件且两张表都有的数据显示之后，将左右表中存在但没有匹配的数据也会显示出来，只是将左右表中没有数据使用null填充。(也就是说以左右表为主，将左右表的所有数据显示出来)
				1. 语法格式(旧版不支持)：
				2. 语法格式(新版):
					select [表名1.]字段名1 别名1, [表名2.]字段名2, ... from 表名1 [别名1] full outer join 表名2 [别名2] on 两张表的关联条件;
***
23. 分页查询 
**** 
	由于查询到的数据内容比较多无法在一个页面中全部显示，因此可以将查询到的数据内容分成很多页分别显示即可，这样的机制就叫分页查询。  
	分页查询的优化：
		-- 借助伪列实现真正的分页查询, 采用三层子查询  
		-- 第一步：负责排序
			select * from xdl_bank_account_34 order by acc_money
		-- 第二步：负责编号起别名以及部分过滤
			select rownum r,t.* from (select * from xdl_bank_account_34 order by acc_money) t    
		-- 第三步：使用别名进行过滤
			select * from (select rownum r,t.* from (select * from xdl_bank_account_34 order by acc_money) t where rownum < (页数*条数+1)) where r > (上一页数*条数)； 
****
>在 xml中不识别<应用实体符代替
######数据的完整性约束****
1. 基本概念
	* 为了避免以后向表中插入一些错误或不符合规范的数据，就需要程序员在设计表时建立表中数据的完整性约束。
	* 完整性主要包括四大类：域完整性、实体完整性、自定义完整性、引用完整性。
2. 主键约束
	* 当创建表时指定主键约束可以确保该字段(列)中的数据唯一并且不能为空。
		* -- 创建一个worker工人表  
				create table worker(  
     			-- 相当于给字段id加入主键约束  
     			--        约束关键字 约束名称 约束类型  
     			id number constraint pk_id   primary key          -- 描述工人表的工号字段  
  			);  
  		* -- 向表中插入数据  
  			insert into worker values(1);  
  			insert into worker values(2);  
  		* -- 工号重复但没有报错，加入主键约束后下面的代码会报错  
  			insert into worker values(1);
  		* -- 不能为空  
  			insert into worker values(null);
  		* -- 删除工人表  
  			drop table worker;
3. 唯一约束
	* 当创建表时指定唯一约束则可以确保该字段(列)中的数据唯一但可以为空。
	>>注意：只能有一个空值
4. 非空约束
	* 当创建表时指定非空约束则可以确保该字段(列)中的数据不能为空。
5. 检查约束
	* 当创建表时指定检查约束则可以确保该字段(列)中的数据不能超过取值范围和格式限制
		* -- 创建一个worker工人表  
  			create table worker(  
    		 	-- 相当于给字段id加入主键约束  
     			--        约束关键字 约束名称 约束类型  
     			-- 描述工人表的工号字段  
     			id number constraint pk_id   primary key,  
     			-- 相当于给字段name加入唯一约束    
     			name varchar2(30) constraint uq_name unique,  
     			-- 相当于给字段sex加入非空约束   
     			sex char(3) constraint nn_sex not null,  
     			-- 相当于给字段salary加入检查约束    约束说明  
     			salary number(11, 2) constraint ck_salary check(salary >= 2200) 
  			);
6. 表级约束
	* 当创建表时写完所有的字段列表后再写约束规则的形式，就叫做表级约束。
7. 创建表后约束的控制
	* 当一个表已经创建完毕后，若希望添加或者删除约束时的处理方式如下：
	***
		  -- 删除工人表
  		  drop table worker;
  		  -- 创建一个worker工人表
		  create table worker(
		     id number,  
		     name varchar2(30),
		     sex char(3),
		     salary number(11, 2) 
		  );
		
		  -- 通过修改表结构的方式来添加约束，注意表格中不要有数据
		  alter table worker
		  add constraint pk_id primary key(id);
		  -- 向表中插入数据
		  insert into worker values(1, '张飞', '男', 3000);
		  insert into worker values(2, '关羽', '男', 4000);
		  -- 此时应该报错，违反主键约束
		  insert into worker values(1, '刘备', '男', 5000);
		
		  -- 通过修改表结构的方式来删除约束
		  alter table worker
		  drop constraint pk_id;
		  -- 此时应该可以
		  insert into worker values(1, '刘备', '男', 5000);
	***
8. 外键约束
	* 当本表中某一字段的数值取决于另外一张表的主键时，此字段就需要加上外键约束。（也就是依赖于外部的主键，当前表叫做子表，所依赖的表叫做主表）
######事务
1. 事务的由来
	- 使用图形化工具向年级表中插入数据，此插入操作实际上只是把数据放到内存中
	- 没有进行提交时，命令行工具无法进行插入操作，因为图形化工具的流程没有走完（类似于数据库被锁住，直到*图形化工具的流程走完才能解锁）
	- 这个图形化工具执行的流程 叫做 一个事务
	- 使用commit实现数据的提交，才会将数据由内存插入到数据库的表空间中
   commit;
	>注意：在以后的开发中只要使用到dml语句一定要记得commit才会提交到数据库中

2. 事务的使用
***
	-- 使用事务来模拟离校手续的办理
	begin
	  -- 1.创建HistoryResult历史成绩表，字段与result表中的字段一样
	  CREATE TABLE HistoryResult(
	    Id number(4)  primary key,
	    StudentNo varchar2(50) NOT NULL,
	    SubjectId number(2) NOT NULL,
	    StudentResult number(4) NULL,
	    ExamDate date NOT NULL
	   );
	  -- 2.查询Result表中所有“白银”阶段学生的考试成绩，保存到表HistoryResult中
	  insert into HistoryResult select * from result 
	  where subjectid in ( select subjectid from subject 
	    where gradeid = ( select gradeid from grade 
	      where gradename = '白银' ));
	  -- 3.删除Result表中所有“白银”阶段学生的考试成绩
	  delete from result where subjectid in ( select subjectid from subject 
	    where gradeid = ( select gradeid from grade 
	      where gradename = '白银' ));
	  
	  -- 4.创建HistoryStudent历史学生表，字段与student表中的字段一样
	  CREATE TABLE HistoryStudent(
	    StudentNo varchar2(50) NOT NULL primary key,
	    LoginPwd varchar2(20) NOT NULL,
	    StudentName varchar2(50) NOT NULL,
	    Sex char(3) NOT NULL,
	    GradeId number(2,0) NOT NULL,
	    Phone varchar2(255) NOT NULL,
	    Address varchar2(255) NULL,
	    BornDate date NULL,
	    Email varchar2(50) NULL
	  );
	  -- 5.查询Student表中所有“白银”阶段的学生记录，保存到表HistoryStudent中
	  insert into HistoryStudent select * from student
	  where gradeid = ( select gradeid from grade
	    where gradename = '白银');
	  -- 6.删除Student表中所有“白银”阶段学生记录
	  delete from student where gradeid = ( select gradeid from grade
	    where gradename = '白银');
	  commit; -- 一起成功
	  exception
	    when others then
	    rollback; -- 一起失败
	end;	
***
######视图
1. 视图的由来
	* 对于同一张表中的数据来说，不同的人群可能关注不同的数据，若将不同的数据放在不同的表中会造成表空间的浪费，此时可以使用视图来满足该需求。
	* 视图本质上就是用于展示单个表中部分字段或者多个表中综合字段的虚拟表。
2. 视图的创建和删除
	* 创建视图的语法格式
		***
		create view vw_视图名  
		as  
		select查询语句;  
		如：  
		   -- 创建一个男同学关心的视图: 所有女同学的名字和电话  
		   create view vw_girls  
		   as  
		   select studentname 姓名, phone 电话号码 from student  
		   where sex = '女';  
		
		   -- 查询视图中的数据  
		   select * from vw_girls;  
		***
	* 删除视图的语法格式
		* drop view vw_视图名;
	