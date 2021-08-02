# Java泛型

## 定义与背景

> ==Java 泛型（generics）==是 JDK 5 中引入的一个新特性, 泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。

- 泛型的本质是**参数化类型（Parameterized type，把元素的类型设计成一个参数，这个类型参数叫做泛型。）**，也就是说所操作的数据类型**（不能是基本数据类型，只能是类）**被指定为一个参数。

- 所谓泛型，就是允许在定义类、接口时**通过一个标识**表示类中某个属性的类型或者是某个方法的返回值及参数类型。

- 这个类型参数将在使用时（例如，继承或实现这个接口，用这个类型声明变量、创建对象时）被确定（即传入实际的类型参数，也称为类型实参）。

- 为什么要有泛型的演示：

```java
@Test
public void test1(){
    List list = new ArrayList();
    //需求：存放学生数据
    list.add(23);
    list.add(93);
    list.add(54);
    list.add(88);
    //问题一：类型不安全
    list.add("something that is not an Integer");

    for (Object score: list){
        //问题二：强转时，容易出现ClassCastException类型转换异常
        int stuScore = (int) score;
        System.out.println(stuScore);
    }
}
```

## 泛型的使用

### 在集合中使用泛型

