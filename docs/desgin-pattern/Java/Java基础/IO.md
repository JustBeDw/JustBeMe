## File

#### 创建

`file();` : 得到路径

`file.CreatNewFile();` : 不能重名，路径

`file.mkdirs();` ： 多层 

`file.mkdir();` ： 单层

#### 删除

`file.delete();` 删除的文件夹下面不能有文件

#### 重命名

`file.renameTo(new File("")); `在同一个文件夹下就是改名，不同文件夹下就是剪贴

#### 判断

`boolean isDirectory();`是否是目录

`boolean isFile();`是否是文件

`boolean exists();`是否存在

`boolean isHidden();`是否隐藏

`boolean canRead();`是否可读

`boolean canWrite();`是否可写

#### 获取

`String getAbsolutePath();`获取绝对路径

`String getPath();`获取相对路径

`String [] list = file.list();`以字符串形式获取文件下的所有文件名称 

例：*打印当前目录下所有 .txt 结尾的文件*

```java
public class Demo{
    public static void main(String[]args){
        File file = new File("你的绝对路径或相对路径文件夹");
        String [] txtLists = file.list();
        for (String list : txtLists){
            if (list.endsWith(".txt")){
                System.out.print(list);
            }
        }
    }
}
```

`File[] list = file.listFiles();`以文件形式获取文件

```java
public class Demo{
    public static void main(String[]args){
        File file = new File("你的绝对路径或相对路径文件夹");
        File [] txtLists = file.list();
        for (File list : txtLists){
            if (list.endsWith(".txt")){
                System.out.print(list);
            }
        }
    }
}
//获取文件类型，可以对其进行操作
```

## IO

**流向**（相对内存而言且都是单向的）：  
		输入流（把数据输入到程序中，Input、Read）  
		输出流（把数据从程序中输出去，Output、Write）  
**传输的内容**：  
		字节（输入InputStream输出OutputStream）：图片、音频、视频  
		字符（输入Reader输出Writer）：文本内容

#### **Class OutputStream**  

java.lang.Object  
		java.io.OutputStream  
*All Implemented Interfaces*:  
Closeable, Flushable, AutoCloseable  
*Direct Known Subclasses*://**子类**  
***ByteArrayOutputStream, FileOutputStream, FilterOutputStream, ObjectOutputStream, OutputStream, PipedOutputStream***

##### 方法

| Modifier and Type | Method and Description                                       |
| ----------------- | ------------------------------------------------------------ |
| void              | close()<br/>Closes this output stream and releases any system resources associated with this stream. 关闭输出流并释放系统资源 |
| void              | flush()<br/>Flushes this output stream and forces any buffered output bytes to be written out. 刷新此输出流并强制写出任何缓冲输出字节 |
| void              | write(byte[] b)<br/>Writes b.length bytes from the specified byte array to this output stream. 写入b长度的字节从指定的字节数组到这个输出流 |
| void              | write(byte[] b, int off, int len)<br/>Writes len bytes from the specified byte array starting at offset off to this output stream. |
| abstract void     | write(int b)<br/>Writes the specified byte to this output stream. 将指定的字节写入此输出流。 |

##### FileOutputStream

FileOutputStream 继承自 OutputStream

| Constructor and Description                                  |
| ------------------------------------------------------------ |
| ***FileOutputStream(File file)***<br/>Creates a file output stream to write to the file represented by the specified File object.创建一个文件输出流，以便写入由指定文件对象表示的文件。 |
| FileOutputStream(File file, boolean append)<br/>Creates a file output stream to write to the file represented by the specified File object. |
| FileOutputStream(FileDescriptor fdObj)<br/>Creates a file output stream to write to the specified file descriptor, which represents an existing connection to an actual file in the file system. |
| ***FileOutputStream(String name)***<br/>Creates a file output stream to write to the file with the specified name.创建一个文件输出流，以便写入具有指定名称的文件。 |
| FileOutputStream(String name, boolean append)<br/>Creates a file output stream to write to the file with the specified name. |

