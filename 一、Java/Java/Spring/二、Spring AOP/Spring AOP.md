# 3. Spring AOP

## 3.1 AOP

### 3.1.1 什么是AOP

> AOP的全称是 Aspect Oriented Programming, 即面向切面编程.它是面向对象编程(OOP)的一种补充(不是替代),目前已成为一种比较成熟的编程方式.

​    AOP采取横向抽取机制,将分散在各个方法中的重复代码提取出来,然后在程序编译或运行时,再将这些提取出来的代码应用到需要执行的方法.

​    AOP的使用,使开发人员在编写业务逻辑时可以专心于核心业务,不但提高了开发效率,而且增强了代码的可维护性.

### 3.1.2 AOP术语

- Aspect(切面): 指<u>封装的用于横向插入系统功能(事务,日志)的类.</u>
- Pointcut(切入点):指<u>切面与程序流程的交叉点</u>.如某个通知要应用到所有以add开头的方法中,那么所有满足这一规则的方法都是切入点.
- Advice(通知/增强处理):指<u>切面类(代理类)中的方法.它是切面的具体实现.</u>
- Target Object(目标对象):指<u>委托类</u>.
- Weaving(织入):指<u>将切面代码插入到目标对象上,从而生成代理对象的过程.</u>

## 3.2 静态代理与动态代理

### 3.2.1 引入

- 动态代理:狱警. 如果想取另外一个包裹,告诉狱警（犯人的分身）去做就好.过程中出现任何变动,都仅仅影响分身.
- 静态代理:吃药.我吃了一种药,胳膊编的无限长(超出原来的部分就是静态代理),我用自己的胳膊去取.如果被炸弹炸了,我的胳膊就没了.

所以,整个过程我们认识到:

​		动态代理我还是原来的我.而静态代理实现这个功能,已经把我变成怪物了.

### 3.2.2 创建代理的时间

- 静态代理:是程序员已经编写好的特定的代理类.所以程序运行前,类的.class文件就已经存在了.
- 动态代理:在程序运行时,运用反射机制动态地创建而成.

## 3.3 JDK动态代理

### 3.3.1 代码

1) 目标对象类: UserDao.java 

```java
public class UserDaoImpl implements UserDao {
    public void save() {
        System.out.println("UserDao is saving");
    }
}
```

2) 切面类: MyAspect.java

```Java
/**
 * @Auther: yxl15
 * @Date: 2021/1/20 09:52
 * @Description:
 *
 *      切面类：其中有系统功能的方法
 */
public class MyAspect {

    public void authorize() {
        System.out.println("权限检查");
    }

    public void log() {
        System.out.println("打印日志");
    }
}
```

3) 代理类: MyProxy.java

```
/**
 * @Auther: yxl15
 * @Date: 2021/1/20 09:54
 * @Description:
 *
 *        代理类：用于创建代理对象（动态JDK代理）
 */
public class MyProxy implements InvocationHandler {

    // 委托类
    public Object object;

    public Object createProxy(Object object) {
        this.object = object;

        // 创建代理对象
        // 参数1：目标对象的类加载器 或者是 代理类的类加载器都可以！
        // 参数2：目标对象实现的接口
        // 参数3：this
        return Proxy.newProxyInstance(object.getClass().getClassLoader(),
        	object.getClass().getInterfaces(), 
        	this);
        // return Proxy.newProxyInstance(this.getClass().getClassLoader(), 
        	object.getClass().getInterfaces(), 
        	this);
    }

    /**
     *
     * @description: 实现接口中的invoke方法
     *
     * @param:
     *          proxy：被代理后的对象；
     *          method：将要被执行的方法（反射）；
     *          args：执行方法所需的参数
     *
     * @return: 反射后产生的对象
     * @date: 2021/1/20 10:21
     *
     */

    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        // 声明切面
        MyAspect aspect = new MyAspect();
        // 代理前的增强处理
        aspect.authorize();
        // 代理
        Object o = method.invoke(object, args);
        // 代理后的增强处理
        aspect.log();

        return o;
    }
}
```

4）测试

```java
    public static void main(String[] args) {
        // 创建代理对象
        JDKProxy proxy = new JDKProxy();
        // 创建目标对象
        UserDao user = (UserDao) proxy.createProxy(new UserDaoImpl());
        // 调用方法
        user.save();
    }
```



### 3.3.2 代码分析

在代理类MyProxy.java中:

