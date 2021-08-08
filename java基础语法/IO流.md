# File 类的使用

java.io.File类是**文件**和**文件目录路**径的抽象表示形式，与平台无关。

## 常用构造器

<img src="images/image-20210803220837163.png" alt="image-20210803220837163" style="zoom: 67%;" />

- `public File(String pathname)` 

![image-20210803221855434](images/image-20210803221855434.png)

![image-20210803221911996](images/image-20210803221911996.png)

- `public File(String parent,String child)`

以parent为父路径，child为子路径创建File对象。

- `public File(File parent,String child)`

根据一个**父File对象**和子文件路径创建File对象。

## 常用方法

### 获取功能

![image-20210803224840646](images/image-20210803224840646.png)

适用于文件目录（**要求文件目录存在**）的方法：

![image-20210803224910166](images/image-20210803224910166.png)

### 重命名功能

`public boolean renameTo(File dest)`

把文件重命名为指定的**文件路径**，注意，这里需要调用此方法的文件是真实存在的，且dest文件不能存在。

### 判断功能

![image-20210803230836201](images/image-20210803230836201.png)

### 创建功能

- `public boolean createNewFile()` ：创建文件。若文件存在，则不创建，返回false
- `public boolean mkdir()` ：创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的上层目录不存在，也不创建。 
- `public boolean mkdirs()` ：创建文件目录。**如果上层文件目录不存在，一并创建**。

**注意事项：如果你创建文件或者文件目录没有写盘符路径，那么，默认在项目路径下。** 

### 删除功能

![image-20210803232027550](images/image-20210803232027550.png)

### 总结

File类中的方法并未涉及到文件内容的操作。需要读取或写入文件内容，必须使用IO流来完成。

后续File类的对象常会作为参数传递到流的构造器中，指明读取或写入的终点。

# IO流

![image-20210805102350313](images/image-20210805102350313.png)

![image-20210804133902587](images/image-20210804133902587.png)



# NIO（周末完成）

## 三大组件

![image-20210804140047330](images/image-20210804140047330.png)

## ByteBuffer

### ByteBuffer基本使用

1. 向buffer写入数据（例如，`channel.read(buffer)`）
2. 调用flip()，切换至读偶数
3. 从buffer读取数据（例如，`buffer.get()`）
4. 调用`clear()`或`compact()`切换至写模式
5. 重复1-4步骤

```java
@Test
public void test1(){
    //读取文件需要用到 FileChannel，获得的办法：1. 输入输出流   2.RandomAccessFile(如下)
    //RandomAccessFile file = new RandomAccessFile("data.txt","rw");
    //FileChannel channel1 = file.getChannel();

    try (FileChannel channel = new FileInputStream("data.txt").getChannel()) {
        //准备一个缓冲区
        ByteBuffer buffer = ByteBuffer.allocate(1);
        while(true) {
            //从channel中读取数据, 向 buffer 写入
            int i = channel.read(buffer); //read操作后返回一个整型，表示读到的字节数
            if(i == -1){
                break;//读取后返回结果为-1表示没有内容了
            }

            //测试看看数据有没有读取到：打印 buffer 中的内容
            buffer.flip();//切换Buffer到： 读模式
            String string = new String();
            while (buffer.hasRemaining()) { //检查是否还有剩余未读的数据
                byte b = buffer.get();
                //                System.out.println( b);//打印出对应的ASCII码
                System.out.println((char) b);//将每个字节，强转成字符打印

            }
            buffer.clear(); //buffer 切换为写模式
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```



### byteBuffer常用方法

![image-20210806170349231](images/image-20210806170349231.png)

#### 分配空间

- allocate()------->HeapByteBuffer，java堆内存；读写效率较低；收到垃圾回收的影响

- allocateDirect()------->DirectByteBuffer，直接内存；读写效率高（少一次拷贝）；使用系统内存，不会受到垃圾回收的影响；分配的效率低


#### 向buffer写入数据

1. 调用`channel.read(buffer)`：从channel中读取数据, 向 buffer 写入
2. 调用buffer.put()：向buffer写入

#### 从buffer读取数据

1. 调用channel.write(buffer)方法
2. 调用buffer.get()方法

