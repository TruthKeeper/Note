## Filter

> 过滤器

- 用于完成通用的操作，例如登录验证、统一编码处理、敏感字符过滤

### 使用

- 实现`Filter`接口
- 注解配置`@WebFilter("/*")`（拦截所有资源）
- 放行（责任链模式）：`filterChain.doFilter`

### 执行流程

- 执行放行 上面的代码
- 响应页面
- 执行放行 下面的代码

### 生命周期

- 构造方法和`init`在服务器启动时，只执行一次
- `doFilter`在每次匹配到规则后执行，拦截资源
- `destroy`在服务器正常关闭后，只执行一次

### 配置

#### 拦截路径

- 所有资源拦截：`/*`
- 具体资源拦截：`/index.jsp`
- 目录拦截：`/user/`
- 后缀名拦截：`*.jsp`

#### 拦截方式

- `dispatcherTypes：REQUEST`默认值，浏览器直接请求资源时拦截
- `dispatcherTypes：FORWARD`转发访问资源时拦截
- `dispatcherTypes：INCLUDE`包含访问资源
- `dispatcherTypes：ERROR`错误访问资源
- `dispatcherTypes：ASYNC`异步访问资源

### 执行顺序 

> 注解配置是按照类名的字符串比较规则（每一个字符分别比较）

- 过滤器1
- 过滤器2
- 过滤器执行
- 过滤器2
- 过滤器1

