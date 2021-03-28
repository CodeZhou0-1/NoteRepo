# Java中的异常机制

### 概念
异常：**是Java传达给你的系统和程序错误的方式**。

- 在java中，异常功能是通过实现比如Throwable，Exception，RuntimeException之类的类，然后还有一些处理异常时候的关键字，比如throw，throws，try，catch，finally之类的。 
- 所有的异常都是通过Throwable衍生出来的。Throwable把错误进一步划分为 - java.lang.Exception 和 java.lang.Error.java.lang.Error 用来处理系统错误，例如java.lang.StackOverFlowError 之类的。然后Exception用来处理程序错误，请求的资源不可用等等。

### 异常类概览
![image.png](http://codezhou.com/upload/2021/03/image-a25c01411c8e46a1b4ce4f22c9096440.png)

- Throwable是java语言中所有错误和异常的超类（万物即可抛），它有
两个子类：Error、Exception
  - 错误：Error类以及他的子类的实例，代表了JVM本身的错误。错误不能被程序员通过代码处理，Error很少出现。因此，程序员应该关注Exception为父类的分支下的各种异常类。
  - 异常：Exception以及他的子类，代表程序运行时发送的各种不期望发生的事件。可以被Java异常处理机制使用，是异常处理的核心

- Throwable 类常用方法
  - public string getMessage():返回异常发生时的简要描述
  - public string toString():返回异常发生时的详细信息
  - public string getLocalizedMessage():返回异常对象的本地化信息。使用 Throwable 的子类覆盖这个方法，可以生成本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与 getMessage（）返回的结果相同
  - public void printStackTrace():在控制台上打印 Throwable 对象封装的异常信息

- Java中的异常是线程独立的，线程的问题应该由线程自己来解决，而
不要委托到外部，也不会直接影响到其它线程的执行。


### 检查异常和非检查异常

![Java异常类层次结构图2.png](http://codezhou.com/upload/2021/03/Java%E5%BC%82%E5%B8%B8%E7%B1%BB%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84%E5%9B%BE2-b7d98defc4c64e59a99c2cbbcafc855c.png)

- 非检查异常（unckecked exception）：Error 和 RuntimeException 以及他们的子类。javac在编译时，不会提示和发现这样的异常，不要求在程序处理这些异常，这种异常是运行时发生，无法预先捕捉处理的。

- 检查异常（checked exception）：除了Error 和 RuntimeException的其它异常。javac强制要求程序员为这样的异常做预备处理工作在方法中要么用try-catch语句捕获它并处理，要么用throws子句声明抛出它，否则编译不会通过。
  - 通过把IOException声明为检查型异常，Java 确保了你能够
优雅的对异常进行处理。另一个可能的理由是，可以使用catch或finally来确保数量受限的系统资源（比如文件描述符）在你使用后尽早得到释放。（关于为什么Java要由检查异常的一些理由）

- 检查型异常和非检查型异常的主要区别在于其处理方式。检查型异常需要使用try, catch和finally关键字在编译期进行处理，否则会出现编译器会报错。对于非检查型异常则不需要这样做。Java中所有继承自java.lang.Exception类的异常都是检查型异常，所有继承自RuntimeException的异常都被称为非检查型异常。

 

### 异常处理中的finally
- finally块不管异常是否发生，只要对应的try执行了，则它一定也执行。只有一种方法让finally块不执行：System.exit()。因此finally块通常用来做资源释放操作：关闭文件，关闭数据库连接等等。

- 良好的编程习惯是：在try块中打开资源，在finally块中清理释放这些资源。

- 在 try块中即便有return，break，continue等改变执行流的语句，finally也会执行。


### 参考文章
- [文章1](https://snailclimb.gitee.io/javaguide/#/docs/java/basis/Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86?id=_32-%e5%bc%82%e5%b8%b8)

- [文章2](https://github.com/h2pl/Java-Tutorial/blob/master/docs/java/basic/10%E3%80%81Java%E5%BC%82%E5%B8%B8.md)