注意：**get()会让position指针向后走**，如果想要重复读取数据：

- 调用`rewind()`将position重置为0
- 或者调用`get(int index)`读取特定索引的内容，**不会移动指针**

```java
/*ByteBuffer 常用方法：从buffer读取数据*/
@Test
public void byteBufferReadTest(){
    ByteBuffer buffer = ByteBuffer.allocate(10);
    buffer.put(new byte[]{'a','b','c','d'});
    buffer.flip();
    // 1. rewind() 从头开始读取
    /* byte[] bytes = new byte[4];
        buffer.get(bytes);//将数据读取到bytes数组中
        System.out.println((char)bytes[0]);//a

        buffer.rewind();//调用完rewind()后，position归零，可以从头开始读
        System.out.println((char) buffer.get());//a*/

    /*//2. mark & reset
        // mark 做一个标记，记录position 位置
        // reset 将position 位置 重置到 mark 的位置
        System.out.println((char)buffer.get());//a
        System.out.println((char)buffer.get());//b，第二次position位置改变了
        buffer.mark();//将标记加到索引为 2 的位置
        System.out.println((char)buffer.get());//c
        System.out.println((char)buffer.get());//d
        buffer.reset();//将position 重置到 mark位置
        //reset之后，还可以第二次读到c/d
        System.out.println((char)buffer.get());//c
        System.out.println((char)buffer.get());//d*/

    //3. get(int index)
    System.out.println((char) buffer.get(3));//直接拿到d
    System.out.println(buffer);//java.nio.HeapByteBuffer[pos=0 lim=4 cap=10],位置没有改变
}
```

#### 字符串与ByteBuffer之间的相互转换

```java
@Test
public void byteBufferStringTest(){
    /*//1. 字符串转为 ByteBuffer : 使用字符串的getBytes得到字节，然后将字节put到buffer中
        ByteBuffer buffer = ByteBuffer.allocate(19);
        buffer.put("hello".getBytes());
        buffer.flip();
        System.out.println((char)buffer.get(1));//e*/

    /*//2. Charset
        ByteBuffer buffer1 = StandardCharsets.UTF_8.encode("hello");
        //使用Charset的encode()方法，buffer自动切换到读模式：
        System.out.println(buffer1.position());//0
        System.out.println(buffer1.limit());//5*/

    //3. wrap(): nio提供的工具方法，将一个字节数组包装成一个buffer
    ByteBuffer buffer = ByteBuffer.wrap("hello".getBytes());

    //4. 转为字符串：同理，ByteBuffer转化为字符串也有很多办法
    java.lang.String string = StandardCharsets.UTF_8.decode(buffer).toString();
    System.out.println(string);//hel lo

}
```

### 分散读、集中写的思想

```java
public class TestScatteringRead {
    /*分散读取 即：一个文件中的不同内容要加载到不同buffer中 */
    public static void main(String[] args) {
        try (FileChannel channel = new RandomAccessFile("words.txt", "r").getChannel()) {
            ByteBuffer buffer1 = ByteBuffer.allocate(3);
            ByteBuffer buffer2 = ByteBuffer.allocate(3);
            ByteBuffer buffer3 = ByteBuffer.allocate(5);
            //channel.read()可以将数据分散写入多个buffer中
            long l = channel.read(new ByteBuffer[]{buffer1, buffer2, buffer3});
            System.out.println(l);//11

            buffer1.flip();
            buffer2.flip();
            buffer3.flip();

            System.out.println((char) buffer1.get(2));//e


        } catch (IOException e) {


        }
    }

    /*集中写入 即：把多个buffer中的内容写入到一个文件中*/
    @Test
    public void testGatheringWrite(){
        ByteBuffer buffer1 = StandardCharsets.UTF_8.encode("hello");//5个字节
        ByteBuffer buffer2 = StandardCharsets.UTF_8.encode("world");//5个字节
        ByteBuffer buffer3 = StandardCharsets.UTF_8.encode("你好");//6个字节

        try (FileChannel channel = new RandomAccessFile("words2.txt", "rw").getChannel()) {
            channel.write(new ByteBuffer[]{buffer1,buffer2,buffer3});
            //成功产生 words2.txt,内容为：helloworld你好

        } catch (IOException e) {
        }
    }
}
```

