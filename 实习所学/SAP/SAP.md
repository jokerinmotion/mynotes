# 项目中所学💡

## Swager/Spring Fox💡













## SOAP请求

**SOAP（Simple Object AccessProtocol）简单对象访问协议**。soap请求 (Simple Object Access Protocol，简单对象访问协议) 是HTTP POST的一个专用版本，遵循一种特殊的xml消息格式Content-type设置为: text/xml任何数据都可以xml化。

所有的 SOAP 消息发送都使用 HTTP POST 方法，并且所有 SOAP 消息的 URI 都是一样的，这是基于 SOAP 的 Web 服务的基本实践特征。



## 技术点

### oauth2

token解码后，能得到一个scope属性，即对各个ms的权限。权限都在WebSecurityConfig类下设置

### ThreadLocal

### FeignClient

![image-20220817103247359](/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220817103247359.png)

webservice

soap

Dme-eventrelay-ms

Eventconsumersubs

DTO







## 需要测试的功能

### planned order download



### yield confirmation



# 敏捷开发

## 单元测试



## TDD与BDD



## 重构



## Decoupling & test isolation





# 云开发💡

## HTTP REST

### http协议

**http请求：**

-格式

​		- request line

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAp/images/image-20220728134030677.png" alt="image-20220728134030677" style="zoom:33%;" />

​		- header fields

​		- message body

**http响应：**

-格式：基本上对应

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220728135405839.png" alt="image-20220728135405839" style="zoom: 33%;" />

状态码：

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220728135514455.png" alt="image-20220728135514455" style="zoom:33%;" />

### REST架构风格

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220728140546445.png" alt="image-20220728140546445" style="zoom:50%;" />

REST的URL格式

$https://<host>/<API namespace>/<API version>/<resource path>[?<query>]$

​	REST请求举例：

![image-20220728141314987](/Users/I528479/Desktop/mynotes/实习所学/sap/images/image-20220728141314987.png)



### OData

一种增强版的REST

### 使用springMVC实现REST风格💡





## Logging-日志

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220731135432360.png" alt="image-20220731135432360" style="zoom: 67%;" />



### 结构化参数

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220731142724810.png" alt="image-20220731142724810" style="zoom: 67%;" />



### MDC

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220731161441159.png" alt="image-20220731161441159" style="zoom: 33%;" />



## Persistence - 持久化

数据库事务的四大特性——ACID

### Java persistence API（JPA）



### spring Data JPA





## Authentication认证 & Authorization授权💡

![image-20220804153120831](/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220804153120831.png)

### 认证

**openID：一种标准**

![image-20220804152113130](/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220804152113130.png)

### 授权

**OAuth 2**——颁发有时效性的token

SSO——single sign on，单点登录。







## Cloud Foundry💡

PaaS平台

### CLI

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220803225541525.png" alt="image-20220803225541525" style="zoom:33%;" />

### manifesr

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220803230135531.png" alt="image-20220803230135531" style="zoom: 50%;" />

### Routes

![image-20220803230455759](/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220803230455759.png)

例子：同一个route可以map两个app（蓝绿部署），CF会平衡这新旧两个版本app的流量。



### Continuous  Delivery

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220807104836432.png" alt="image-20220807104836432" style="zoom: 33%;" />

CD/CI 工具：Jenkins、Azure Devops







## Kubernetes/ K8s

**瞬时、可替代**

工具是kubectl。

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220806211433295.png" alt="image-20220806211433295" style="zoom: 33%;" />







# 蓝绿部署

绿——新的

蓝——旧的



# BTP系列知识💡





# 覆盖率测试

## 白盒覆盖率

白盒覆盖率中使用的最常见的就是逻辑覆盖率（Logical Coverage ），也叫代码覆盖率（Code Coverage）或者结构化覆盖率（Structural Coverage），我们常见的逻辑覆盖包括：语句覆盖、判定覆盖、条件覆盖、判定条件覆盖、条件组合覆盖、路径覆盖。

### 语句覆盖

100%语句覆盖率含义：在测试时，首先设计若干个测试用例，然后运行被测程序，使程序中的每个可执行语句至少执行一次。

### 判定覆盖/分支覆盖

100%条件覆盖率含义：在测试时，首先设计若干个测试用例，然后运行测试程序，使得程序中每个判断的取真分支和取假分支至少经历一次，即判断的真假值均曾被满足。

### 条件覆盖

100%条件覆盖率含义：在测试时，首先设计若干个测试用例，然后运行被测试程序，要使每个判断中每个条件的可能取值至少满足一次。



## 黑盒覆盖率

比较少，主要是功能覆盖率（Function Coverage），其中最常见的是需求覆盖。

## 灰盒覆盖率

**函数覆盖和接口覆盖**

函数覆盖=（至少被执行一次的函数数量）/（系统中函数的总数）

接口覆盖=（至少被执行一次的接口数量）/（系统中接口的总数）



