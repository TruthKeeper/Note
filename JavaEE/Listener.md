## Listener

> 事件监听机制，围绕事件源，事件，监听器，注册监听展开

### ServletContextListener

>监听 ServletContext 对象的生命周期，实际上就是监听 Web 应用的生命周期。

主要有两个方法，一个在当`Servlet`容器启动web应用时调用，另一个是在`Servlet`容器终止web应用时调用。实现`ServletContextListener`接口需重写这两方法

- `contextInitialized`：当`Servlet`容器启动web应用时调用
- `contextDestroyed`：当`ServletContext`被销毁之前调用



启动web应用时获取context-param web环境参数，然后配置到context的attribute里，在servlet或者jsp中通过getServletContext().getAttribute(“**”)来获取存初始配置的参数。

可用于网站访问量统计

