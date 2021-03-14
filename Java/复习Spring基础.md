# 复习Spring基础

## 1. IOC

### 1.1 定义
 IOC译为“控制反转”，意思为将创建（new）对象的控制权全部上缴给“第三方”IOC容器，IOC容器成了整个系统的关键核心，起到了解耦的作用，由上可知创建对象的控制权反转了，不再是由自己来创建对象。
### 1.2 用xml配置方式创建 bean
![image.png](http://codezhou.com/upload/2021/01/image-e0ae2f1fda504d1e9955da60b65c7be7.png)
![image.png](http://codezhou.com/upload/2021/01/image-3d063416e5ec4dd397191efb9cdec0a6.png)
### 1.3 创建bean的三种方式:
![image.png](http://codezhou.com/upload/2021/01/image-5fd2f44392ff44dfaea89efd07d79e91.png)
![image.png](http://codezhou.com/upload/2021/01/image-aeb11ae4e2904231bd004fa71334d06f.png)
### 1.4 bean的作用范围:(Scope)
Spring定义了多种 Bean 作用域，可以基于这些作用域创建 bean，包括：
- 单例（Singleton）：整个应用中，只创建bean的一个实例。
- 原型（Prototype）：多例，每次都会创建一个新的bean实例。
- 会话（Session）：在Web应用中每个会话创建一个bean实例。
- 请求（Rquest）：在Web应用每个请求创建一个bean实
### 1.5 bean对象的生命周期:
![image.png](http://codezhou.com/upload/2021/01/image-d9b67ce924ed4e6e8961a2648f1dd002.png)
### 1.6 依赖注入的方式
 - Type1－接口注入（Interface Injection）（没用过）
 - Type2－set方法注入（Setter Injection）
 - Type3－构造方法注入（Constructor Injection）
 - 使用注解注入

## 2. AOP
### 2.1 定义
百度百科：AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。**利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性**，同时提高了开发的效率。
### 2.2 AOP相关的名词介绍
 - **通知（Advice）**
    通知定义了切面是什么以及何时使用。除了描述切面要完成的工作，通知还解决了何时执行这个工作的问题。它应该应用在某个方法被调用之前？之后？之前和之后都调用？还是只在方法抛出异常时调用？

Spring切面可以应用5种类型的通知：
> 前置通知（Before）：在目标方法被调用之前调用通知功能；
> 后置通知（After）：在目标方法完成之后调用通知，此时不会关心方法的输出是什么；
> 返回通知（After-returning）：在目标方法成功执行之后调用通知；
> 异常通知（After-throwing）：在目标方法抛出异常后调用通知；
> 环绕通知（Around）：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为。


 - **连接点（Join point）**
   连接点是在应用执行过程中能够插入切面的一个点。这个点可以是调用方法时、抛出异常时、甚至修改一个字段时。切面代码可以利用这些点插入到应用的正常流程之中，并添加新的行为。

 - **切点（Pointcut）**
如果说通知定义了切面的“什么”和“何时”的话，那么切点就定义了“何处” 。切点的定义会匹配通知所要织入的一个或多个连接点。我们通常使用明确的类和方法名称，或是利用正则表达式定义所匹配的类和方法名称来指定这些切点。有些AOP框架允许我们创建动态的切点，可以根据运行时的决策（比如方法的参数值）来决定是否应用通知。

 - **切面（Aspect）**
通知+切点=切面

 - **引入（Introduction）**
引入允许我们向现有的类添加新方法或属性

 - **织入（Weaving）**
织入是把切面应用到目标对象并创建新的代理对象的过程。切面在指定的连接点被织入到目标对象中。在目标对象的生命周期里有多个点可以进行织入

### 2.3 AOP 四个通知的XML配置 以及通用切入点表达式:
![image.png](http://codezhou.com/upload/2021/01/image-ae2d305941614c908cc9b7a848aa3049.png)







## 3. spring的注解配置
### 3.1 IOC相关的注解
 - xml配置和注解配置的对应关系:
 ![image.png](http://codezhou.com/upload/2021/01/image-9edba9937aa541979e87eb60c044dd19.png)
 - 创建对象的注解:
![image.png](http://codezhou.com/upload/2021/01/image-0e165b6e6021469aabec5a36ca454bbd.png)、
![image.png](http://codezhou.com/upload/2021/01/image-8c79d035e61c4b699353068b4f178000.png)
 - 注入数据的注解：
 ![image.png](http://codezhou.com/upload/2021/01/image-8a68163a1a0f47fa8451eaaa32101132.png)
![image.png](http://codezhou.com/upload/2021/01/image-251c17e54ce64daaaa451ec6a1c0cded.png)
 - 其他注入数据的方式:
 ![image.png](http://codezhou.com/upload/2021/01/image-3325cef589a04028a59e57b7d5bcc232.png)
![image.png](http://codezhou.com/upload/2021/01/image-77bdb87a3583420faaa233b3ce20d050.png)
 - 改变作用范围的注解:
 ![image.png](http://codezhou.com/upload/2021/01/image-2036e42b59fd463eb9c12e9ff76ec9e6.png)
 - 当全部用注解来配置不用Xml时 需要的spring新的注解:
 ![image.png](http://codezhou.com/upload/2021/01/image-004097798a434bc2be291d2e63fe4e03.png)
 - 将不是自己创建的对象存入spring容器当中的注解：
 ![image.png](http://codezhou.com/upload/2021/01/image-e472b4c5a94a4483bff8b780789c2c89.png)
 ![image.png](http://codezhou.com/upload/2021/01/image-29f74ace6e5347b1aab9b772c23bf744.png)

### 3.2 AOP相关的注解

- 切入点表达式的注解：
![image.png](http://codezhou.com/upload/2021/01/image-5c07e1203d554b8b948bd53e8577681a.png)
- 环绕通知的注解：
![image.png](http://codezhou.com/upload/2021/01/image-1f927730627748ada9c52ad9362f625e.png)
- 其余四个通知的注解配置格式如下：
```java
@Aspect
public class Audience{
  //配置切入点
  @Pointcut("execution(** concert.Performance.perform(..))")
  public void performance(){}
 
  @Before("performance()")
  public void silenceCellPhones(){
    System.out.println("Silencing cell phones");
  }
  @Before("performance()")
  public void taskSeats(){
    System.out.println("Talking seats");
  }
  @AfterReturning("performance()")
  public void applause(){
    System.out.println("CLAP CLAP CLAP!!!");
  }
  @AfterThrowing("performance()")
  public void demanRefund(){
    System.out.println("Demanding a refund");
  }
}

```
- 有关事务的注解：
![image.png](http://codezhou.com/upload/2021/01/image-b2ab1a6879824b4c85be3174e57bacd0.png)
![image.png](http://codezhou.com/upload/2021/01/image-983c965bf5f2422899a59eb1d20a6ab8.png)
> 事务控制的注解配置 配置比XML简单很多 但是要是多个事务的属性不同的话 就要分别配置属性 此时就无法像XML可以使用通配符方便了

### 3.3 其他注解
 - 读取配置文件中的数据的注解@@ConfigurationProperties
 [详细介绍参考此处](https://www.cnblogs.com/tian874540961/p/12146467.html)
- @ConditionalOnProperty根据是否满足条件来决定导入数据：

![image.png](http://codezhou.com/upload/2021/01/image-4ab77b1a42994a36bf600609350607ea.png)
![image.png](http://codezhou.com/upload/2021/01/image-5b21d86bd6d544c9b78f0e733acc9eb4.png)