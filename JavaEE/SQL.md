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

### 查询语句

- `select * from 表名`

### 删除语句

> delete from 表名 [where 条件]

- 如果不加条件将会删除表的全部记录，效率较低
- 清空表 `truncate table 表名`删除表并且创建一个一模一样的表

### 修改语句

> `update 表名 set 列名1 = 值1 [where 条件],列名2 = 值2 `

- 如果不加条件将会将所有记录全部修改

### 查询语句

- `selete 列名1,列名2 from 表名`

#### 去重

> `selete distinct 列名1,列名2 from 表名`

distinct 作用于多列，当查询结果集一致才去重

#### 求和

- `SELECT NAME,math,english,IFNULL(math,0)+IFNULL(english,0) FROM USER`

