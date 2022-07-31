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

<img src="/Users/I528479/Desktop/mynotes/实习所学/Untitled/images/image-20220728134030677.png" alt="image-20220728134030677" style="zoom:33%;" />

​		- header fields

​		- message body

**http响应：**

-格式：基本上对应

<img src="/Users/I528479/Desktop/mynotes/实习所学/Untitled/images/image-20220728135405839.png" alt="image-20220728135405839" style="zoom: 33%;" />

状态码：

<img src="/Users/I528479/Desktop/mynotes/实习所学/Untitled/images/image-20220728135514455.png" alt="image-20220728135514455" style="zoom:33%;" />

### REST架构风格

<img src="/Users/I528479/Desktop/mynotes/实习所学/Untitled/images/image-20220728140546445.png" alt="image-20220728140546445" style="zoom:50%;" />

REST的URL格式

$https://<host>/<API namespace>/<API version>/<resource path>[?<query>]$

​	REST请求举例：

![image-20220728141314987](/Users/I528479/Desktop/mynotes/实习所学/Untitled/images/image-20220728141314987.png)



### OData

一种增强版的REST

### 使用springMVC实现REST风格💡





## Logging

<img src="/Users/I528479/Desktop/mynotes/实习所学/SAP/images/image-20220731135432360.png" alt="image-20220731135432360" style="zoom: 67%;" />











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



