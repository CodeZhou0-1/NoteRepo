### 一. @Bean和@Component相通之处

> - 最终效果都是为spring容器注册Bean.
> - 注册Bean之后都可以使用@Autowired或者@Resource注解注入

### 二. @Bean和@Component的不同之处

#### 2.1 使用@Component注解代码格式如下

```java
@Component
public class User {
 
    private String name = "zhangsan";
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
}
```

> - @Component使用类路径扫描**自动检测**并配置Bean
> - @Component是**类级别的注释，**而@Bean是**方法级别的注释**.

#### 2.2 使用@Bean注解代码格式如下

```java
@Configuration
public class UserPayConfig {
    @Bean
    public User User(){
        return new User();
    }
}
```

> - @Bean通常和@Configuration注解搭配使用
> - @Bean是**方法级别的注释**，该方法的名称用作Bean名称
> - @Bean**显式声明**单个Bean，而不是让Spring自动执行。

#### 2.3 @Bean和@Component的不同使用场景

> - **如果想将第三方的类变成组件，又没有源代码，也就没办法使用`@Component`进行自动配置，这种时候使用`@Bean`就比较合适了。**
> - @Component注解一般用于自定义类上将其注入spring容器中进行管理。

 

#### 2.4 参考文章

[https://stackoverflow.com/questions/10604298/spring-component-versus-bean#](https://stackoverflow.com/questions/10604298/spring-component-versus-bean#)

### 三. 顺便复习一下@RestController注解（凑凑字数 ==。）
#### 3.1 注解功能：

> @RestController注解是@Controller和@ResponseBody的合集,**表示这是个控制器 bean,并且是将函数的返回值直接填入 HTTP 响应体中,是 REST 风格的控制器。**

#### 3.2 注解应用场景

> 单独使用 @Controller 不加 @ResponseBody的话一般使用在要返回一个视图的情况，这种情况属于比较传统的 Spring MVC 的应用，对应于前后端不分离的情况。**@Controller +@ResponseBody 返回 JSON 或 XML 形式数据**

#### 3.3 @ResponseBody
> 　@ResponseBody注解的作用是将controller的方法返回的对象 通过适当的转换器 转换为指定的格式之后，**写入到Response对象的Body区（响应体中）**，通常用来返回JSON数据或者是XML。

 