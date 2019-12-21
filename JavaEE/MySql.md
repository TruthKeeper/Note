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

> foreign key 使表与表之间产生关系，保证数据的正确性

- `constraint 外键名称 foreign key 外键列名称 references 主表名称(主表的列名称)`

- `CONSTRAINT fk_class_id FOREIGN KEY (class_id) REFERENCES classes (id)`