​	1)实现了InvocationHandler接口

​	2)编写了创建代理对象的方法

​		<font color='red'>注意:	注释里关于的三个参数</font>

​	3)实现了该接口中的invoke()方法,所有动态代理类所调用的方法都会交由该方法进行处理.

​		<font color='blue'>注意:	invoke属于回调函数,执行的是形参对象中的方法.并且,回调函数不是由该函数的实现方直接调用,而是在特定的时间或条件发生时,由另外一方调用的.</font>



## 3.4 CGLIB代理

### CGLIB简介

​	JDK动态代理有一定的局限性——委托类必须实现一个或多个接口。如果要对没有实现接口的类进行代理，则可以使用CGLIB代理。

​	**CGLIB**（Code Generation Library）是一个高性能开源的代码生成包。它采用非常底层的字节码技术，对指定的目标类型生成一个子类，并对子类进行增强。

### 3.4.1 代码

1）目标对象类：CglibDao.java（不需要实现接口）

```java
public class CglibDao {

    public void add() {
        System.out.println("add...");
    }

    public void delete() {
        System.out.println("delete...");
    }

}
```

2）切面类：MyAspect.java

```java
/**
 * @Auther: yxl15
 * @Date: 2021/1/20 09:52
 * @Description:
 *
 *      切面类：其中有系统功能的方法
 */
public class MyAspect {

    public void authorize() {
        System.out.println("权限检查");
    }

    public void log() {
        System.out.println("打印日志");
    }
}
```



3）代理类：CglibProxy.java

```java

/**
 * @Auther: yxl15
 * @Date: 2021/1/21 21:41
 * @Description:
 *
 *        代理类：用于创建代理对象（CGLIB代理）
 */
public class CglibProxy implements MethodInterceptor {


    public Object createProxy(Object target) {
        // 创建一个动态类对象
        Enhancer enhancer = new Enhancer();
        // 设置需要增强的类为enhancer的父类
        enhancer.setSuperclass(target.getClass());
        // 添加回调函数
        enhancer.setCallback(this);
        // 返回创建的代理类
        return enhancer.create();
    }

    /**
     *
     * @description: 实现接口中的intercept方法
     *
     * @param:
     *                proxy：被代理后的对象
     *                method：将要被执行的方法（反射）  （拦截的方法）
     *                args：执行方法所需的参数        （拦截的参数）
     * @return:
     *              反射后产生的对象
     *
     * @date: 2021/1/21 21:47
     *
     */

    public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {

        // 声明切面
        MyAspect aspect = new MyAspect();
        // 代理前的增强
        aspect.authorize();
        // 代理
        Object obj = methodProxy.invokeSuper(proxy, args);
        // 代理后的增强
        aspect.log();

        return obj;
    }
}

```

4）测试

```java
    public static void main(String[] args) {
        // 创建代理对象
        CglibProxy cglibProxy = new CglibProxy();
        // 创建目标对象
        CglibDao cglibDao = (CglibDao) cglibProxy.createProxy(new CglibDao());
        // 执行方法
        cglibDao.add();
        cglibDao.delete();
    }
```



### 3.4.2 代码分析



## 3.5 Spring的AOP

### 3.5.1 Spring AOP开发明确的事项

#### 1. 需要编写的内容

- 编写核心的业务代码（目标类和目标方法）
- 编写切面类（切面类中有通知/增强的方法）
- 在配置文件中，编写织入关系（即将哪些通知与哪些连接点进行结合）

#### 2. AOP技术实现的原理

Spring框架<u>监控切入点方法的执行</u>，一旦监控到切入点方法被执行，就<u>使用代理机制，动态创建目标对象的代理对象</u>。并根据通知的类别，在代理对象的对应位置，<u>将通知对应的功能织入</u>，完成完整的代码逻辑运行。

#### 3. AOP底层使用哪种代理方式

在Spring中，框架会<u>根据目标类是否实现了接口</u>来决定采用哪种动态代理的方式。

### 3.5.2 Spring AOP开发的知识要点

- AOP
- AOP的两种底层实现
- AOP的几个概念
  - Pointcut（切入点）：被增强的方法
  - Advice（通知）
  - Aspect（切面）：切点+通知
  - Weaving（织入）：将切点与通知结合的过程
- 明确事项
  - 谁是切点（切点表达式配置）
  - 谁是通知
  - 如何将切点和通知进行织入配置