# Spring-MVC获得请求数据



## 获得请求参数

> 格式为：name=value&name=value...

服务器端要获得请求参数，有时还要对数据进行封装，SpringMVC可接受如下类型的参数：

- 基本类型参数
- POJO类型参数
- 数组类型参数
- 集合类型参数



### 1. 获得基本类型参数

> Controller中的业务方法的参数名称要与请求参数的name一致，参数值会自动映射匹配！



### 2. 获得POJO类型参数

> SpringMVC会自动将参数匹配映射到user中，前提是pojo中的属性与参数名称相同。



### 3. 获得数组类型参数

>  也要相同



### 4. 获得集合类型参数

> 若前端将集合转成了json类型，则服务器端可以通过在方法中添加注解和参数即可



### 5. 获得Restful类型

Restful是一种软件 架构风格、设计风格 ，而不是标准，只是提供了一组设计原则和约束条件。主要用于客户端和服务端交互类的软件。

其格式为“url+请求方式”，HTTP协议里面四个表示操作方式的动词如下：

- GET：获取资源		 	/user/zhangsan  GET		得到name=zhangsan的user
- POST：新建资源 	 	/user  POST                       新增user
- PUT：更新资源			/user/zhangsan  PUT       更新name=zhangsan的user
- DELETE：删除资源	  /user/zhangsan  DELETE		删除name=zhangsan的user



### 6. 获得请求头信息

### 7.获得请求头中的cookie



### 8. 文件上传

#### 1） 文件上传  客户端三要素

- 提供\<input type='file' name='uploadFilename'/>的文件上传输入框
- form表单的method属性设置为post
- form表单的enctype属性设置为multipart/form-data





#### 2）单文件上传的步骤

1. 导坐标（fileupload和io）
2. 配置文件上传解析器
3. 编写文件上传代码