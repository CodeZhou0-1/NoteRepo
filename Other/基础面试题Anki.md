# 基础面试题Anki



### 关于赋值和自增自减

- ![image-20210403083947267](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403083947267.png)
- ![image-20210403090747937](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403090747937.png)



### 请创建双重校验单例模式

- ![image-20210403094032124](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403094032124.png)
- ![image-20210403094232644](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403094232644.png)
- ![image-20210403094157780](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403094157780.png)



###  子类初始化的执行顺序

- ![image-20210403095533764](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403095533764.png)
- ![image-20210403100636859](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403100636859.png)
- ![image-20210403100739951](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403100739951.png)
- ![image-20210403100824932](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403100824932.png)



### 实参和形参的值传递

- ![image-20210403101012746](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403101012746.png)
- ![image-20210403103425706](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403103425706.png)

- ![image-20210403103509481](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403103509481.png)



### 成员变量，局部变量，静态变量的区别

- ![image-20210403105216572](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403105216572.png)
- ![image-20210403110417600](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403110417600.png)



###  Spring事务的传播属性

![image-20210403111818930](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403111818930.png)





### Null，Empty，Blank的区别

- `Null`指**对象没有初始化，也就是说没有引用指向这个对象**
- `isEmpty()` :此方法可以使用于字符串，数组，集合
  - public boolean isEmpty() {
           return value.length == 0; 
    }
  - 根据对象的长度进行判断，使用这个方法时，先要排除对象不为null，当对象为null时，调用isEmpty方法就会报空指针异常。
  - 据源码可知要想要此方法返回true，也就是一个当对象的长度为0，如字符串，数组等，就会返回true。
  - **需要注意的是当字符串对象内容为空格的时候，字符串的长度是不为0的，所以当判断一些字符串需要有数据的时候空格应该也要被否认，下面有一种方法（isNotBlank）就是用来处理这种情形的。**

- `isNotBlank` : 判断字符串内容不为空串且不为空白串
  - ![image-20210403112622669](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403112622669.png)



### int和Integer之间的比较

```java
   public static void main(String[] args) {
        int i1 = 127;
        Integer i2 = new Integer(127);
        Integer i3 = 127;
        Integer i4 = 127;
        Integer i5 = new Integer(500);
        Integer i6 = new Integer(500);
        Integer i7 = 500;
        Integer i8 = 500;
        System.out.println(i1 == i2);//true  当与i1基本类型比较的时候i2自动拆箱为基本类型，相当于基本类型之间的比较
        System.out.println(i1 == i3);//true  同上
        System.out.println(i2 == i3);//false 两个不同对象之间的比较
        System.out.println(i3 == i4);//true  i3和i4范围在[-128~128)之间的时候且以直接赋值方式生成对象，那么指向同一个已经缓存好的Integer对象
        System.out.println(i5 == i6);//false 两个不同对象之间的比较
        System.out.println(i7 == i8);//false 范围不在[-128~128)之间，两个不同对象直接的比
    }
```

- ![image-20210403135624693](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403135624693.png)



### 四大代码块的区别

- ![image-20210403141235305](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403141235305.png)
- ![image-20210403141240733](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403141240733.png)
- ![image-20210403141244613](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403141244613.png)
- ![image-20210403141302221](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403141302221.png)



### 前后端传值的三个注解

- ![image-20210403142023657](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403142023657.png)
- ![image-20210403142126709](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403142126709.png)



### @Bean和@Component的联系和区别

> - 最终效果都是为spring容器注册Bean.
> - 注册Bean之后都可以使用@Autowired或者@Resource注解注入
> - @Component是**类级别的注释，而@Bean是方法级别的注释**.
> - @Bean通常和@Configuration注解搭配使用
> - @Bean是**方法级别的注释**，该方法的名称用作Bean名称
> - @Bean**显式声明**单个Bean，而不是让Spring自动执行。
> - **如果想将第三方的类变成组件，又没有源代码，也就没办法使用`@Component`进行自动配置，这种时候使用`@Bean`就比较合适了。**
> - @Component注解一般用于自定义类上将其注入spring容器中进行管理。



