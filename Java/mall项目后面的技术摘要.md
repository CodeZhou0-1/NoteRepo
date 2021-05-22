# mall项目后面的技术摘要

### Nginx动静分离
- 由于动态资源和静态资源目前都处于服务端，所以为了减轻服务器压力，我们将js、css、img 等静态资源放置在 Nginx 端，以减轻服务器压力。

- 在 nginx 的 html 文件夹创建 staic 文件夹，并将 index/css 等静态资源全部上传到该文件夹中
  - 步骤①：修改index.html的静态资源路径，使其全部带有static前缀src="/static/index/img/img_09.png"
  - 步骤②：修改nginx的配置文件/mydata/nginx/conf/conf.d/kedamall.conf，若遇到有/static为前缀的请求，转发至 html 文件夹

### ELASTICSEARCH
-  概述
  ElasticSearch是一个搜索和分析引擎：

- 全文检索功能比MySQL强大

  - 性能更好，因为ES将数据存到内存，且天然支持分布式，不必担心内存不够的问题

    | 关系数据库    | 数据库      | 表         | 行             | 列           |
    | ------------- | ----------- | ---------- | -------------- | ------------ |
    | Elasticsearch | 索引(Index) | 类型(type) | 文档(Docments) | 字段(Fields) |

只保存检索页面需要展示的信息——sku的基本信息；
spu 在 ElasticSearch 中的存储模型的抉择：

如果每个sku都存储 同一个spu的规格参数（attrs），会有冗余存储
将 规格参数 单独建立索引会出现检索时出现大量数据传输的问题，会阻塞网络
因我们选用第一种存储模型——以空间换时间

### 缓存
- ① 为了系统性能的提升，我们一般都会将部分数据放入缓存中，加速访问

哪些数据适合放入缓存？

即时性、数据一致性要求不高的
访问量大且更新频率不高的数据（读多、写少）

- ② 本地缓存面临问题

当有多个服务存在时，每个服务的缓存仅能够为本服务使用，这样每个服务都要查询一次数据库，并且当数据更新时只会更新单个服务的缓存数据，就会造成数据不一致的问题；

解决：所有的服务都到同一个 redis 进行获取数据，就可以避免这个问题

- ③ 高并发下缓存失效问题
  - **缓存穿透**
    指查询一个一定不存在的数据，由于缓存是不命中，将去查询数据库，但是数据库也无此记录，我们没有将这次查询的 null 写入缓存。这将导致这个不存在的数据每次请求都要到数据库查询，也就失去了缓存的意义。
    风险： 利用不存在的数据进行攻击，数据库瞬时压力增大，最终导致崩溃
    **解决： null 结果缓存，并加入短暂过期时间**
  - **缓存雪崩**
    指在我们在缓冲中放了很多数据，并设置了相同的过期时间。这就导致缓存在某一时刻，数据大面积失效，这时大并发的请求将被全部转发到数据库，数据库瞬时压力过重，导致雪崩。
    **解决： 原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。**
  - **缓存击穿**
    对于一些设置了过期时间的key，如果这些key可能会在某些时间点被超高并发地访问，是一种非常“高频热点”的数据。如果这个key在大量请求同时进来前正好失效，那么所有对这个key的数据查询都落到db，我们称为缓存击穿。
    **解决： 加锁。大量并发时，只放一个去查数据库，其他人等待，查到以后释放锁，其他人获取到锁，先查缓存，就会有数据，就不用去数据库。**



### 分布式锁——Redisson

官网文档上详细说明了：不推荐使用 setnx来实现分布式锁，应该参考 The Redlock algorithm 的实现
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210102111511866.png)
在Java 语言环境下使用 Redisson



### 线程回顾
- 初始化线程的 4 种方式

  ① 继承 Thread

​       ② 实现 Runnable

​       ③ 实现 Callable 接口 + FutureTask（可以拿到返回结果，可以处理异常）

​       ④ 线程池

方式一 和 方式二：主进程无法获取线程的运算结果，不适合当前场景

方式三：主进程可以获取当前线程的运算结果，但是不利于控制服务器种的线程资源，可以导致服务器资源耗尽

方式四：通过如下两种方式初始化线程池

```java
Executors.newFixedThreadPool(3);
//或
new ThreadPollExecutor(corePoolSize,maximumPoolSize,keepAliveTime,TimeUnit,unit,workQueue,threadFactory,handler);
```



### 异步编排

### CompletableFuture 异步编排

**业务场景：**
查询商品详情页逻辑较复杂，有些数据需要远程调用，必然需要花费更多的时间：

- 获取Sku基本信息
- 获取Sku图片信息
- 获取Sku销售属性
- 获取Sku促销（秒杀）信息

