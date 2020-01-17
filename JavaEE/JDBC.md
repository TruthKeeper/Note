## JDBC

### 概念

> Java语言操作数据库

```java
        //1.导入MySQL jar包
        //2.注册驱动（会在静态代码块中自动注册）（MySQL5之后可以省略）
        Class.forName("com.mysql.cj.jdbc.Driver");
        //3.获取数据库连接对象，其中默认的localhost:3306可以省略
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/db1?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8",
                "root", "123456");
        //4.定义SQL语句
        String sql = "UPDATE student SET age = 18 WHERE NAME = '张三'";
        //5.获取Statement执行对象
        Statement statement = connection.createStatement();
        //6.执行SQL并获取返回码
        int result = statement.executeUpdate(sql);
        //7.处理结果
        System.out.println(result);
        //8.释放资源
        statement.close();
        connection.close();
```

### API

#### 预编译SQL

> 有效防止SQL注入

```java
PreparedStatement statement = connection.prepareStatement("select * from student where name = ?");
statement.setXXX(?的位置编号, ?的值);
```



#### 管理事务

- 开启事务，`connection.setAutoCommit(false);`
- 提交事务，`connection.commit()`
- 回滚，`connection.rollback()`

#### 执行

- `executeUpdate`：可以执行DML语句（insert，delete，update），DDL语句，返回值是影响的行数，大于0即成功
- `executeQuery`，可以执行DQL语句（select），返回结果集对象

### 数据库连接池

#### C3P0

#### Druid

```java
public class JDBCUtils {
    public static DataSource dataSource;

    static {
        Properties properties = new Properties();
        try {
            properties.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            dataSource = DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    public static DataSource getDataSource() {
        return dataSource;
    }
}
```

```properties
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/db1?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8
username=root
password=123456
initialSize=5
maxActive=10
maxWait=5000

```

```java
public class UserDao {
    private final JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());

    public User login(String username, String password) {
        String sql = "select * from user where username = ? and password = ?";
        List<Map<String, Object>> list = template.queryForList(sql, username, password);
        if (list == null || list.isEmpty()) {
            return null;
        } else {
            return JSON.parseObject(JSON.toJSONString(list.get(0)), User.class);
        }
    }
}
```



### Spring JDBC

>Spring 框架对于JDBC的封装

- `new JdbcTemplate(dataSource)`
- `update`：执行DML语句
- `queryForMap`返回`Map<String,Object>`
- `queryForList`，返回Map集合的List
- `query`将返回结果封装成bean，通过BeanPropertyRowMapper的常见实现类
- `queryForObject`：返回查询结果，常用于聚合函数

