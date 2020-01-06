🐾 API设计和RESTful

🕘 2019.12.06 由 hoanfirst 编辑


### 一、接口设计一般原则

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

### 二、What is REST

REST is REpresentational State Transfer.

It is architectural style for `distributed hypermedia systems`.（分布式超媒体系统的体系结构风格）


### 三、RESTful's 6 guiding constraints

To be referred as RESTful, a interface must satisfy 6 guiding constraints.

1. **Client-server**

通过将用户界面关注点（user interface concerns）与数据存储关注点（data storage concerns）分离，我们提高了用户界面跨多个平台的可移植性（portability），并通过简化服务器组件提高了可伸缩性（scalability）。


2. **Stateless**

从客户机到服务器的每个请求必须包含理解请求所需的所有信息，并且不能利用服务器上存储的任何上下文（any stored context on the server）。因此，会话状态（session state）完全保存在客户机上。


3. **Cacheable**

缓存约束（cache constraints）要求响应请求中的数据隐式（implicitly）或显式（explicitly）地标记为可缓存（cacheable）或不可缓存（non-cacheable）。如果一个响应是可缓存的，那么客户端缓存就被赋予了重用该响应数据的权利，以用于以后的等效请求。


4. **Uniform interface**

将软件工程的通用性原则（principle of generality）应用于组件接口，简化了整个系统的体系结构，提高了交互的可视性。为了获得统一的接口（uniform interface），需要多个体系结构约束（multiple architectural constraints）来指导组件的行为。

REST由四个接口约束（four interface constraints）定义：

- identification of resources，资源标识；
- manipulation of resources through representations，通过表现形式操纵资源；
- self-description messages，自描述信息；
- hypermedia as the engine of application state，超媒体作为应用状态的引擎；


5. **Layered system**

分层的系统风格通过约束组件行为，使得每个组件不能“看到”与之交互的直接层之外的东西，从而允许架构由分层的层组成（allows an architecture to be composed of hierarchical layers）。


6. **Code on demand（optional）**

REST允许通过以applet或scripts的形式下载和执行代码来扩展客户端功能（extend client functionality）。这减少了需要预先实现（pre-implement）的特性（features）的数量，从而简化了客户端。



### 四、REST关键的信息抽象——Resource

1. Resource相关概念

The key abstraction of information in REST is a `resource`.

REST使用资源标识符，`resource identifier`，来标识组件之间交互中涉及的特定资源。

The state of the resource at any particular `timestamp` is known as `resource representation`.

一个`representation`由数据（data）、描述数据的元数据（metadata describing the data）和超媒体链接（hypermedia links）组成，这些超媒体链接可以帮助客户端过渡到下一个需要的状态。And the data format of a representation is known as a `media type`, the media type identifies a specification that defines how a representation is to be processed.


2. Resource Methods

Another important thing associated with REST is `resource methods` to be used to perform the desired transition.

注意，很多人错误地将Resource Methods与HTTP的GET/PUT/POST/DELETE方法联系起来。

RESTful提出者Roy Fielding从来没有提到过在什么情况下使用哪种方法的建议。他所强调的是：**界面应该是统一的（uniform interface）**。如果开发者决定使用HTTP POST来更新资源，而不是大多数人建议使用HTTP PUT，那么就没有问题，应用程序接口就是RESTful的。

在构建RESTful API时，另一件可以帮助的事情是，基于查询的API结果应该由带有摘要信息的链接列表表示，而不是由原始资源表示的数组表示，因为查询不能代替资源标识。










