# 复习SpringBoot-基础

### 1.什么是 SpringBoot ？

> **Spring Boot 是**基于 Spring Framework 派生的用于**快速搭建独立的，基于生产级别的 Spring 应用的框架**，可以以最小的依赖引入来构建一个  Spring 应用。

> SpringBoot 是整合 Spring 技术栈的一站式框架
> SpringBoot 是简化 Spring 技术栈的快速开发脚手架


> - 内嵌 web 服务器(Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files))
> - 有不同开发环境下的 starter依赖，简化构建配置
> - 自动配置 Spring 以及第三方功能
> - 提供生产级别的监控、健康检查及外部化配置
> - 无代码生成、无需编写 XML

### 2. 上手 SpringBoot
#### 2.1 创建maven工程之后引入与SpringBoot有关的maven依赖
```java
   <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>


    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

    </dependencies>

```
> - 以spring-boot-starter-parent作为父项目
> - 引入与Web相关的starter依赖
#### 2.2 创建主程序
```java 
/**
 * 主程序类
 * @SpringBootApplication：表明这是一个SpringBoot应用
 */
@SpringBootApplication
public class SpringBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootApplication.class,args);
    }

}
```
>  - 注解的扫描会默认扫描主程序所在包以及子包
>  - 所以主程序类一般放置顶层包下方便扫描
>
>  ####  2.3 编写业务逻辑代码
```java
@RestController
public class HelloController {


    @RequestMapping("/hello")
    public String handle01(){
        return "Hello, Spring Boot!";
    }

}
```
> - 运行测试成功返回Json数据

#### 2.4 修改SpringBoot的配置文件
创建`application.yaml`文件，并编写需要设置的配置，比如将端口号设置为8081
```xml
server
   port: 8888
```
#### 2.5 将项目打包成jar包运行
```xml
   <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```
> 在pom文件中引入以上插件将项目打包方式改成jar包

### 3. SpringBoot特点
#### 3.1 依赖管理
- 父项目做依赖管理
```java
// SpringBoot父项目中对许多开发相关的maven依赖做了依赖管理防止依赖冲突   
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
</parent>

他的父项目
 <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.4.RELEASE</version>
  </parent>

几乎声明了所有开发中常用的依赖的版本号,自动版本仲裁机制
```
- 开发导入starter场景启动器
> 1、见到很多 spring-boot-starter-* ： *就某种场景
> 2、只要引入starter，这个场景的所有常规需要的依赖我们都自动引入
> 3、SpringBoot所有支持的场景
> https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter
> 4、见到的  *-spring-boot-starter： 第三方为我们提供的简化开发的场景启动器。
> 5、所有场景启动器最底层的依赖
> <dependency>
> <groupId>org.springframework.boot</groupId>
> <artifactId>spring-boot-starter</artifactId>
> <version>2.3.4.RELEASE</version>
> <scope>compile</scope>
> </dependency>

- 无需关注版本号，自动版本仲裁
> 1、引入依赖默认都可以不写版本
> 2、引入非版本仲裁的jar，要写版本号。

- 可以修改默认版本号
```xml
1、查看spring-boot-dependencies里面规定当前依赖的版本 用的 key。
2、在当前项目里面重写配置
    <properties>
        <mysql.version>5.1.43</mysql.version>
    </properties>
```
#### 3.2 自动配置
> • 各种配置拥有默认值
> • 默认配置最终都是映射到某个类上，如：MultipartProperties
> • 配置文件的值最终会绑定每个类上，这个类会在容器中创建对象
> • 按需加载所有自动配置项
> • 引入了哪些场景这个场景的自动配置才会开启
> • SpringBoot所有的自动配置功能都在 spring-boot-autoconfigure 包里面

#### 3.3 组件添加
##### 1、@Configuration

- 基本使用
- **Full模式与Lite模式**


   - 配置 类组件之间无依赖关系用Lite模式加速容器启动过程，减少判断
        - 配置类组件之间有依赖关系，方法会被调用得到之前单实例组件，用Full模式


> 1、配置类里面使用@Bean标注在方法上给容器注册组件，默认也是单实例的
> 2、配置类本身也是组件
> 3、proxyBeanMethods：代理bean的方法
> Full(proxyBeanMethods = true)、【保证每个@Bean方法被调用多少次返回的组件都是单实例的】
>   Lite(proxyBeanMethods = false)【每个@Bean方法被调用多少次返回的组件都是新创建的】
>  组件依赖必须使用Full模式默认。其他默认是否Lite模式

