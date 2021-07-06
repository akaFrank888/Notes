# Spring集成web环境

## 一、ApplicationContext的获取方式

```
背景：
	应用上下文原来是通过new ClasspathXmlApplicationContext方式获取的，但每次从容器中获得Bean时都要再获取一便。所以，弊端就是配置文件加载多次，应用上下文对象创建多次。

改进：
	可以考虑在web应用启动时，就加载Spring的配置文件，创建应用上下文对象ApplicationContext，再将该对象存储到servletContext域中。这样就可以在任意位置从域中获得ApplicationContext对象了。

如何实现？
	在web项目中，使用ContextLoaderListener监听web应用的启动。
	
Spring中是否集成了该功能呢？是的！
	spring-web包下的ContextLoaderListener类即为监听器，再配合全局初始化参数将app存进servletContext，便可在servlet类中通过包下的WebApplicationContextUtils获取app！
	
```

而同理，我们就可以理解怎么读取 spring-mvc.xml。因为前端控制器需要spring-mvc.xml中的配置的controller注解，所以要在web.xml中的前端控制器中去配置。 



## 二、开发步骤

1. 导入SpringMVC相关坐标
2. 配置SpringMVC的前端控制器DispatcherServlet
3. 配置SpringMVC的核心文件spring-mvc.xml
4. 创建Controller类和jsp，并使用注解配置Controller类中业务方法的映射地址
5. 客户端测试

