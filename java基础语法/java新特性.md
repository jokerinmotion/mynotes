# java 8新特性

## lambda表达式

Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）。

### 格式

```java
(parameters) -> expression
或
(parameters) ->{ statements; }
```

![image-20210915203839111](images/image-20210915203839111.png)



### 使用

![image-20210915204652876](images/image-20210915204652876.png)

![image-20210915204710815](images/image-20210915204710815.png)

注意，**lambda表达式的本质：函数式接口的实现实例**



### 函数式接口

![image-20210915211836663](images/image-20210915211836663.png)

![image-20210915213036062](images/image-20210915213036062.png)

**以上接口及其拓展接口在实例化的时候可以根据需求考虑使用Lambda表达式**



## 方法引用

![image-20210915220236781](images/image-20210915220236781.png)

### 格式

<img src="images/image-20210915221020554.png" alt="image-20210915221020554" style="zoom:80%;" />

#### 情况一

```java
// 情况一：对象 :: 实例方法
//Consumer中的void accept(T t)
//PrintStream中的void println(T t)
@Test
public void test1() {
    Consumer<String> con1 = str -> System.out.println(str);
    con1.accept("something");

    System.out.println("*************************************");
    PrintStream stream = System.out;
    Consumer<String> con2 = stream::println;
    con2.accept("something else");
}

//Supplier中的T get()
//Employee中的String getName()
@Test
public void test2() {
    Employee employee = new Employee(1001,"tom",23,5680);
    Supplier<String> sup1 = () -> employee.getName();
    System.out.println(sup1.get());
    System.out.println("*************************************");
    Supplier<String> sup2 = employee::getName;
    System.out.println(sup2.get());
}
```

#### 情况二

`类::静态方法名`

![image-20210916153213433](images/image-20210916153213433.png)

#### 情况三

`类 :: 非静态方法名`

![image-20210916201254832](images/image-20210916201254832.png)





## 构造器引用



![image-20210916201522463](images/image-20210916201522463.png)

### 特例：数组引用

把数组看成是特殊的类，将`type[]`看成是构造器

![image-20210916203714039](images/image-20210916203714039.png)



## Stream API

### 概述

- Java8中有两大最为重要的改变。第一个是 **Lambda** **表达式**；另外一个则是 **Stream API**。 
- Stream API ( java.util.stream) 把真正的函数式编程风格引入到Java中。这是目前为止对Java类库最好的补充，因为Stream API可以极大提供Java程序员的生产力，让程序员写出高效率、干净、简洁的代码。
- Stream 是 Java8 中处理集合的关键抽象概念，它可以指定你希望对集合进行的操作，可以执行非常复杂的查找、过滤和映射数据等操作。 使用Stream API 对集合数据进行操作，就类似于使用 SQL 执行的数据库查询。也可以使用 Stream API 来并行执行操作。简言之，Stream API 提供了一种高效且易于使用的处理数据的方式。

![image-20210916210448071](images/image-20210916210448071.png)

- Stream的本质：

是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。**“集合讲的是数据，Stream讲的是计算！”**

![image-20210916210651727](images/image-20210916210651727.png)

### 操作步骤

![image-20210916210720289](images/image-20210916210720289.png)

#### 实例化

- 方式一：通过集合

![image-20210916212037681](images/image-20210916212037681.png)



- 方式二：通过数组

![image-20210916212215335](images/image-20210916212215335.png)

​    

- 方式三：Stream的of()方法

![image-20210916212303605](images/image-20210916212303605.png)



- 方式四：创建无限流（了解）

![image-20210916212424968](images/image-20210916212424968.png)



#### 中间操作

多个**中间操作**可以连接起来形成一个**流水线**，除非流水线上触发终止操作，否则**中间操作不会执行任何的处理**！而在**终止操作时一次性全部处理，称为“惰性求值”**。

1. 筛选与切片

![image-20210916213941286](images/image-20210916213941286.png)

2. 映射 

![image-20210916214327778](images/image-20210916214327778.png)

3. 排序

![image-20210916215839304](images/image-20210916215839304.png)



#### 终止操作

![image-20210916220911427](images/image-20210916220911427.png)

1. 匹配与查找

![image-20210916220953784](images/image-20210916220953784.png)

![image-20210916221004434](images/image-20210916221004434.png)

2. 归纳

![image-20210916221024234](images/image-20210916221024234.png)



3. 收集

![image-20210916221057835](images/image-20210916221057835.png)

![image-20210916221114066](images/image-20210916221114066.png)

![image-20210916221126247](images/image-20210916221126247.png)



## Optional类

`Optional<T>` 类(java.util.Optional) 是一个**容器类**，它可以保存类型T的值，代表这个值存在。或者仅仅保存null，表示这个值不存在。原来用 null 表示一个值不存在，现在 Optional 可以更好的表达这个概念。**并且可以避免空指针异常**。

![image-20210917101820287](images/image-20210917101820287.png)



# java 9/10/11新特性

## 模块化系统

![image-20210917204202485](images/image-20210917204202485.png)

![image-20210917204218684](images/image-20210917204218684.png)

![image-20210917204227341](images/image-20210917204227341.png)



## REPL工具：jShell命令

![image-20210917205031587](images/image-20210917205031587.png)



## 接口的私有方法

- Java 8中规定接口中的方法除了抽象方法之外，还可以定义静态方法和默认的方法。一定程度上，扩展了接口的功能，此时的接口更像是一个抽象类。 
- 在Java 9中，接口更加的灵活和强大，连**方法的访问权限修饰符都可以声明为private的**了，此时方法将不会成为你对外暴露的API的一部分。



## 钻石操作符（<>泛型符号）

在 java 9 中， `<>`可以与匿名的内部类一起使用，从而提高代码的可读性。

![image-20210917213820193](images/image-20210917213820193.png)



## 改进try语句

![image-20210917214637858](images/image-20210917214637858.png)

![image-20210917214716779](images/image-20210917214716779.png)

## String存储结构变更

String 再也不用 char[] 来存储啦，改成了 byte[] 加上编码标记，节约了一些空间。



## 集合工厂方法

略