```java
@Configuration(proxyBeanMethods = false) //告诉SpringBoot这是一个配置类 == 配置文件
public class MyConfig {

    /**
     * Full:外部无论对配置类中的这个组件注册方法调用多少次获取的都是之前注册容器中的单实例对象
     * @return
     */
    @Bean //给容器中添加组件。以方法名作为组件的id。返回类型就是组件类型。返回的值，就是组件在容器中的实例
    public User user01(){
        User zhangsan = new User("zhangsan", 18);
        //user组件依赖了Pet组件
        zhangsan.setPet(tomcatPet());
        return zhangsan;
    }

    @Bean("tom")
    public Pet tomcatPet(){
        return new Pet("tomcat");
    }
}


################################@Configuration测试代码如下########################################
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.atguigu.boot")
public class MainApplication {

    public static void main(String[] args) {
        //1、返回我们IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);

        //2、查看容器里面的组件
        String[] names = run.getBeanDefinitionNames();
        for (String name : names) {
            System.out.println(name);
        }

        //3、从容器中获取组件

        Pet tom01 = run.getBean("tom", Pet.class);

        Pet tom02 = run.getBean("tom", Pet.class);

        System.out.println("组件："+(tom01 == tom02));


        //4、com.atguigu.boot.config.MyConfig$$EnhancerBySpringCGLIB$$51f1e1ca@1654a892
        MyConfig bean = run.getBean(MyConfig.class);
        System.out.println(bean);

        //如果@Configuration(proxyBeanMethods = true)代理对象调用方法。SpringBoot总会检查这个组件是否在容器中有。
        //保持组件单实例
        User user = bean.user01();
        User user1 = bean.user01();
        System.out.println(user == user1);


        User user01 = run.getBean("user01", User.class);
        Pet tom = run.getBean("tom", Pet.class);

        System.out.println("用户的宠物："+(user01.getPet() == tom));



    }
}
```

##### 2、@Bean、@Component、@Controller、@Service、@Repository



##### 3、@ComponentScan、@Import

```
 * 4、@Import({User.class, DBHelper.class})
 *      给容器中自动创建出这两个类型的组件、默认组件的名字就是全类名
 */

@Import({User.class, DBHelper.class})
@Configuration(proxyBeanMethods = false) //告诉SpringBoot这是一个配置类 == 配置文件
public class MyConfig {
}
```



##### 4、@Conditional

条件装配：满足Conditional指定的条件，则进行组件注入

```java
=====================测试条件装配==========================
@Configuration(proxyBeanMethods = false) //告诉SpringBoot这是一个配置类 == 配置文件
//@ConditionalOnBean(name = "tom")
@ConditionalOnMissingBean(name = "tom")
public class MyConfig {


    /**
     * Full:外部无论对配置类中的这个组件注册方法调用多少次获取的都是之前注册容器中的单实例对象
     * @return
     */

    @Bean //给容器中添加组件。以方法名作为组件的id。返回类型就是组件类型。返回的值，就是组件在容器中的实例
    public User user01(){
        User zhangsan = new User("zhangsan", 18);
        //user组件依赖了Pet组件
        zhangsan.setPet(tomcatPet());
        return zhangsan;
    }

    @Bean("tom22")
    public Pet tomcatPet(){
        return new Pet("tomcat");
    }
}

public static void main(String[] args) {
        //1、返回我们IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);

        //2、查看容器里面的组件
        String[] names = run.getBeanDefinitionNames();
        for (String name : names) {
            System.out.println(name);
        }

        boolean tom = run.containsBean("tom");
        System.out.println("容器中Tom组件："+tom);

        boolean user01 = run.containsBean("user01");
        System.out.println("容器中user01组件："+user01);

        boolean tom22 = run.containsBean("tom22");
        System.out.println("容器中tom22组件："+tom22);


    }
```
##### 5.原生配置文件引入
- @ImportResource
```xml
======================beans.xml=========================
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <bean id="haha" class="com.atguigu.boot.bean.User">
        <property name="name" value="zhangsan"></property>
        <property name="age" value="18"></property>
    </bean>

    <bean id="hehe" class="com.atguigu.boot.bean.Pet">
        <property name="name" value="tomcat"></property>
    </bean>
</beans>
```
```java
@ImportResource("classpath:beans.xml")
public class MyConfig {}

======================测试=================
        boolean haha = run.containsBean("haha");
        boolean hehe = run.containsBean("hehe");
        System.out.println("haha："+haha);//true
        System.out.println("hehe："+hehe);//true
```

#### 3.4 读取application配置文件绑定到JavaBean
 - @Component + @ConfigurationProperties
```java
/**
 * 只有在容器中的组件，才会拥有SpringBoot提供的强大功能
 */
@Component
@ConfigurationProperties(prefix = "ruoyi")
public class Car {

    private String brand;
    private Integer price;

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public Integer getPrice() {
        return price;
    }

    public void setPrice(Integer price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", price=" + price +
                '}';
    }
}
```

### 4.文章截取并稍加修改自一下文章：
https://www.yuque.com/atguigu/springboot/rmxq85