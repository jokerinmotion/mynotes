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

![image-20210804140047330](images/image-20210804140047330.png)

## ByteBuffer基本使用

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



## byteBuffer常用方法

![image-20210806170349231](images/image-20210806170349231.png)

### 分配空间

allocate()------->HeapByteBuffer，java堆内存；读写效率较低；收到垃圾回收的影响

allocateDirect()------->DirectByteBuffer，直接内存；读写效率高（少一次拷贝）；使用系统内存，不会受到垃圾回收的影响；分配的效率低

### 向buffer写入数据

1. 调用`channel.read(buffer)`：从channel中读取数据, 向 buffer 写入
2. 调用buffer.put()：向buffer写入

### 从buffer读取数据

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

### 字符串与ByteBuffer之间的相互转换