### Java序列化和序列化UID的作用

> **为了保存在内存中的各种对象的状态（也就是实例变量，不是方法），并且可以把保存的对象状态再读出来，这是java中的提供的保存对象状态的机制—序列化。** **将对象的状态信息转换为可以存储或传输的形式的过程**。



> ###   serialVersionUID如何产生以及为什么在类中加上这个变量
>
> - **当我们一个实体类中没有显示的定义一个名为“serialVersionUID”、类型为long的变量时，Java序列化机制会根据编译时的class自动生成一个serialVersionUID作为序列化版本比较**，这种情况下，只有同一次编译生成的class才会生成相同的serialVersionUID。
> - 譬如，当我们编写一个类时，随着时间的推移，我们因为需求改动，需要在本地类中添加其他的字段，这个时候再反序列化时便会出现serialVersionUID不一致，导致反序列化失败。那么如何解决呢？
> - 解决办法为**在本地类中添加一个“serialVersionUID”变量，值保持不变，便可以进行序列化和反序列化。**



### `==`、`equal()` 和 `hashCode()`的联系和区别

> - `==`：判断的是两个对象是否是同一个对象，即实际比较他们的内存地址是否指向同一个对象（**基本类型比较的是值是否相等，引用数据类型比较的是内存地址**）
> - `equal()`：方法存在于Object类中，所有类都可以重写这个方法，如果不重写实际效果比较的也是两个对象是否是同一个对象，与`==`效果一样
> - 所以当我们想比较两个实例对象的内容相同（或者部分内容相同）即返回true而不是比较内存地址是否一样的时候，我们则可以重写 `equals` 方法
> - `String` 类就重写了 `equals` 方法，使得当字符串内容相同的时候就返回true

- ![image-20210403162039420](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403162039420.png)

### Java 语言有哪些特点

1. 简单易学；
2. 面向对象（封装，继承，多态）；
3. 平台无关性（ Java 虚拟机实现平台无关性）；
4. GC实现垃圾回收；
5. 异常处理机制；
6. 支持多线程；
7. 支持网络编程并且很方便；
8. 编译与解释并存；

### List , Set , Map的大致区别

- ![image-20210403162548007](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403162548007.png)



### Map练习题

