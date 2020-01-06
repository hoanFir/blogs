🐾 API设计和RESTful

🕘 2019.12.06 由 hoanfirst 编辑


### 接口设计一般原则

1. 动宾

RESTful的核心思想就是，客户端发送的请求，即数据操作指令，为“动词+宾语”的结构。例如`get/articles`。

动词通常就是五种HTTP请求方法：`GET`、`POST`、`DELETE`、`PUT`、`PATCH`，对应CRUD操作（增加(Create)、读取(Retrieve)、更新(Update)和删除(Delete)）。

2. 宾语必须是名词或者动名词

宾语是HTTP中动词作用的对象，例如`/articles`是正确示范，而诸如`/getAllCars`、`/CreateNewCar`、`/deleteAllRedCards`均是错误的。

3. 基于区分对象的特定操作要求

在某些情况下，需要区分对象的特定操作，可以将URL设计为如`/car/del`的形式。

4. 避免多级URL

比如，要获取ID为12的用户其创建的类型为2的所有文章，有时会设计成`get/authors/12/categories/2`，但这样的URL非常不利于扩展，语义也不明确，因此会导致开发成本的提升。

对于多级URL，推荐除第一级，其他级别使用查询字符串来表达，如`get/authors/12?categories=2`。

### What is REST

REST is REpresentational State Transfer.

It is architectural style for `distributed hypermedia systems`.（分布式超媒体系统的体系结构风格）


### RESTful's 6 guiding constraints

To be referred as RESTful, a interface must satisfy 6 guiding constraints.

1. 


