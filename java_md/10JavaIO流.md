# Java I/O流

> 一个流被定义为一个数据序列。输入流用于从源读取数据，输出流用于向目标写数据。
>
> 字符流和字节流

![iostream2xx](3EDA835DECAA40EEA0BB1E18ED40D4F8)

## FileInputStream

该流用于从文件读取数据，它的对象可以用关键字 new 来创建。

有多种构造方法可用来创建对象。

可以使用字符串类型的文件名来创建一个输入流对象来读取文件：

`InputStream f = new FileInputStream("C:/java/hello");`

也可以使用一个文件对象来创建一个输入流对象来读取文件。我们首先得使用 File() 方法来创建一个文件对象：

`File f = new File("C:/java/hello"); InputStream out = new FileInputStream(f);`

创建了`InputStream`对象，就可以使用下面的方法来读取流或者进行其他的流操作。

| **序号** | **方法及描述**                                               |
| :------- | :----------------------------------------------------------- |
| 1        | **`public void close() throws IOException{}`** 关闭此文件输入流并释放与此流有关的所有系统资源。抛出`IOException`异常。 |
| 2        | **`protected void finalize()throws IOException {}`** 这个方法清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出`IOException`异常。 |
| 3        | **`public int read(int r)throws IOException{}`** 这个方法从` InputStream `对象读取指定字节的数据。返回为整数值。返回下一字节数据，如果已经到结尾则返回-1。 |
| 4        | **`public int read(byte[] r) throws IOException{}`** 这个方法从输入流读取`r.length`长度的字节。返回读取的字节数。如果是文件结尾则返回-1。 |
| 5        | **`public int available() throws IOException{}`** 返回下一次对此输入流调用的方法可以不受阻塞地从此输入流读取的字节数。返回一个整数值。 |

## FileOutputStream

该类用来创建一个文件并向文件中写数据。

如果该流在打开文件进行输出前，目标文件不存在，那么该流会创建该文件。

有两个构造方法可以用来创建 `FileOutputStream` 对象。

使用字符串类型的文件名来创建一个输出流对象：

`OutputStream f = new FileOutputStream("C:/java/hello")`

也可以使用一个文件对象来创建一个输出流来写文件。我们首先得使用File()方法来创建一个文件对象：

`File f = new File("C:/java/hello"); OutputStream f = new FileOutputStream(f);`

创建`OutputStream` 对象完成后，就可以使用下面的方法来写入流或者进行其他的流操作。

| **序号** | **方法及描述**                                               |
| :------- | :----------------------------------------------------------- |
| 1        | **`public void close() throws IOException{}`** 关闭此文件输入流并释放与此流有关的所有系统资源。抛出`IOException`异常。 |
| 2        | **`protected void finalize()throws IOException {}`** 这个方法清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出`IOException`异常。 |
| 3        | **`public void write(int w)throws IOException{}`** 这个方法把指定的字节写到输出流中。 |
| 4        | **`public void write(byte[] w)`** 把指定数组中`w.length`长度的字节写到`OutputStream`中。 |