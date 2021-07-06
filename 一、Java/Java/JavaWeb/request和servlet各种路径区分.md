# request各种路径区分



## 一、request获取

| 方法                                                         | 解释                                                         | 例子                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ | :------------------------------------------ |
| request.getRequestURL()                                      | 访问资源的全路径                                             | http://localhost:8080/pathTest/test         |
| request.getRequestURI()<br />或<br />request.getServletPath() | 访问资源的部分路径（除去host）                               | /pathTest/test                              |
| request.getContextPath()                                     | 工程的虚拟路径（若虚拟路径为/，则返回空）                    |                                             |
| request.getServletContext().getRealPath("/")                 | servlet容器（tomcat）的输出路径（可在Artifacts中的output directory设置） | D:\Java\project\SpringDemo\spring_mvc\src\s |