### 黏包半包分析

网络上的数据进行传输的时候，会有黏包和半包

例子：

多条数据要发送给服务器，数据之间使用\n 进行分隔，比如:

> Hello, world\n
>
> I'm zhangsan\n
>
> How are you?\n

变成了下面的两个byteBuffer

> Hello, world\nI'm zhangsan\nHo
>
> w are you?\n

编写程序，将错乱的数据恢复：

```java
public class TestByteBufferExam {
    public static void main(String[] args) {
        ByteBuffer source = ByteBuffer.allocate(32);
        source.put("Hello, world\nI'm zhangsan\nHo".getBytes());
        spilt(source);
        source.put("w are you?\n".getBytes());
        spilt(source);

    }
    private static void spilt(ByteBuffer source){
        source.flip();//改为读模式
        //遍历source ,寻找 \n
        for (int i = 0; i < source.limit(); i++) {

            if(source.get(i) == '\n'){//找到一条完整消息后:
                //1. 计算该消息的长度
                int length = i + 1 - source.position();
                ByteBuffer target = ByteBuffer.allocate(length);
                //2. 从 source 读，向 target 写
                for (int j = 0; j < length; j++) {
                    target.put(source.get());
                }
                //3. 每拆出一条完整消息，打印一下
                target.flip();
                System.out.print(StandardCharsets.UTF_8.decode(target).toString());
            }
        }
        //如果没有找到\n ,切换到写模式,出方法体
        source.compact();
    }
}
```

## 文件编程

### FileChannel

> 注意，FileChannel只能工作在阻塞模式下

#### 获取

通过getChannel()方法获取

- FileInputStream获取的channel只能读；
- FileOutputStream获取的channel只能写；
- RandomAccessFile可以通过构造器的mode（r/w/rw）参数设置读写模式；

#### 读取

`channel.read()`方法，返回值表示读到了多少字节，-1表示到达了文件的末尾

```java
int readBytes = channel.read(buffer);
```

#### 写入

channel的写入能力是有上限的，**特别是SocketChannel**，因此一般这么写入数据：

```java
ByteBuffer buffer = ...
buffer.put(...);  //存入数据 
buffer.flip();    //切换读模式

while(buffer.hasRemaining()){
    channel.write(buffer);  //从buffer中向channel中写入数据
}
```

在while中调用channel.wirte是因为write不能保证一次性将buffer中的内容全部写入channel

#### 关闭

channel也是一种资源，可以使用“`try with resource`”的语法关闭资源

```java
 try (FileChannel channel = new RandomAccessFile("words2.txt", "rw").getChannel()) {
     channel.write(new ByteBuffer[]{buffer1,buffer2,buffer3});
     //成功产生 words2.txt,内容为：helloworld你好
 } catch (IOException e) {
 }
```

使用前述各种FileInputStream等的close()也会间接调用channel的close()

#### 位置

获取当前位置

```java
long pos = channel.position();
```

设置当前位置

```java
long newPos= ..
channel.position(newPos);
```

#### 大小

使用size()获取文件的大小

### 两个filechannel间传输数据

transferTo()方法

```java
public class TestFileChannelTransferTo {
    public static void main(String[] args) {
        try (
                FileChannel from = new FileInputStream("data.txt").getChannel();
                FileChannel to  = new FileOutputStream("to.txt").getChannel();
        ) {
            //创建出新的to.txt文件，
            //效率高，底层使用零拷贝进行优化
            from.transferTo(0,from.size(),to);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

传输的最大大小为2g。改进的方法：多次传输。

```java
public class TestFileChannelTransferTo {
    public static void main(String[] args) {
        try (
                FileChannel from = new FileInputStream("data.txt").getChannel();
                FileChannel to  = new FileOutputStream("to.txt").getChannel();
        ) {
            //创建出新的to.txt文件，
            //效率高，底层使用零拷贝进行优化
            long size = from.size();
            //left变量表示还剩余多少字节
            for(long left = size; left>0; ) {
                left -= from.transferTo((size-left), left, to);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