![QQ图片20210324221752.png](http://codezhou.com/upload/2021/03/QQ%E5%9B%BE%E7%89%8720210324221752-10551c0c7ba04426b71a03c5dc7db98c.png)

- **第一次输出**：输出P1和P2，因为remove(p1)方法没有删除到p1，原因是p1的name字段已经被修改了，remove方法是根据哈希码计算索引位置删除该索引位置的对象，但是Person类的hashCode方法已经被重写为根据id和name计算，所以删除时候计算的哈希码与原先存储时候的哈希码不一样了，删除到其他索引位置去了，并没有删除p1
- **第二次输出**：输出三个对象，因为第三次添加的时候，计算出的索引位置并没有存放其他数据，跟它现在name和id相同的p1位置是根据修改前的name计算出的索引来存储的，所以添加成功
- **第三次输出**：输出四个对象，因为第四次添加的时候，计算出索引位置和p1相同，但是调用equals方法去比较的时候，name不相同，于是链接到了p1的后面，添加成功



### 事务的ACID和数据库三范式

- ![image-20210403163516230](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403163516230.png)



### 事务的四种隔离级别

- ![image-20210403163507330](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403163507330.png)



### MyISAM和InnoDB区别

- ![image-20210403163556401](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403163556401.png)



### 乐观锁和悲观锁，以及乐观锁的实现方式

![image-20210403163753247](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403163753247.png)



### 读锁和写锁

- ![image-20210403163910874](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403163910874.png)



### 事务的原理

- ![image-20210403164046550](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403164046550.png)



###  数据库中char 和 varchar

![image-20210403164227696](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403164227696.png)





### 检查异常和非检查异常的区别

![image-20210403165042045](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403165042045.png)

### 异常处理中的finally的特性

- finally块不管异常是否发生，**只要对应的try执行了，则它一定也执行。只有一种方法让finally块不执行：System.exit()**。因此finally块通常用来做资源释放操作：关闭文件，关闭数据库连接等等。
- 良好的编程习惯是：**在try块中打开资源，在finally块中清理释放这些资源**。
- **在 try块中即便有return，break，continue等改变执行流的语句，finally也会执行。**

### 

### Mysql索引的分类和哪些情况适合建立索引

- ![image-20210403183404505](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403183404505.png)

-  哪些情况适合建立索引
  - 主键自动建立唯一索引:primary
  - **频繁作为查询条件的字段应该创建索引**：比如银行系统银行帐号,电信系统的手机号
  - **查询中与其它表关联的字段,外键关系建立索引**：比如员工表的部门外键
  - **频繁更新的字段不适合建立索引**：每次更新不单单更新数据,还要更新索引
  - **where条件里用不到的字段不建立索引**
  - **查询中排序的字段,排序的字段若通过索引去访问将大大提升排序速度**
  - **查询中统计或分组的字段**
  - 经常增删改的表和数据重复的表字段不适合建立索引
- **最佳左前缀法则** : **如果索引有多列,要遵守最左前缀法则,指的就是从索引的最左列开始 并且不跳过索引中的列**



### MySQL如何正确使用索引

- **使用全值匹配**，使用全部的复合索引一起查询是性能最好的，也可以最大限度避免索引的失效
-  **最佳左前缀法则** : 如果索引有多列,要遵守最左前缀法则,指的就是从索引的最左列开始 并且不跳过索引中的列
- **不在索引列上做任何操作** : 计算,函数,类型转换会导致索引失效而转向全表扫描 如：`EXPLAIN SELECT * FROM employee WHERE trim(NAME)='白骨精'`
- **mysql在使用不等于(!=或者<>)的时候无法使用索引会导致全表扫描**
- **少用or 用or连接时, 会导致索引失效**
- like**以通配符开头(**%)索引失效变成全表扫描



### 重载和重写的区别

- **重载**就是同样的一个方法能够根据输入数据的不同，做出不同的处理
  - 发生在同一个类中（或者父类和子类之间），方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同。

- **重写**就是当子类继承自父类的相同方法，输入数据一样，但要做出有别于父类的响应时，你就要覆盖父类方法
  - 返回值类型、方法名、参数列表必须相同，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类。
  - 如果父类方法访问修饰符为 `private/final/static` 则子类就不能重写该方法，但是被 static 修饰的方法能够被再次声明。
  - 构造方法无法被重写

- ![image-20210403190431619](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403190431619.png)



### JVM  JDK 和 JRE 的各自的作用和区别

- Java 虚拟机（JVM）是**运行 Java 字节码的虚拟机**。JVM 有针对不同系统的特定实现（Windows，Linux，macOS），目的是使用相同的字节码，它们都会给出相同的结果。字节码和不同系统的 JVM 实现是 Java 语言“一次编译，随处可以运行”的关键所在。
  - ![image-20210403191033106](C:\Users\big\AppData\Roaming\Typora\typora-user-images\image-20210403191033106.png)
- **JDK 是 Java Development Kit 缩写，它是功能齐全的 Java SDK。它拥有 JRE 所拥有的一切，还有编译器（javac）和工具（如 javadoc 和 jdb）。它能够创建和编译程序。**
- **JRE 是 Java 运行时环境。它是运行已编译 Java 程序所需的所有内容的集合，包括 Java 虚拟机（JVM），Java 类库，java 命令和其他的一些基础构件。但是，它不能用于创建新程序。**
- 如果你只是为了运行一下 Java 程序的话，那么你只需要安装 JRE 就可以了。如果你需要进行一些 Java 编程方面的工作，那么你就需要安装 JDK 了。但是，这不是绝对的。有时，即使您不打算在计算机上进行任何 Java 开发，仍然需要安装 JDK。例如，如果要使用 JSP 部署 Web 应用程序，那么从技术上讲，您只是在应用程序服务器中运行 Java 程序。那你为什么需要 JDK 呢？因为应用程序服务器会将 JSP 转换为 Java servlet，并且需要使用 JDK 来编译 servlet。



###  Java 泛型了解么？什么是类型擦除？介绍一下常用的通配符

- 该机制允许程序员在编译时检测到非法的类型。泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。

- **Java 的泛型是伪泛型，这是因为 Java 在编译期间，所有的泛型信息都会被擦掉，这也就是通常所说类型擦除** ，如下面的例子，反射得到类就没有泛型的限制

  - ```java
    List<Integer> list = new ArrayList<>();
    list.add(12);
    //这里直接添加会报错
    list.add("a");
    Class<? extends List> clazz = list.getClass();
    Method add = clazz.getDeclaredMethod("add", Object.class);
    //但是通过反射添加，是可以的
    add.invoke(list, "kl");
    System.out.println(list)
    ```

    

- **常用的通配符为： T，E，K，V，？**

  - ？ 表示不确定的 java 类型
  - T (type) 表示具体的一个 java 类型
  - K V (key value) 分别代表 java 键值中的 Key Value
  - E (element) 代表 Element



###  Java 中的几种基本数据类型是什么？对应的包装类型是什么？各自占用多少字节？

- Java**中**有 8 种基本数据类型：byte、short、int、long、float、double  char  boolean
- 八种基本类型都有对应的包装类分别为：Byte、Short、Integer、Long、Float、Double、Character、Boolean

| 基本类型 | 位数 | 字节 | 默认值  |
| -------- | ---- | ---- | ------- |
| int      | 32   | 4    | 0       |
| short    | 16   | 2    | 0       |
| long     | 64   | 8    | 0L      |
| byte     | 8    | 1    | 0       |
| char     | 16   | 2    | 'u0000' |
| float    | 32   | 4    | 0f      |
| double   | 64   | 8    | 0d      |
| boolean  | 1    |      | false   |

- 对于 boolean，官方文档未明确定义，它依赖于 JVM 厂商的具体实现。逻辑上理解是占用 1 位，但是实际中会考虑计算机高效存储因素



###  构造方法有哪些特性



### 面向对象的三大特征

- 封装：封装是指把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性、

- 继承：**继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能**，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高我们的开发效率。

  - 子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问，**只是拥有**。
  - 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。

- 多态：顾名思义，表示一个对象具有多种的状态。具体表现为父类的引用指向子类的实例。

  - 对象类型和引用类型之间具有继承（类）/实现（接口）的关系；
  - 引用类型变量发出的方法调用的到底是哪个类中的方法，必须在程序运行期间才能确定；
  - 多态不能调用“只在子类存在但在父类不存在”的方法；
  - 如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法。

  

### String StringBuffer 和 StringBuilder 的区别是什么? String 为什么是不可变的

- `String` 类中使用 final 关键字修饰字符数组来保存字符串，`private final char value[]`，所以`String` 对象是不可变的
- `StringBuilder` 与 `StringBuffer` 都继承自 `AbstractStringBuilder` 类，在 `AbstractStringBuilder` 中也是使用字符数组保存字符串`char[]value` 但是没有用 `final` 关键字修饰，所以这两种对象都是可变的
- 线程安全性：
  - `String` 中的对象是不可变的，也就可以理解为常量，线程安全
  - `StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，线程安全
  - `StringBuilder` 并没有对方法进行加同步锁，线程不安全
- **对于三者使用的总结：**
  1. 操作少量的数据: 适用 `String`
  2. 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
  3. 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`



### Object 类的常见方法有哪些

```java
public final native Class<?> getClass()//native方法，用于返回当前运行时对象的Class对象，使用了final关键字修饰，故不允许子类重写。

public native int hashCode() //native方法，用于返回对象的哈希码，主要使用在哈希表中，比如JDK中的HashMap。
public boolean equals(Object obj)//用于比较2个对象的内存地址是否相等，String类对该方法进行了重写用户比较字符串的值是否相等。

protected native Object clone() throws CloneNotSupportedException//naitive方法，用于创建并返回当前对象的一份拷贝。一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，x.clone().getClass() == x.getClass() 为true。Object本身没有实现Cloneable接口，所以不重写clone方法并且进行调用的话会发生CloneNotSupportedException异常。

public String toString()//返回类的名字@实例的哈希码的16进制的字符串。建议Object所有的子类都重写这个方法。

public final native void notify()//native方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。

public final native void notifyAll()//native方法，并且不能重写。跟notify一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。

public final native void wait(long timeout) throws InterruptedException//native方法，并且不能重写。暂停线程的执行。注意：sleep方法没有释放锁，而wait方法释放了锁 。timeout是等待时间。

public final void wait(long timeout, int nanos) throws InterruptedException//多了nanos参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上nanos毫秒。

public final void wait() throws InterruptedException//跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念

protected void finalize() throws Throwable { }//实例被垃圾回收器回收的时候触发的操作
```



###  简述线程、程序、进程的基本概念。以及他们之间关系是什么?

- **线程**与进程相似，但线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。

- **程序**是含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中，也就是说程序是静态的代码。

- **进程**是程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。简单来说，一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，同时，每个进程还占有某些系统资源如 CPU 时间，内存空间，文件，输入输出设备的使用权等等。换句话说，当程序在执行时，将会被操作系统载入内存中。 线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。



### 线程有哪些基本状态?

Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态（图源《Java 并发编程艺术》4.1.4 节）

![Java线程的状态](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/19-1-29/Java%E7%BA%BF%E7%A8%8B%E7%9A%84%E7%8A%B6%E6%80%81.png)



线程在生命周期中并不是固定处于某一个状态而是随着代码的执行在不同状态之间切换。Java 线程状态变迁如下图所示（图源《Java 并发编程艺术》4.1.4 节）：

![Java线程状态变迁](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/19-1-29/Java%20%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81%E5%8F%98%E8%BF%81.png)

### BIO,NIO,AIO 有什么区别?(这个还不熟悉)

- **BIO (Blocking I/O):** 同步阻塞 I/O 模式，数据的读取写入必须阻塞在一个线程内等待其完成。在活动连接数不是特别高（小于单机 1000）的情况下，这种模型是比较不错的，可以让每一个连接专注于自己的 I/O 并且编程模型简单，也不用过多考虑系统的过载、限流等问题。线程池本身就是一个天然的漏斗，可以缓冲一些系统处理不了的连接或请求。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。
- **NIO (Non-blocking/New I/O):** NIO 是一种同步非阻塞的 I/O 模型，在 Java 1.4 中引入了 NIO 框架，对应 java.nio 包，提供了 Channel , Selector，Buffer 等抽象。NIO 中的 N 可以理解为 Non-blocking，不单纯是 New。它支持面向缓冲的，基于通道的 I/O 操作方法。 NIO 提供了与传统 BIO 模型中的 `Socket` 和 `ServerSocket` 相对应的 `SocketChannel` 和 `ServerSocketChannel` 两种不同的套接字通道实现,两种通道都支持阻塞和非阻塞两种模式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模式正好与之相反。对于低负载、低并发的应用程序，可以使用同步阻塞 I/O 来提升开发速率和更好的维护性；对于高负载、高并发的（网络）应用，应使用 NIO 的非阻塞模式来开发
- **AIO (Asynchronous I/O):** AIO 也就是 NIO 2。在 Java 7 中引入了 NIO 的改进版 NIO 2,它是异步非阻塞的 IO 模型。异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。AIO 是异步 IO 的缩写，虽然 NIO 在网络操作中，提供了非阻塞的方法，但是 NIO 的 IO 行为还是同步的。对于 NIO 来说，我们的业务线程是在 IO 操作准备好时，得到通知，接着就由这个线程自行进行 IO 操作，IO 操作本身是同步的。查阅网上相关资料，我发现就目前来说 AIO 的应用还不是很广泛，Netty 之前也尝试使用过 AIO，不过又放弃了。



### **引用类型的大致创建过程**

现在为其创建一个对象Student d1= new Student (8,8,2021);

在内存中的具体创建过程是：

1）首先在栈内存中位其d1分配一块空间；

2）然后在堆内存中为Student 对象分配一块空间，并为其三个属性设初值0，0，0；

3）根据类Student 中对属性的定义，为该对象的三个属性进行赋值操作；

4）调用构造方法，为三个属性赋值为8，8，2021；（注意这个时候d1与Student 对象之间还没有建立联系）

5）将Student 对象在堆内存中的地址，赋值给栈中的d1;通过句柄d1可以找到堆中对象的具体信息。



### final 在 java 中有什么作用

- final 修饰的类叫最终类，该类不能被继承。
- final 修饰的方法不能被重写。
- final 修饰的变量不可更改，**其不可更改指的是其引用不可修改，对于引用类型值还是可能改变的，举个列子：String 内部对于 value 的定义；而对于基本类型来说就叫做常量了。**
- **final、finally、finalize 有什么区别？**
  - final可以修饰类、变量、方法，修饰类表示该类不能被继承、修饰方法表示该方法不能被重写、修饰变量表示该变量是一个常量不能被重新赋值。
  - finally一般作用在try-catch代码块中，在处理异常的时候，通常我们将一定要执行的代码方法finally代码块中，表示不管是否出现异常，该代码块都会执行，一般用来存放一些关闭资源的代码。
  - finalize是一个方法，属于Object类的一个方法，而Object类是所有类的父类，该方法一般由垃圾回收器来调用，当我们调用System的gc()方法的时候，由垃圾回收器调用finalize(),回收垃圾。



### String 为什么设置为不可变？

- **为了实现字符串常量池**(如果字符串是可变的，某一个字符串变量改变了其值，那么其指向的变量的值也会改变，String pool将不能够实现)
- **为了线程安全**(字符串自己便是线程安全的)
- **可以加快字符串处理速度，保证同一个对象调用 hashCode() 都产生相同的值**，于是在创建对象时其hashcode就可以放心的缓存，不需要重新计算。这也是 Map 类的 key 使用 String 的原因。



### 接口和抽象类的区别

- 接口中不能有非抽象方法，而抽象方法中可以有非抽象方法
- 实现接口的关键字为implements，继承抽象类的关键字为extends。一个类可以实现多个接口，但一个类只能继承一个抽象类。所以，使用接口可以间接地实现多重继承
- 接口成员变量默认为public static final，必须赋初值，不能被修改，其所有的成员方法都是public、abstract的，抽象方法可以有public、protected这些修饰符



### 有关字符串常量池

（JVM每个内存区域以及一些常量池的知识点整理下）



### 















### 琐碎的知识

- new 创建对象实例（对象实例在堆内存中），对象引用指向对象实例（对象引用存放在栈内存中）。一个对象引用可以指向 0 个或 1 个对象（一根绳子可以不系气球，也可以系一个气球）;一个对象可以有 n 个引用指向它（可以用 n 条绳子系住一个气球
- 构造方法的特性：
  - 名字与类名相同
  - 没有返回值，但不能用 void 声明构造函数
- 在调用子类构造方法之前会先调用父类没有参数的构造方法,其目的是帮助子类做初始化工作
- 在一个静态方法内调用一个非静态成员为什么是非法的：**静态方法在类加载的时候就会分配内存，非静态成员(变量或方法)属于类的对象，只有在类的对象产生(实例化)时才会分配内存，然后通过类的对象(实例)去访问**
- **java中的基本数据类型存放位置**：在方法中声明的基本类型变量存放在方法栈中，随着方法栈出栈而失效，在类中声明的基本类型变量其变量名及其值放在堆内存中
- **用最有效率的方法计算 2 乘以 8**：2 << 3（左移 3 位相当于乘以 2 的 3 次方，右移 3 位相当于除以 2 的 3 次方）。





