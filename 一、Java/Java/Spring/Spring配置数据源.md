# 1. Spring配置数据源

## 1.1 数据源（连接池）的作用



>  数据源是提高程序性能出现的



- 事先实例化数据源，初始化部分连接资源
- 使用连接资源时从数据源中获取
- 使用完毕后将连接资源归还（close）给数据源

tips：可以理解为一种“环保”的思想——共享单车而不是直接买一个单车

常见的数据源：C3P0，Druid等



## 1.2 非Spring配置步骤

1. 导入三个依赖坐标：
   - mysql-connector：数据库驱动
   - druid：数据源
   - junit：单元测试

2. 新建测试类，创建druid.properties（手动创建druid数据源的优化）
3. 加载数据源



## 1.3 Spring配置数据源步骤



> 将DataSource的创建权交由Spring容器去完成



1. 导入Spring开发的基本包坐标——spring-context
2. 创建Spring核心配置文件——applicationContext
3. 使用Spring的API获得Bean实例



# 2. Spring注解开发

## 2.1 原始注解

> Spring原始注解主要是替代<Bean>的配置

![image-20210119075357905](Spring配置数据源.assets/image-20210119075357905.png)

tips:

1) @Qualifier的括号里写id

2) @Value常搭配SEL表达式来使用,否则就和基础的赋值语句没区别了.

**开发步骤:**

1. **在applicationContext.xml中配置组件扫描**

   作用是指定哪个包及其子包下的Bean需要进行扫描以便识别使用注解配置的类,字段和方法.

2. **注解**



## 2.2 新注解

1. 前言:

   ​    使用原始注解还不能全部代替xml配置文件,还需要使用其他注解来替代如下的配置:

   ![image-20210119093417648](Spring配置数据源.assets/image-20210119093417648.png)

2. 新注解:

​          ![image-20210119093516084](Spring配置数据源.assets/image-20210119093516084.png)