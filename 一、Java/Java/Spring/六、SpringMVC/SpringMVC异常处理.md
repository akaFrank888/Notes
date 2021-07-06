# SpringMVC异常处理

## 1.1 异常处理的思路

> 系统的DAO、Service和Controller出现异常后，都通过throw Exception向上抛出，最后由SpringMVC前端控制器交给异常处理器进行异常处理。(恰好与请求顺序相反)



![image-20210323153425060](image/image-20210323153425060.png)



## 1.2 异常处理的方式

- SpringMVC已经定义好的 简单映射异常转换器

![image-20210324175217967](image/image-20210324175217967.png)



- 自定义异常处理器

（比较麻烦）