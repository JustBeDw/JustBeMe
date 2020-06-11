## 运行时数据

Java虚拟机运行过程中把所管理的内存划分成若干个数据区，以下是，不同作用的数据区。

#### 堆

堆，内存最大的一个区域，这里是GC管理的主要的区域，此区域主要存放实例。细分的话，分实例区和句柄区，再细分就是新生代和老年代，再再再细分就是Eden、From Survivor、To Survivor。**线程共享区域**

#### 方法区

存放静态变量，常量和类信息。**线程共享区域**

#### 常量池

在方法区内部，存放编译期生成的字面量和符号引用。

#### 虚拟机栈

就是经常喊的栈，每个方法在执行的时候，创建一个栈帧存放局部变量表、操作数栈、动态链接，方法出口等。局部变量表存放了**编译期可知的**：`基本类型`和`对象引用`。

#### 本地方法栈

区别于 Java 虚拟机栈的是，Java 虚拟机栈为虚拟机执行 Java 方法(也就是字节码)服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。

#### 程序计数器

当前线程所执行的字节码行号地址。字节码解释器工作时就是通过改变计数器的值来选取下一条执行的字节码指令。