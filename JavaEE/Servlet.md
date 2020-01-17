## Servlet

> server applet，运行在服务端的小程序，就是一个接口，实现了Java类被浏览器访问到(tomat识别)的规则

- 定义类，实现`Servlet`接口
- 在web.xml中配置
```xml
    <servlet>
        <servlet-name>demo1</servlet-name>
        <servlet-class>com.yuese.test.ServlatTest1</servlet-class>
		<!--指定Servlet创建时机，负数为第一次被访问，0或正整数为服务器启动
        <load-on-startup></load-on-startup>
		-->
    </servlet>
    <servlet-mapping>
        <servlet-name>demo1</servlet-name>
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>
```
#### 原理

- 当服务器接收到客户端浏览器的请求后，会解析请求的url地址，获取访问的Servlet的资源路径
- 查找web.xml文件，是否有对应的<url-parteern>标签体内容
- 如果有，则再找到对应的<servlet-class>全类名
- tomact会将字节码文件加载进内存，并且创建其对象
- 调用方法

#### 生命周期

- init：当`Servlet`被创建时回调，只执行一次，在内存只存在一个对象，多线程时存在安全问题
- service：提供服务方法，每一次被访问都会执行
- destory：销毁方法，当服务器**正常关闭**时执行

### GenericServlet

> `Servlet`的抽象实现，空实现了接口的方法

### HttpServlet

> 对HTTP协议的封装，简化操作

### 3.0+

> 支持注解配置，可以不创建web.xml

- 类注解`@WebServlet("/demo2")`

### 4.0+

### 相关配置

#### urlpattern

> 一个`Servlet`可以定义多个访问路径`@WebServlet({"/a","/ab","/abc"})`

##### 规则

- /xxx
- /xxx/xxx：多层路径，可以用*来通配
- *.do

### Request对象

- `getMethod`：请求方式GET，POST
- `getQueryString`：获取get方式的请求参数
- `getContextPath`：获取项目的根路径，"/adc"
- `getRequestURI`：获取请求的URI，"/abc/def"
- `getRequestURL`：获取请求的URL，"http://localhost/abc/def"
- `getProtocol`：获取协议及版本 HTTP/1.1
- `getRemoteAddr`：获取客户机的IP地址
- `getHeader`：获取请求头的值
- `getReader().readLine()`获取字符的输入流（仅post的请求体）
- `getParameter`：获取请求参数的值
- `getParameterValues`：获取请求参数的值数组

#### 中文乱码

get方式 tomcat已经处理了

- post需要加上`request.setCharacterEncoding("utf-8")`
- response写中文输出，`response.setContentType("text/html;charset=utf-8")`，告诉浏览器应该使用的编码，默认是ISO-8859-1

#### 请求转发

> 服务器内部的资源跳转方式，`request.getRequestDispatcher("/abc").forward(request,response)`

- 浏览器地址栏不发生变化
- 只能访问当前服务器的内部资源
- 只有一次HTTP请求

#### 请求重定向

- 浏览器地址栏发生变化
- 可以访问其他站点的资源
- 2次HTTP请求，（不能用request来共享数据）

#### 共享数据

> 一个request域代表一次http请求的范围，一般用于请求转发的多个资源中共享数据

- `setAttribute`：存储数据
- `getAttribute`：获取数据
- `removeAttribute`：移除数据

### Response对象

- `sendRedirect`：重定向到指定路径

#### 验证码案例

> 通过`BufferedImage`和`ImageIO.write`写入到输出流

### ServletContext

> 代表整个web应用，可以和程序的容器（服务器）通信，通过request对象或者httpservlet的getServletContext来获取到



- 获取MIME类型，text/html，image/jpeg
- 域对象：共享数据（所有request域）
- 获取文件的真实（服务器）路径
- web目录下的文件访问：`new File(getServletContext().getRealPath("/a.txt"))`
- WEB-INF目录下的文件访问：`new File(getServletContext().getRealPath("/WEB-INF/b.txt"))`
- src目录下的文件访问：`new File(getServletContext().getRealPath("/WEB-INF/classes/c.txt"))`

### 文件下载的需求

- 将超链接的`href`指向`servlet`，并且传递资源名称
- 向字节输入流中加载文件到内存
- 指定**response**的响应头，`content-type`指定文件的`mimetype`，`content-disposition`指定`attachment;filename=${文件名}`
- 中文文件名称问题：需要用url编码，火狐是base64