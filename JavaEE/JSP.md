## JSP

> Java Server Page，java服务器端页面，既可以指定html页面，又可以写java代码，类似模板语言，原理是解析成servlet，方便开发

- `<%  %>`：定义的方法就是在service方法中执行的
- `<%!  %>`：定义的会作为类成员变量
- `<%=  %>`：输出语句

### 内置对象

- request：一起请求访问的多个资源（转发）
- response：响应对象
- out：字符输出流对象，`response.getWriter`数据输出永远在`out.write`之前
- pageContent：当前页面的共享数据，可以获取到其他8个内置对象
- session：一次会话的多个请求间
- application：所有用户间共享数据，服务器
- page：当前页面的Servlet对象，this
- config：Servlet配置对象
- exception：throwable

### EL表达式

> 替换和简化jsp中的java代码，语法${}

- 算数运算符
- 逻辑运算符
- 比较运算符
- 空运算法，empty，判断数组字符串对象是否为空 `${not empty requestScope.list}`

#### 获取值

> 只能从域对象中获取，`${域对象.键名}`，例如`request`域中`setAttribute`了`name`，直接`requestScope.name`获取

- pageScope：即pageContext
- requestScope：即request
- sessionScope：即session
- applicationScope：即application

#### 获取对象的值

> `${域对象.键名.属性名}`

- `${requestScope.user.name}`
- `${requestScope.map["name"]}`
- `${requestScope.list[1]}`