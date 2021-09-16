# java 8新特性

## 方法引用

![image-20210915220236781](images/image-20210915220236781.png)

### 格式

<img src="images/image-20210915221020554.png" alt="image-20210915221020554" style="zoom:80%;" />

举例

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