java.util.concurrent.CompletableFuture提供了四个静态方法来创建一个异步操作：

```java

public static CompletableFuture<Void> runAsync(Runnable runnable)

public static CompletableFuture<Void> runAsync(Runnable runnable,
                                                   Executor executor)

public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier)

public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier,
                                                       Executor executor)
```

- runXxx 都是没有返回结果的，supplyXxxx都是可以获取返回结果的
- 可以传入自定义的线程池，否则就是用默认的线程池
- 根据方法的返回类型来判断是否该方法是否有返回类型



### OAuth2.0社交登录
- OAuth（开放授权）是一个开放标准，允许用户授权第三方网站访问他们存储在另外的服务提供者上的信息，而不需要将用户名和密码提供给第三方网站或分享他们的数据的内容

- OAuth2.0：对于用户相关的 OpenAPI（例如获取用户信息，动态同步，照片，日志，分享等），为了保存用户数据的安全和隐私，第三方网站访问用户数据前都需要显示向用户授权

![在这里插入图片描述](http://img.codezhou.com/img/20210105101914553.png)





### 分布式 Session不共享不同步问题
问题描述：

- session不可跨域，它有自己的作用范围。例如：在auth.kedamall.com中保存session，但是网址跳转到kedamall.com中，取不出auth.kedamall.com中保存的session
- 同一个服务，复制多份，session不同步问题。

**解决方案：** 统一存储

![在这里插入图片描述](http://img.codezhou.com/img/20210105103355689.png)

### SpringSession

- 核心原理
  - @EnableRedisHttpSession导入了RedisHttpSessionConfiguration配置
  - 这个配置给容器中添加了一个组件RedisOperationsSessionRepository，这个组件是 Redis 操作 Session 的增删改查封装类。
  - 这个配置还添加了一个SessionRepositoryFilter组件，它是 Session 存储过滤器，每个请求过来必须经过这个 Filter。
    - 创建的时候，就自动从容器中获取到了上面的RedisOperationsSessionRepository
    - 核心原理——装饰者模式
      原生的获取session时是通过HttpServletRequest获取的
      这里对request进行包装成了wrappedRequest，并且重写了包装request的getSession()方法

### 购物车服务
- 数据模型分析

  - **数据存储**
    购物车是一个读多写多的场景，因此放入数据库并不合适，但购物车又是需要持久化，因此这里我们选用 Redis 存储购物车数据。

  - **数据结构**
    一个购物车是由各个购物项组成的，但是我们用List进行存储并不合适，因为使用List查找某个购物项时需要挨个遍历每个购物项，会造成大量时间损耗，为保证查找速度，我们使用Hash进行存储

![在这里插入图片描述](http://img.codezhou.com/img/20210105111645531.png)



### ThreadLocal用户身份鉴别
- **用户身份鉴别方式**
  参考京东，在点击购物车时，会为临时用户生成一个name为user-key的 Cookie 临时标识，过期时间为一个月；
  如果手动清除user-key，那么临时购物车的购物项也被清除，所以user-key是用来标识和存储临时购物车数据的

  ![在这里插入图片描述](http://img.codezhou.com/img/202101051120363.png)

- **使用ThreadLocal进行用户身份鉴别信息传递**
  在调用购物车的接口前，先通过 Session 信息判断是否登录，并分别进行用户身份信息的封装，并把user-key放在 Cookie 中
  这个功能使用拦截器进行完成



### Threadlocal基本理解

- threadlocal是做什么用的，用在哪些场景当中？
  - ThreadLocal **适用于每个线程需要自己独立的实例且该实例需要在多个方法中被使用，也即变量在线程间隔离而在方法或类间共享的场景**。简单易懂的就是。每个线程有自己的数据副本，当线程结束后可以独立回收。
  - 通常使用场景：如spring mvc收到请求。拦截器将用户id等数据放到threadlocal当中。在当前线程就可以直接拿到数据
- 如果启动另外一个线程。那么在主线程设置的threadlocal值能被子线程拿到吗?
  - 无法拿到主线程的threadlocal
  - 那我们应该怎么去解决这个问题：
    - 在java8中提供了一个这个类(**InheritableThreadLocal**)
    - 可以实现父线程到子线程的共享
  - JDK的**InheritableThreadLocal**类**可以完成父线程到子线程的值传递。**
  - 但**对于使用线程池等会池化复用线程的组件的情况，线程由线程池创建好，并且线程是池化起来反复使用的；这时父子线程关系的ThreadLocal值传递已经没有意义**
  - 如果是在线程池的情况下就需要运用其他类如：**阿里的[transmittable-thread-local](https://github.com/alibaba/transmittable-thread-local)**



 ### RabbitMQ
- 消息代理规范
  JMS（Java Message Service）JAVA消息服务
  基于JVM消息代理的规范。ActiveMQ、HornetMQ是 JMS 实现
  AMQP（Advanced Message Queuing Protocol）
  高级消息队列协议，也是一个消息代理的规范，兼容JMS
  RabbitMQ 是 AMQP 的实现

- 应用场景
  异步处理：消息发送的时间取决于业务执行的最长的时间
  应用解耦：**即使下单时库存系统不能正常使用，也不影响正常下单。因为下单后，订单系统写入消息队列就不再关心其他的后续操作了。实现订单系统与库存系统的应用解耦**

- 流量消峰：**服务器接收用户的请求后，先写入消息队列。假如消息队列长度超过最大数量，则直接抛弃用户请求或跳转到错误页面**
  **秒杀业务中根据消息队列中的请求信息，再做后续处理**

  

#### **RabbitMQ核心概念**

- Message
  - **消息**，消息是不具名的，它由消息头和消息体组成消息头
  - 包括**routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）**等

- Publisher	
  - 消息的**生产者**，也是一个向交换器发布消息的客户端应用程序

- Exchange
  - **交换器**，将生产者消息路由给服务器中的队列类型有direct(默认)，fanout，topic，和headers。具有不同转发策略

- Queue
  - **消息队列**，保存消息直到发送给消费者

- Binding
  - **绑定**，用于消息队列和交换器之间的关联

- Connection
  - **网络连接**，比如一个 TCP 连接

- Consumer
  - 消息的**消费者**，表示一个从消息队列中取得消息的客户端应用程序

- Virtual Host
  - **虚拟主机**，表示一批交换器、消息队列和相关对象。vhost 是 AMQP 概念的基础，必须在连接时指定RabbitMQ 默认的 vhost 是 /

- Broker
  - 消息队列服务器实体

#### **RabbitMQ运行机制**

- 消息路由：AMQP 中增加了 Exchange 和 Binding 的角色， Binding 决定交换器的消息应该发送到那个队列

**Exchange 类型**

- direct
  对点模式，**消息中的路由键（routing key）如果和 Binding 中的 binding 的 key 完全一致， 交换器就将消息发到对应的队列中**。
- topic
  **将路由键和某个模式进行匹配，此时队列需要绑定到一个模式上。**它将路由键和绑定键的字符串切分成单词，这些单词之间用点隔开。
  识别通配符： #匹配 0 个或多个单词， *匹配一个单词

- fanout
  **广播模式，每个发到 fanout 类型交换器的消息都会分到所有绑定的队列上去**



#### RabbitMQ消息确认机制 - 可靠到达

![在这里插入图片描述](http://img.codezhou.com/img/20210105203348630.png)

- **ConfirmCallback**

消息只要被 broker 接收到就会执行 confirmCallback，如果 cluster 模式，需要所有 broker 接收到才会调用 confirmCallback

被 broker 接收到只能表示 message 已经到达服务器，并不能保证消息一定会被投递到目标 queue 里，所以需要用到接下来的 returnCallback

- **ReturnCallback**

confirm 模式只能保证消息到达 broker，不能保证消息准确投递到目标 queue 里。在有些模式业务场景下，我们需要保证消息一定要投递到目标 queue 里，此时就需要用到 return 退回模式

- **Ack消息确认机制**

- 消费者获取到消息，成功处理，可以回复 Ack 给 Broker
  basic.ack用于肯定确认：broker 将移除此消息
  basic.nack用于否定确认：可以指定 beoker 是否丢弃此消息，可以批量
  basic.reject用于否定确认，同上，但不能批量
- 默认消息被消费者收到后是自动确认的，即消息会从 queue 中移除
  消费者收到消息，默认自动ack，但是如果无法确定此消息是否被成功处理，我们可以开启手动ack模式
  消息处理成功，ack()，接受下一条消息，此消息broker就会移除
  消息处理失败，nack()/reject() 重新发送给其他人进行处理，或者容错处理后ack
  消息一直没有调用ack/nack方法，brocker认为此消息正在被处理，不会投递给别人，此时客户端断开，消息不会被broker移除，会投递给别人

```java
# 开启发送端确认
spring.rabbitmq.publisher-confirms=true

# 开启发送端消息抵达队列的确认
spring.rabbitmq.publisher-returns=true

# 只要抵达队列，以异步发送优先回调 publisher-returns
spring.rabbitmq.template.mandatory=true

# 手动ack消息
spring.rabbitmq.listener.simple.acknowledge-mode=manual
```



###  订单确认业务流程

![在这里插入图片描述](http://img.codezhou.com/img/20210106095459286.png)

- 查询购物项、库存和收货地址都要调用远程服务，串行会浪费大量时间，**因此我们使用CompletableFuture进行异步编排**
- 可能由于延迟，订单提交按钮可能被点击多次。**为了防止重复提交（保证幂等性）**，我们在返回订单确认页时，在Redis中放入一个随机生成的令牌，过期时间为 30mi；，提交的订单时会携带这个令牌，我们将会在订单提交的处理页面核验此令牌



### Feign远程调用丢失请求头问题
Feign 远程调用的请求头中没有含有 JSESSIONID 的 Cookie，所以也就不能得到服务端的 Session 数据，Cart（远程服务）认为没登录，也就获取不了用户信息

**分析：**

- Feign 会创建一个新的 Request（没有任何请求头）

- 在 Feign 的调用过程中，会使用容器中的RequestInterceptor对RequestTemplate进行处理，因此我们可以通过向容器中导入定制的RequestInterceptor为请求加上 Cookie。

```java
@Configuration
public class KedaFeignConfig {
    //每次远程调用，都会触发拦截器
    @Bean("requestInterceptor")
    public RequestInterceptor requestInterceptor(){
        return new RequestInterceptor() {
            @Override
            public void apply(RequestTemplate requestTemplate) {
                //拦截器和原生请求都在同一个线程
                //RequestContextHolder拿到刚进来的请求
                            ServletRequestAttributes requestAttributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
            HttpServletRequest request = requestAttributes.getRequest();
            //同步请求头信息
            if(request!=null){
                requestTemplate.header("Cookie",request.getHeader("Cookie"));
                System.out.println("feign之前进行的requestInterceptor");
            }
        }
    };
}
}
```

- 由于`RequestContextHolder`使用`ThreadLocal`共享数据，所以在开启异步时获取不到老请求的信息，自然也就无法共享 Cookie 了
- 在这种情况下，我们需要在开启异步的时候将老请求的`RequestContextHolder`的数据设置进去



### RabbitMQ实现延迟队列

- 如未付款的订单，超过一定时间后，系统自动取消订单并释放占有物品

![在这里插入图片描述](http://img.codezhou.com/img/20210105205747333.png)

- **常用解决方案：**
  Spring 的 Schedule 定时任务轮询数据库、消息队列

  **缺点：**
  如果恰好在一次扫描后完成业务逻辑，那么就会等待两个扫描周期才能扫到过期的订单，不能保证时效性

- **最终解决方案：** RabbitMQ 的消息 TTL 和死信 Exchange 结合

- 定义：延迟队列存储的对象肯定是对应的延时消息；所谓"延时消息"是指当消息被发送以后，并不想让消费者立即拿到消息，而是等待指定时间后，消费者才拿到这个消息进行消费。

**实现：RabbitMQ可以通过设置队列的TTL和 死信路由 实现延迟队列**

- TTL：
  RabbitMQ 可以针对 Queue 设置x-expires或者针对 Message 设置x-message-ttl，来控制消息的生存时间；如果超时(两者同时设置以最先到期的时间为准)，则消息变为 Dead Letter(死信)

- 死信路由 DLX
  RabbitMQ 的 Queue 可以配置x-dead-letter-exchange和x-dead-letter-routing-key（可选）两个参数，如果队列内出现了 Dead Letter，则按照这两个参数重新路由转发到指定的队列。
  - x-dead-letter-exchange：出现dead letter之后将 Dead Letter 重新发送到指定 exchange
  - x-dead-letter-routing-key：出现dead letter之后将 Dead Letter 重新按照指定的 routing-key 发送

#### 定时关单与库存解锁主体逻辑
- ① 订单超时未支付触发订单过期状态修改与库存解锁
  创建订单时消息会被发送至队列order.delay.queue，经过 TTL 的时间后消息会变成死信以order.release.order的路由键经交换机转发至队列order.release.order.queue，再通过监听该队列的消息来实现过期订单的处理

如果该订单已支付，则无需处理
否则说明该订单已过期，修改该订单的状态并通过路由键order.release.other发送消息至队列stock.release.stock.queue进行库存解锁

- ② 库存锁定后延迟检查是否需要解锁库存
  在库存锁定后通过路由键stock.locked发送至延迟队列stock.delay.queue，延迟时间到，死信通过路由键stock.release转发至stock.release.stock.queue，通过监听该队列进行判断当前订单状态，来确定库存是否需要解锁

由于关闭订单和库存解锁都有可能被执行多次，因此要保证业务逻辑的幂等性，在执行业务时重新查询当前的状态进行判断
订单关闭和库存解锁都会进行库存解锁的操作，来确保业务异常或者订单过期时库存会被可靠解锁



