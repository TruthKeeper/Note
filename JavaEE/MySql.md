### MySql常用语句

- 开启服务：`net start mysql`
- 关闭服务：`net stop mysql`
- 登录数据库：`mysql -u root -p`
- 退出：`exit` `quit` 
- 单行注释 --  多行注释 /* */

### CRUD

- create  创建 `create database 数据库名`
- retrieve 查询
- update  更新
- delete  删除 `drop database 数据库名` `drop table 表名`

### 插入语句

> `insert into 表名(列名1,列名2) values(值1,值2)`

除了数字类型，其他类型都要用引号（单双引号）修饰

### 删除语句

> delete from 表名 [where 条件]

- 如果不加条件将会删除表的全部记录，效率较低
- 清空表 `truncate table 表名`删除表并且创建一个一模一样的表

### 修改语句

> `update 表名 set 列名1 = 值1 [where 条件],列名2 = 值2 `

- 如果不加条件将会将所有记录全部修改

### 查询语句

- `select * from 表名`
- `selete 列名1,列名2 from 表名`

#### 去重

> `selete distinct 列名1,列名2 from 表名`

`distinct` 作用于多列，**当查询结果集一致才去重**

#### 计算

> 一般用四则运算符计算数值型

- `SELECT NAME,math,english,IFNULL(math,0)+IFNULL(english,0) as total FROM USER`
- `IFNULL`第二个条件放null时的代替值
- `as` 作为别名，也可以省略

#### 条件查询

- `where age >= 20`

- `where age = 20`

- `where age != 20` `where age <> 20`

- `where age >= 20 and age <= 30` `where age between 20 and 30` **between and 包含边界**

- `where age in (20,21,26)`

- `where score is null` `where score is not null` **null不能用= 和!=判断，只能用is**

##### LIKE

- 姓张的 `where name like '张%'`
- 第二个叫三的 `where name like '_三%'`
- 三个字的 `where name like '___'`
- 包含哈的 `where name like '%哈%'`

#### 排序查询

> order by 列名 

- `select * from user order by age` 默认生效
- `select * from user order by age desc`  降序
- `select * from user order by math,english` **第一条件相同时才按照后续排序条件**

#### 聚合函数

> 将一列的结果看成一个整体，会自动排除null，所以根据需求选择不包含非空的列来计算，或者用IFNULL函数

##### count、max、min、sum、avg

> 计算个数、最大值、最小值、求和、平均值

- `select count(name) from user`
- `select max(IFNULL(math,0)) from user`
- `select min(IFNULL(math,0)) from user`
- `select sum(IFNULL(math,0)) from user`
- `select avg(IFNULL(math,0)) from user`

##### 分组查询

> group by

- 分组查询的字段只能是分组的字段或者聚合函数，因为分组查询的结果就是一个整体
- `select sex,avg(age) from user group by sex`各个性别的平均年龄
- `select sex,avg(age) from user where age > 18 group by sex`各个性别大于18岁的平均年龄
- `select sex,avg(age) from user where age > 18 group by sex having avg(age)>60`各个性别大于18岁的平均年龄，最后取大于60岁的结果

##### WHERE和HAVING

- **where**在分组查询之前进行限定，不满足条件则不参与分组查询；**having**在分组查询后进行限定，不满足结果的不会被查询出来
- **where**后面不能跟聚合函数，而**having**可以进行聚合函数的判断

#### 分页查询

- `select * from user limit 0,3` 含义是第0条开始取3条数据
- `select * from user limit 3,3` 含义是跳过前3条，第4条开始取3条数据
- `select * from user limit n*page,page` n：当前页码，page分页条数
- `select * from user limit 3 offset 0` 含义是第0条开始取3条数据
- `select * from user limit 3 offset 3` 含义是跳过前3条，第4条开始取3条数据
- `select * from user limit page offset n*page` n：当前页码，page分页条数

#### 多表查询

> select * from user,teacher   返回笛卡尔积结果（所有组合情况）

##### 内连接查询

> 要知道查询的表，知道查询条件，知道查询的字段

- 隐式内连接：用where

- ```mysql
  SELECT
  	t1.name,t2.name location_name
  FROM
  	user t1,location t2
  WHERE
  	t1.location_id=t2.id
  ```

- 显示内连接 **`JOIN [] ON []`**

- ```mysql
SELECT
	t1.name,t2.name location_name
FROM
	user t1
INNER JOIN
	-- INNER可不写
location t2
	ON
	t1.location_id=t2.id
	```

##### 外连接查询 LEFT JOIN [] ON []

> 除了返回条件匹配的数据，那些不满足的数据也会返回，例如NULL

