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
