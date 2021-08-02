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

- 以ArrayList为例

```java
@Test
public void test2(){
    ArrayList<Integer> list = new ArrayList<>();//定义的时候指明Integer类型

    list.add(12
    // list.add(new Date());编译报错
    // list.add("something");编译报错

    for(Integer score:list){
        int stuScore = score;
        System.out.println(stuScore);
    }
}
```

在方法、属性、构造器中，可以使用类或接口的泛型。

- 以HashMap为例

```java
@Test
public void test3(){
    HashMap<String, Integer> map = new HashMap();//可以省略后面的泛型，但是<>不能省略

    map.put("张三",100);
    map.put("jack",88);
    //map.put(2,4);//报错
    map.put("rick",88);
    map.put("morty",88);

    System.out.println(map);
    System.out.println("***************");
    //遍历
//    Set<String> strings = map.keySet();
//    Iterator<String> iterator = strings.iterator();
//    while(iterator.hasNext()){
//        String key = iterator.next();
//        Integer value = map.get(key);
//        System.out.println(key + "=====" + value);
//    }
    //<Map.Entry<String, Integer>>为泛型的嵌套
    Set<Map.Entry<String, Integer>> entry = map.entrySet();
    Iterator<Map.Entry<String, Integer>> iterator = entry.iterator();
    while(iterator.hasNext()){
        Map.Entry<String, Integer> e = iterator.next();
        String key = e.getKey();
        Integer value = e.getValue();
        System.out.println(key+"-==========="+ value);
    }
}
```

- 总结

1. 集合接口或集合类在jdk5时，修改为带泛型的结构
2. 在实例化集合类时，可以指明具体的泛型类型
3. 指明完以后，内部结构（方法、属性、构造器等）使用到类的泛型的位置，都指定为实例化时的泛型类型

例子：`add(E e)`----------->实例化后：`add(Integer e)`

4. 再次强调：泛型类型必须是个类，不能是基本数据类型。
5. 实例化时，没有使用泛型的话，默认为Object类型

6. 类型推断（jdk7新特性）

```java
HashMap<String, Integer> map = new HashMap<String, Integer>();
HashMap<String, Integer> map = new HashMap<>();//可以省略后面的泛型，但是<>不能省略
```

## 自定义泛型结构

### 泛型类



