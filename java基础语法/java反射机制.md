# 反射机制

## 概述

静态语言和动态语言

![image-20210914183019860](images/image-20210914183019860.png)





java反射机制

![image-20210914183614111](images/image-20210914183614111.png)



java反射机制提供的功能

- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时判断任意一个类所具有的成员变量和方法
- 在运行时获取泛型信息
- 在运行时调用任意一个对象的成员变量和方法
- 在运行时处理注解
- 生成动态代理





## Class类

![image-20210914195920739](images/image-20210914195920739.png)

Class类的实例就对应着一个**运行时类（除了类，其他结构也行）**。

### 获取Class实例

```java
/*文件名：ReflectionTest.java*/
//方式一：调用 运行时类 的属性
Class clazz1 = Person.class;

//方式二：通过运行时类的对象，调用getClass()
Person p1 = new Person();
Class clazz2 = p1.getClass();

//方式三：调用Class的静态方法：forName(String ClassPath)
Class clazz3 = Class.forName("com.atguigu.java.Person");

System.out.println(clazz1 == clazz2);//true
System.out.println(clazz1 == clazz3);//true,运行时类Person是内存中同一个运行时类

//方式四：使用类的加载器:ClassLoader（了解）
ClassLoader classLoader = ReflectionTest.class.getClassLoader();
Class clazz4 = classLoader.loadClass("com.atguigu.java.Person");
```

![image-20210914200103574](images/image-20210914200103574.png)



## 类的加载于ClassLoader的理解

![image-20210914201025967](images/image-20210914201025967.png)

![image-20210914202455223](images/image-20210914202455223.png)

![image-20210914202503340](images/image-20210914202503340.png)



- 使用ClassLoader加载配置文件

```java
@Test
    public void test1() throws IOException {
        Properties pros = new Properties();
        //此时的文件默认在当前module下
        //读取配置文件的方式一
//        FileInputStream fis = new FileInputStream("jdbc.properties");
//        pros.load(fis);

        //读取配置文件的方式二：使用ClassLoader
        //此时配置文件默认识别为，当前module的src下
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        InputStream is = classLoader.getResourceAsStream("jdbc1.properties");
        pros.load(is);
        
        System.out.println(pros.getProperty("user")+"  "+pros.getProperty("password"));
    }
```



## 创建运行时类的对象

运行时类的对象，相当于加载到内存中的Person类。

![image-20210914211048135](images/image-20210914211048135.png)