***FileOutputStream(String name)***   
例：

```java
public class Demo{
    public static void main(String[]args) throws Exception {
        //如果demo.txt 不存在，则创建一个demo.txt 存在的话直接写入文件输出流
        FileOutputStream fos = new FileOutputStream("demo.txt");
        String data = "HiHiHiHi";
        
        //data转成byte 存在数组里
        byte[] bytes = data.getBytes();
        
        //写入文件里
        fos.write(bytes);
        
        //关闭流；释放资源
        fos.close();
    }
}
```

#### Class InputStream

java.lang.Object  
		java.io.InputStream  
*All Implemented Interfaces*:  
Closeable, AutoCloseable  
*Direct Known Subclasses*:  
***AudioInputStream, ByteArrayInputStream, FileInputStream, FilterInputStream, InputStream, ObjectInputStream, PipedInputStream, SequenceInputStream, StringBufferInputStream***

#### 缓冲流

对读写的数据提供了缓冲的功能，提高了读写的效率。

直接上例子：

```txt
-----------a.txt-----------
啊啊啊啊
哦哦哦哦
呵呵呵呵
哈哈哈哈
```

```java
public class Demo{
    public static void main(String[]args) throws Exception {
        //缓冲流不具备读取的功能，能读取是字符流和字节流(FileReader)
        BufferedReader br = new BufferedReader(new FileReader("a.txt"));
        String l;
        while( (l = br.readLine()) != null ){
            System.out.print(l);
        }
        br.close();
    }
}
------------------------------------------------------------------------------
输出：
    啊啊啊啊
    哦哦哦哦
    呵呵呵呵
    哈哈哈哈
```

#### 数据流

数据输入输出流的构造器都只有一个`DataOutputStream(OutputStream out);`、`DataInputStream(InputStream In);`     

```java
  public class Demo{
    public static void main(String[]args) throws Exception {
        //数据输出流
        DataOutputStream dos = new DataOutputStream(new FileOutputStream("aa.txt"));
  		
        //可以写8种基本类型数据
        dos.writeInt(222);
        dos.writeUTF("hI");
        //...
        
        //数据输入流
        DataInputStream dis = new DataInputStream(new FileInputStrea("aa.txt"));
        
        //读取数据
        int d = dis.readInt();
        String d1 = dis.readUTF();
        //...
        
  	  	dos.close();
        dis.close();
    }
}
```
应用在**网络通信**中比较多。

#### 转换流

原本一个字节一个字节的输入输出，转换后，两个字节两个字节的输入输出，将字节流转换成字符流。  
按马士兵老师说的：就是在字节流上套接了一个字符流。  
原本是一个字节读取，先一个字符读取再转成二个字节读取。

```java
public class Demo{
    public static void main(String[]args){
        // true是在原文后拼接，不写true则默认覆盖原文
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("a.txt",true,"utf-8"));
        osw.write("xxxxxx");
        osw.flush();
        osw.close();
    }
}
```

#### 操作流

字符串操作流：

```java
  public class Demo{
    public static void main(String[]args) throws Exception {
        StringWriter sw = new StringWriter();
        
        String s = sw.write("Hi");
        
        StringReader sr = new StringReader(s);
        int n;
        while (n = sr.read()) != -1){
            System.out.print((char)n)
        }
                
        sw.flush();
        sw.close();
        sr.close();
        
    }
}
```

#### 打印流

```java
  public class Demo{
    public static void main(String[]args) throws Exception {
        PrintWriter pw = new PrintWriter("a.txt");
        
        pw.write("aa");
                
        pw.flush();
        pw.close();
        //具体看API即可，不展开了
    }
}
```

#### Bio Nio Aio

bio:阻塞同步  
nio:非阻塞同步  
aio:非阻塞异步

阻塞就是一直等着，同步指的是上一个方法不执行完就不执行下一个方法。