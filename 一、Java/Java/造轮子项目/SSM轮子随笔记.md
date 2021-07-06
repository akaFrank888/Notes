# SSM轮子随笔记

## Controller

#### 记录：

（1） 如何实现在Controller层中 return 文件名 就可以跳转页面的？

> 在spring-mvc中配置了视图解析器

```xmll
    <!-- 配置视图解析器
        进行jsp解析，默认使用jstl标签，classpath下得有jstl的包
    -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--配置前缀和后缀，也可以不指定-->
        <property name="prefix" value="/WEB-INF/view/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```



（2）将Java中有关时间的属性设置为Date类型，是为了方便直接通过 new Date() 来set，而与mysql传输数据时，要通过mybatis将Date类型转换成TimeStamp（sql中再添加一个jdbcType），就可以与mysql中的datetime类型匹配了。

（3）如果参数只有一个，如单数据、单对象、map等，则paramType可以不写，因为mybatis会自动识别入参对象。但参数，就比较建议用@param啦。

```java
Public User selectUser(@param(“userName”)String name,@param(“userArea”)String area); 

<select id=" selectUser" resultMap="BaseResultMap"> 
   select  *  from user_user_t   where user_name = #{userName，jdbcType=VARCHAR} and user_area=#{userArea,jdbcType=VARCHAR} 
</select>
```



（4）以后可以借鉴学习hutool的源码，但不要依赖使用hutool

[(2条消息) Hutool不糊涂（二）_jui121314的博客-CSDN博客](https://blog.csdn.net/jui121314/article/details/83036640?utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-3.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-3.control)



#### 待回顾：

1. userStatus有什么用
2. RESTFUL 风格
3. ssm实现web安全
   1. JWT：[jwt加密](https://blog.csdn.net/jerry_guangguangyu/article/details/90713259)
4. 整合上自己之前整理的==设计模式和aop==
5. 搞清楚mapper的工作过程



```xml
    <!--mapper扫描器，为实例化mapper-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--如果需要扫描多个包，中间使用半角逗号隔开-->
        <property name="basePackage" value="pers.yxl.blog.mapper"/>
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    </bean>
```



6. user暂且放在session
7. category先只做一个（一级）
8. 关于数据库：
   1. 尚未建立索引
9. 不写的功能：
   1. “浏览量”这个功能了
   2. “删除标签”“删除分类”
10. PageHelper的源码，为什么其后要紧跟select查询语句



#### 问题：

1. User中的userStatus和userAvatar什么意思？
2. 为什么junit测试前要将以下注释掉，才能正常测试

```xml
     <!--   SpringMVC框架自己处理 静态资源 （而不是交给web容器———— <mvc:default-servlet-handler /> ）-->
    <mvc:resources mapping="/css/**" location="/resource/assets/css/"/>
    <mvc:resources mapping="/js/**" location="/resource/assets/js/"/>
    <mvc:resources mapping="/img/**" location="/resource/assets/img/"/>
    <mvc:resources mapping="/plugin/**" location="/resource/assets/plugin/"/>
```





#### 已解决：

1. mapper.xml中只提取了字段名的sql，不提取表名了。有警告，有找不出来。