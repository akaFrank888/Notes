# 4. 基于XML的AOP开发

## 4.1 快速入门

> 步骤

1. <font color='red'>导入AOP的相关依赖（坐标）</font>
3. 创建目标接口和目标类
4. 创建切面类MyAspect
5. <font color='red'>将目标类和切面类的对象创建权交给Spring容器</font>

5. <font color='red'>在applicationContext中配置织入关系</font>
6. 测试代码（记得要导入spring的测试坐标）

> 解释：

1. spring-context坐标中本身就有Spring的AOP包，但实用性并不如AspectJ，又因为Spring不排斥其他优秀的框架，所以我们导入aspectjweaver坐标，即实际上用的是AspectJ进行AOP开发。

2. 步骤5是最重要也是最复杂的。它是在Spring容器中告诉Spring框架哪些方法（切点）要被哪些增强（通知）。

3. 详解步骤5：

   a. 先在Spring容器中引入AOP的命名空间和AOP约束的位置

   b. 再写哪些方法（切点）需要进行哪些增强（前置通知/后置通知）



不过，还是建议用注解来取代Spring配置文件为实现AOP所配置的臃肿的代码。



# 5. 基于注解的声明式AspectJ

> 步骤

1. 创建目标接口Target和目标类TargetImpl（内有切点）
2. 创建切面类MyAspect（内有增强方法）

3. 将目标类和切面类的对象创建权交给Spring（即添加@Component注解）
4. 在切面类中使用注解，配置织入关系（@Before和@After）
5. 在配置文件中开启组件扫描（context）和AOP自动代理（aop）
6. 测试



> 知识点

- 注解类型：

  @Aspect

  @Pointcut

  @Before

  @AfterReturning

  @Around

  @AfterThrowing

  @After

  

- 切点表达式的写法



> 代码

- 在resource包下创建aopAnnotion.xml（记得先配置context和aop的命名空间）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd

">

    <!--context的组件扫描-->
    <context:component-scan base-package="aop_annotion"/>
    <!--aop自动代理-->
    <aop:aspectj-autoproxy/>

</beans>
```



- Target.java

```java
public interface Target {

    void save();
}
```



- TargetImpl.java

```java
// 将该类的创建权交给Spring
@Component("target")
public class TargetImpl implements Target {
    public void save() {
        System.out.println("saving is running");
    }
}
```



- MyAspect.java

```java
// 将该类的创建权交给Spring
@Component("myAspect")
// 标注该类是一个切面类
@Aspect

public class MyAspect {

    // 用注解定义切点表达式
    @Pointcut("execution(* aop_annotion.*.*(..))")
    // 用一个返回值为void，方法体为空的方法命名切入点
    private void pointCut() {}


    // 前置通知
    @Before("pointCut()")
    public void before() {
        System.out.println("前置增强——————权限检查");
    }

    // 后置通知
    @AfterReturning(value = "pointCut()")
    public void afterReturning() {
        System.out.println("后置增强——————记录日志");
    }

    // 环绕通知
    @Around("pointCut()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕前增强");
        // 执行当前目标方法
        Object proceed = pjp.proceed();
        System.out.println("环绕后增强");

        return proceed;
    }

    // 异常通知
    @AfterThrowing(value = "pointCut()",throwing = "e")
    public void afterThrowing(Throwable e) {
        System.out.println("异常抛出增强");
    }

    // 最终通知
    @After("pointCut()")
    public void after() {
        System.out.println("最终增强——————释放资源");
    }

}
```



- AnnoTest.java

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:aopAnnotion.xml")
public class AnnoTest {

    @Resource
    private Target target;

    @Test
    public void test1() {
        target.save();
    }
    
}
```



执行顺序：

<font color='red'>**Around --> Before --> Around --> After --> AfterReturning --> AfterThrowing**</font>



> 注解详解 

1. 不管目标方法有无异常，最终通知（@After）一定会执行，而后置通知（AfterReturning）会因为异常而不执行

2. 环绕增强与前后置增强的区别？

   ​	环绕增强是在调用业务方法之前和调用业务方法之后都可以执行响应的增强语法，也就是说：一个前置增强和一个后置增强相当于是组成了一个环绕增强；不同的是，在前置增强和后置增强中，在AOP中前置增强和后置增强只能拿到JoinPoint类，而在环绕增强中，可以拿到ProceedingJoinPoint类；

   ​	**ProceedingJoinPoint类继承了JoinPoint类，也继承了JoinPoint所有的非私有的方法；比如获取连接点相关信息、获取参数信息、获取方法等等，并且ProceedingJoinPoint类扩展了JoinPoint类的方法，ProceedingJoinPoint可以调用业务方法执行业务逻辑（即调用目标方法），而JoinPoint则不可以；**

   ​	**也就是在环绕增强中，可以执行业务方法，而在前置增强和后置增强中则不可以；这里可以通过环绕增强实现数据库事务的实现，也可以通过环绕增强实现程序运行时间的记录；**
   
   （同时也会发现，最终通知的方法没有参数，而后置通知有JoinPoint参数）