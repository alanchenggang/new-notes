# JVM	
## 运行时数据区
1. 程序计数器 PC 指向下一条指令的地址  线程私有 没有OOM异常
2. 虚拟机栈  基本单位--> 栈帧 一个线程对应一个栈帧  会发生OOM
3. 本地方法栈
4. 堆 
整个Java应用程序共享的区域.职责就是存储和管理对象和数组 GC机制主要发生作用的内存区域
5. 方法区
整个Java应用程序共享的区域.存储所有的类信息/常量/静态变量/动态编译缓存等数据 方法区= 类信息表+运行时常量池. 运行时常量池存放各个类的类常量池信息
---
## Java中的类加载器
Java 中的类加载器大致可以分成两类，一类是系统提供的，另外一类则是由Java 应用开发人员编写的。系统提供的类加载器主要有下面三个：



### 引导类加载器（bootstrap class loader）：

它用来加载 Java 的核心库，是用原生代码来实现的，并不继承自 java.lang.ClassLoader。主要负责jdk_home/lib目录下的核心api 或 -Xbootclasspath 选项指定的jar包装入工作（其中的jdk_home是指配置jdk环境变量是java_home的配置路径，一般是jdk/jre所在目录）。

### 扩展类加载器（extensions class loader）：

它用来加载 Java 的扩展库。Java虚拟机的实现会提供一个扩展库目录，扩展类加载器在此目录里面查找并加载 Java 类，主要负责jdk_home/lib/ext目录下的jar包或 -Djava.ext.dirs 指定目录下的jar包装入工作。

### 系统类加载器（system class loader）：

它根据 Java 应用的类路径（CLASSPATH）来加载 Java 类。一般来说，Java 应用的类都是由它来完成加载的。可以通过 ClassLoader.getSystemClassLoader()来获取它。主要负责CLASSPATH/-Djava.class.path所指的目录下的类与jar包装入工作.

---
## 谈一谈Java中的四大引用
---
1. 强引用:
    - new出来的对象.在代码中最常见
    - 无论何种情况,只要强引用关系还存在,GC就永远不会回收掉被引用的对象
2. 软引用
    - 描述一些还有用,但非必须的对象
    - 内存不够才回收,回收软引用对象后仍然内存不够,才报出内存溢出异常
    - 通过jdk提供的SoftReference类来实现软引用
3. 弱引用
    - 描述那些比软引用更弱一点的对象
    - 只能生存到下一次GC发生为止,不论剩余内存大小都回收
    - 通过jdk提供的WeakReference类来实现软引用
4. 虚引用
    - 为一个对象设置虚引用的唯一目的就是为了能在这个对象被收集器回收时收到一个系统通知
    - 通过jdk提供的PhantomReference类来实现软引用
## JVM中锁的关键字
---

## JVM内存模型
---
<img src="https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/java/image/1628307-20191231224756735-422346379.png">
<center>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">JVM内存模型图解</div>
</center>

> Java 堆内存(Heap)、方法区(Non-Heap)、JVM栈(JVM Stack)、本地方法栈（Native Method Stacks）、程序计数器（Program Counter Register）

### Java 堆内存(Heap)
- 所有线程共享
- 存放对象实例
- 垃圾收集器管理的主要区域(GC堆)
- 会发生OOM异常
### 方法区(Non-Heap)
- 所有线程共享
- 用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据
- 逐步转移到Java Heap 在Java8之后被元空间(Metaspace)所取代
### 程序计数器（Program Counter Register）
- 指向下一条要执行的字节码指令的位置
- 线程私有
### JVM栈(JVM Stack)
- 线程私有,生命周期与线程相同
- 描述的是Java方法执行的内存模型,每个方法执行的同时创建**栈帧**,方法被调用到执行完成对应着栈帧从入栈到出栈的过程
- 如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常 如果虚拟机栈可以动态扩展，当扩展时无法申请到足够的内存时会抛出OutOfMemoryError异常。
#### 栈帧(Stack Frame）
- 局部变量表 局部变量表是一组变量值的存储空间，用于存放方法参数和方法内部定义的局部变量
- 操作数栈 借助栈完成计算等操作
- 动态连接 每个栈帧都包含一个指向运行时常量池中该栈帧所属方法的引用，持有这个引用是为了支持方法调用过程中的动态连接
> class文件的常量池中存有大量的符号引用，字节码中的方法调用指令就以常量池里指向方法的符号引用作为参数，这些符号引用会在类加载阶段或者第一次使用的时候转化为直接引用，这个转化称为静态解析。另外一部分将在每一次运行期间转化为直接引用，这部分就称为动态连接
- 返回地址 (正常调用完成/异常调用完成)
#### 本地方法栈（Native Method Stacks)
- 虚拟机执行使用到的Native方法服务
- 会抛出StackOverflowError和OutOfMemoryError异常
- 与其他语音交互

<img src="https://cscgblog-1301638685.cos.ap-chengdu.myqcloud.com/java/image/1628307-20191231224451199-929179305.png">
<center>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">JVM线程共享区域图解</div>
</center>

## 类的生命周期和类加载
---
## 介绍一下双亲委派机制


## 如何打破双亲委派机制(Tomcat)

---
## 介绍垃圾回收算法
标记清除算法
标记整理算法

---
## 介绍一下各种垃圾回收器,并说明他们有何特点
---
##  介绍一下STW机制

在标记整理算法中,移动存活对象并更新所有引用这些对象的地方将会是一种极为负重的操作,而且这种对象移动操作必须全程暂停用户应用程序才能进行;

根节点枚举必须保证始终还是必须在一个能保障一致性的快照中才能得以运行,否则不能保分析结果的准确性,也会造成用户线程面临STW的困扰

---
## 介绍一下GCROOTS的构成


---

- 在虚拟机栈(栈帧中的本地变量表)中引用的对象(当前方法的参数/局部变量/临时变量等)
- 在方法区中类静态属性引用的对象(Java类的引用类型静态变量)
- 在方法区中常量引用的对象(字符串常量池里的引用)
- 在本地方法栈中JNI引用的对象
- Java虚拟机内部的引用(基本数据类型对应的Class对象/一些常驻异常类/系统类加载器)
- 所有被同步锁(synchronized关键字)持有的对象
- 反映Java虚拟机内部情况的JMXBean/JVMTI中注册的回调/本地代码缓存等

## 介绍下三色标记算法



## 什么是安全点