- 左外连接：以左表为基表连接右表，在右表中若没有匹配到基表中的数据则返回 null

- ```mysql
  SELECT
  	t1.*,t2.name location_name
  FROM
  	user t1
  LEFT OUTER JOIN
  -- outer可不写
  	location t2
  ON
  	t1.location_id=t2.id
  ```

- 右外连接：雷同

##### 子查询

> 查询语句中嵌套查询语句

###### 1.单行单列的结果

> 四则运算符号处理

```mysql
SELECT
	*
FROM
	user
WHERE
	english = (
	SELECT
		MAX(english)
	FROM
		user
	)
```

###### 2.多行单列的结果

> IN关键字处理

```mysql
SELECT
	*
FROM
	user
WHERE
	location_id IN (
	SELECT
		id
	FROM
		location
    WHERE
        name = '杭州' or name = '北京'
	)
```

###### 3.多行多列的结果

> 作为一张虚拟表来做内外连接或者子查询

```mysql
-- 查询员工入职日期是2000-00-00后的员工信息和部门信息
SELECT
	*
FROM
	dept t1,(SELECT * FROM emp WHERE emp.join_date > '2000-00-00') t2
WHERE
	t1.id=t2.dept_id
```

### 约束

#### 主键约束

> primary key

- 表中记录的唯一标识
- 删除主键 `alter table user drop primary key`
- 自增长 `auto_increment`

#### 非空约束

> not null

#### 唯一约束

> unique

在MySql中，可以有多个null

#### 外键约束

> foreign key 使表与表之间产生关系，保证数据的正确性，会降低数据库的性能

- `constraint 外键名称 foreign key 外键列名称 references 主表名称(主表的列名称)`

- `CONSTRAINT fk_class_id FOREIGN KEY (class_id) REFERENCES classes (id)`

### 表设计

- 一对一，可以在任意一方添加**唯一外键**到另一张表的主键
- 一对多，一个员工对应一个部门，一个部门可以有多个员工，**多的表**通过外键关联另一张表的主键
- 多对多，一个学生可以有多个课程，一个课程可以有多个学生，**设计的时候需要第三张中间表记录对应的关系**

### 范式

#### 第一范式（1NF）

> 每一列都是不可分割的原子项 

#### 第二范式（2NF）

> 非码属性必须完全依赖于候选码，（在1NF的基础上消除非主属性对主码的部分函数依赖）

- 函数依赖，如果通过A属性可以确定唯一的B属性（属性组），则称B依赖于A，例如用户id（A）和用户名称（B）
- 完全函数依赖，如果A是一个属性组，则B属性值需要依赖于A属性组中的每一个属性值，例如学生id、学生课程id --- 课程分数
- 部分函数依赖，如果A是一个属性组，则B属性值只需要依赖于A属性组中的部分属性值，例如学生id、学生课程id --- 学生姓名
- 传递函数依赖，如果通过A属性（属性组）的值，可以确定唯一B属性的值，在通过B属性（属性组）的值可以确定唯一C属性的值，则称C传递函数依赖于A，例如学生的课程id（A）--- 课程的分类id --- 分类名称
- 如果在一张表中，一个属性或属性组被其他属性所完全依赖，则称这个属性（属性值）为该表的码

#### 第三范式（3NF）

> 任何非主属性不依赖于其他非主属性（在2NF的基础上消除传递依赖）

### 事务

> 一个包含多个步骤的业务操作，要么同时成功，要么同时失败

- 开启事务：start transaction
- 回滚：rollback
- 提交：commit
- 如果开启事务之后没有提交，那数据只是临时态，不是持久态
- MySQL的DML（增删改）语句默认自动提交一个事物，可通过`select @@autocommit`查看是否是自动提交模式（1：自动提交），可通过`set @@autocommit = 0 `改为手动提交

#### 四大特征

- 原子性：不可再分割的最小操作单位
- 持久性：事务一旦提交或者回滚，数据库会持久化的保存数据
- 隔离性：多个事务之间相互独立
- 一致性：事务操作前后，数据总量不变

#### 隔离级别

> 多个事物之间相互独立，但是多个事务操作同一批数据，会引发一些问题，需要设置不同的隔离级别

存在问题

- 脏读：一个事物读取到另一个事务中没有提交的数据
- 不可重复读（虚读）：在同一个事务中，两次读取的数据不一样
- 幻读：一个事务操作表中所有的记录，另一个事物添加了一条数据，则第一个事务查询不到自己的修改

级别：

- read uncommitted：读未提交
- read commited：读已提交（Oracle默认）：能解决脏读
- repeatable read：可重复读（MySQL默认）：能解决不可重复读
- serializable：串行化：能解决所有问题，相当于加锁，会等待

