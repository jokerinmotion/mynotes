# java集合框架

集合数组都是对多个数据进行存储操作的结构，简称java容器。

- 数组存储的特点

![image-20210729115534179](images/image-20210729115534179.png)

- 集合的使用场景

![image-20210729132707337](images/image-20210729132707337.png)

- java集合
  - Collection接口：单列数据，定义了存取一组对象的方法的集合
    - List接口：元素有序、可重复的集合。也称为**“动态数组”**。
    - Set接口：元素无序、不可重复的集合。
    - 还有其他接口

  <img src="images/image-20210729133956340.png" alt="image-20210729133956340" style="zoom:50%;" />

  - Map接口：双列数据，保存具有映射关系“key-value对”的集合

<img src="images/image-20210729134007589.png" alt="image-20210729134007589" style="zoom:67%;" />

## Collection接口

![image-20210729152556933](images/image-20210729152556933.png)

### 抽象方法的使用

```java
Collection coll = new ArrayList();
//接口不能调用方法，需要使用实现接口的子类
```

<img src="images/image-20210729153843142.png" alt="image-20210729153843142" style="zoom: 67%;" />

<img src="images/image-20210729153900963.png" alt="image-20210729153900963" style="zoom:67%;" />

注意 

1. 向Collection接口的实现类添加数据obj的时候，要求obj所在类要重写equals()方法

2. 集合转化为数组使用`toArrays()`方法；数组转换为集合使用Array类的静态方法`asList()`方法

例子：

```java
@Test
    public void test2(){
        Collection coll = new ArrayList();
        coll.add(124);
        coll.add(1241415367L);

        //集合——>数组：toArray()方法
        Object[] arr = coll.toArray();
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
        //数组——>集合：Array类的静态方法asList()
        List<String> list = Arrays.asList(new String[]{"bb", "aa", "cc"});
        System.out.println(list);

        //特例
        List<int[]> ints = Arrays.asList(new int[]{1, 2, 4, 5});
        System.out.println(ints);//[[I@ba8a1dc]，一个为一维数组，元素是int类型
        System.out.println(ints.size());//1
    }
```

### Iterator迭代器接口

使用 Iterator 接口遍历集合元素

