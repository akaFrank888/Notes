# SpringMVC拦截器

## 1.1 拦截器（interceptor的作用）

SpringMVC的**拦截器**类似于Servlet开发中的**过滤器**，用于对处理器进行**预处理和后处理**。

将拦截器按一定的顺序连接成一条链，这条链成为**拦截器链（Interceptor Chain）**。在访问被拦截的方法时，拦截器链中的拦截器就会按照被定义的顺序进行调用。（AOP思想）



## 1.2 拦截器和过滤器的区别

| 区别     | 拦截器                                                       | 过滤器                                                  |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------- |
| 使用范围 | 是SpringMVC框架自己的，只有使用了SpringMVC框架的工程才能使用 | 是servlet规范中的一部分，任何JavaWeb工程都能使用        |
| 拦截范围 | 只会拦截访问控制器的**方法**。如果访问的是jsp、html、image、或者js是不会被拦截的。 | 在url=pattern中配置了/*后，可以对所有要访问的资源拦截。 |



## 1.3 拦截器的快速入门

- 创建拦截器类实现HandlerInterceptor接口
- 配置拦截器
- 测试