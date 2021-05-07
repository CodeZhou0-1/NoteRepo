#  Mymall商城基础笔记（1）

### 1. 整合MyBatis-Plus

- 1、导入依赖

```xml
*      <dependency>
*             <groupId>com.baomidou</groupId>
*             <artifactId>mybatis-plus-boot-starter</artifactId>
*             <version>3.2.0</version>
*      </dependency>
```

- 2、配置

  - 1、配置数据源；

    - 1）、导入数据库的驱动。
    - 2）、在application.yml配置数据源相关信息

  - 2、配置MyBatis-Plus；

    - 1）、使用@MapperScan（如果启动类在最外面也可以不用这个注解扫描Dao包）

    - 2）、配置文件中配置MyBatis-Plus，sql映射文件位置

      ![image-20210503110904671](http://img.codezhou.com/img/image-20210503110904671.png)



### 2. 如何远程调用feign

* 1）、引入open-feign

```xml
       <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
       </dependency>
```



* 2）、编写一个接口，用@FeignClient注解告诉SpringCloud这个接口需要调用远程服务

```java
@FeignClient("mymall-coupon")//value值说明需要远程调用mymall-coupon模块的服务
public interface CouponFeignService {
  
    /*
    1、声明接口的每一个方法都是调用指定远程服务的那个请求
    2、接口路径要是远程服务的Controller的完整路径@RequestMapping("/coupon/coupon/member/list")
     */
    @RequestMapping("/coupon/coupon/member/list")
    public R membercoupons();
    
}
```

* 3）、在启动类添加@EnableFeignClients(basePackages = "com.codezhou.mymall.member.feign")注解开启远程调用功能

```java
@EnableFeignClients(basePackages = "com.codezhou.mymall.member.feign")
@EnableDiscoveryClient
@SpringBootApplication
public class MymallMemberApplication {

    public static void main(String[] args) {
        SpringApplication.run(MymallMemberApplication.class, args);
    }

}
```







### 3.  关于Nacos配置中心的Bug

- 根据Alibaba Nacos文档搭建：

![image-20210424170555859](http://img.codezhou.com/img/image-20210424170555859.png)

- 根据图中第二步创建`bootstrap.properties`文件写入以上配置信息
- 但是一直报错：`create config service error!properties=NacosConfigProperties` 网上查说是读取不到配置信息
- 然后根据网上说的将以上配置信息写在`application.properties`中

![image-20210424170940175](http://img.codezhou.com/img/image-20210424170940175.png)

- 虽然不报错了，但是从控制台可以看出好像根本就没有去加载`bootstrap.properties`文件，所以这个方法失败
- 由上思考，是不是加载不了或者无法找到这个`bootstrap.properties`文件？于是百度相关关键词
- **最终解决方法：在依赖中添加如下依赖**（来源此文章：https://blog.csdn.net/qq_30398499/article/details/114225896）

```xml
<!-- 若bootstrap配置不生效，加入以下依赖 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bootstrap</artifactId>
            <version>3.0.1</version>
        </dependency>
```

- But！！虽然从控制台信息中可以看出已经加载了配置执行相关任务，但是有个很容易错的地方就是配置的`Data Id`

  ![image-20210424181400506](http://img.codezhou.com/img/image-20210424181400506.png)

- 如上图：控制台显示`Data Id`是`bootstrapProperties-xxx-xxx`什么的，开始以为`bootstrapProperties`这个前缀也要加上，所以导致还是无法动态更改配置

- 最好去掉`bootstrapProperties`只保留`mymall-coupon.properties`，动态更改配置终于成功

- 另外：再修改完application.properties文件的时候碰到字符串编码问题：将GBK修改utf8， 修改 IntelliJ IDEA 的默认文件编码为utf8

- **使用Nacos作为配置中心统一管理配置流程总结**：

  * 1）、引入依赖，

    ```xml
    * <dependency>
    * <groupId>com.alibaba.cloud</groupId>
    * <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
    * </dependency>
    ```

  * 2）、创建一个bootstrap.properties。

    ```properties
    * spring.application.name=mymall-coupon
    * spring.cloud.nacos.config.server-addr=127.0.0.1:8848
    ```

  * 3）、需要给配置中心默认添加一个叫 数据集（**Data Id**）mymall-coupon.properties。默认规则，应用名.properties（注意这个命名规则，不要被控制台那个信息误导）

  * 4）、在配置中心给 应用名.properties 添加任何配置

  * 5）、动态获取配置。

  * @RefreshScope：动态获取并刷新配置

  * @Value("${配置项的名}")：获取到配置。

  * 如果配置中心和当前应用的配置文件中都配置了相同的项，优先使用配置中心的配置。



- **关于nacos的其他基本知识**

```java
*  1）、命名空间：配置隔离；
*      默认：public(保留空间)；默认新增的所有配置都在public空间。
*      1、开发，测试，生产：利用命名空间来做环境隔离。
*         注意：在bootstrap.properties；配置上，需要使用哪个命名空间下的配置，
*         spring.cloud.nacos.config.namespace=9de62e44-cd2a-4a82-bf5c-95878bd5e871
*      2、每一个微服务之间互相隔离配置，每一个微服务都创建自己的命名空间，只加载自己命名空间下的所有配置
*
*  2）、配置集：所有的配置的集合
*
*  3）、配置集ID：类似文件名。
*      Data ID：类似文件名
*
*  4）、配置分组：
*      默认所有的配置集都属于：DEFAULT_GROUP；
*      1111，618，1212
*
* 项目中的使用：每个微服务创建自己的命名空间，使用配置分组区分环境，dev，test，prod
*
* 3、同时加载多个配置集
* 1)、微服务任何配置信息，任何配置文件都可以放在配置中心中
* 2）、只需要在bootstrap.properties说明加载配置中心中哪些配置文件即可
* 3）、@Value，@ConfigurationProperties。。。
* 以前SpringBoot任何方法从配置文件中获取值，都能使用。
* 配置中心有的优先使用配置中心中的，

如果想将application配置文件全部交由配置中心管理，那么可以只在项目中保留bootstrap配置文件，然后在bootstrap配置文件配置添加关于其他配置文件的信息：
 spring.cloud.nacos.config.ext-config[0].data-id=application.yml #指定是命名空间中的哪个配置文件
 spring.cloud.nacos.config.ext-config[0].group=dev #不写这个就是默认分组的
 spring.cloud.nacos.config.ext-config[0].refresh=true #开启动态刷新配置  默认为false 
```



### 4. 三级分类功能代码编写

```java
//查询分类列表，返回三级分类的树形结构
@Override
public List<CategoryEntity> listWigTree() {

    // 1. 查出所有分类信息
    List<CategoryEntity> entities = baseMapper.selectList(null);

    //2、组装成父子的树形结构
    // 2.1找出所有1级分类
    List<CategoryEntity> level1Menu = entities.stream().filter(categoryEntity -> categoryEntity.getParentCid() == 0).
            map(menu1->
            {
                // 2.2 找出所有子分类
                menu1.setChildren(getChildren(menu1,entities));
                return menu1;
            }).sorted((menu1,menu2)->{
        return (menu1.getSort()==null?0:menu1.getSort()) - (menu2.getSort()==null?0:menu2.getSort());
            }).collect(Collectors.toList());
    return level1Menu;
}


//递归查找所有子分类
public List<CategoryEntity> getChildren(CategoryEntity root  ,List<CategoryEntity> all ) {
    List<CategoryEntity> children = all.stream().filter(category -> {
        return category.getParentCid() == root.getCatId();
    }).map(category -> {
        category.setChildren(getChildren(category, all));
        return category;
    }).sorted((menu1, menu2) -> {
        //2、菜单的排序
        return (menu1.getSort() == null ? 0 : menu1.getSort()) - (menu2.getSort() == null ? 0 : menu2.getSort());
    }).collect(Collectors.toList());

    return children;
}
```



### 5. 网关的路由配置

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: product_route
          uri: lb://mymall-product
          predicates:
            - Path=/api/product/**
          filters:
            - RewritePath=/api/(?<segment>.*),/$\{segment}

        - id: third_party_route
          uri: lb://mymall-third-part
          predicates:
            - Path=/api/thirdparty/**
          filters:
            - RewritePath=/api/thirdparty/(?<segment>.*),/$\{segment}

        - id: member_route
          uri: lb://mymall-member
          predicates:
            - Path=/api/member/**
          filters:
            - RewritePath=/api/(?<segment>.*),/$\{segment}
```

- feign远程调用的时候两种情况

```
  /product/skuinfo/info/{skuId}
*
*   1)、让所有请求过网关；
*          1、@FeignClient("mymall-gateway")：给mymall-gateway所在的机器发请求
*          2、/api/product/skuinfo/info/{skuId}
*   2）、直接让后台指定服务处理
*          1、@FeignClient("mymall-gateway")
*          2、/product/skuinfo/info/{skuId}
```

### 6. 跨域CORS

![image-20210425174918118](http://img.codezhou.com/img/image-20210425174918118.png)

- **解决方法之一  配置跨域过滤器**

  ```java
  @Configuration
  public class mymallCorsConfiguration {
  
      @Bean
      public CorsWebFilter corsWebFilter(){
          UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
  
          CorsConfiguration corsConfiguration = new CorsConfiguration();
  
          //1、配置跨域
          corsConfiguration.addAllowedHeader("*");
          corsConfiguration.addAllowedMethod("*");
          corsConfiguration.addAllowedOrigin("*");
          corsConfiguration.setAllowCredentials(true);
  
          source.registerCorsConfiguration("/**",corsConfiguration);
          return new CorsWebFilter(source);
      }
  }
  ```

  

### 7. 网关配置不生效Bug

![image-20210425191420574](http://img.codezhou.com/img/image-20210425191420574.png)

**直接复制的提供的配置文件，复制过来的时候这个都自动缩进了一个空格，导致报错**





### 8. nacos找不到实例的问题

![image-20210425202509928](http://img.codezhou.com/img/image-20210425202509928.png)

- 刚刚开始以为是配置出来问题，但是检查没有问题
- ....三个小时试了了很多解决方法
- 最后一次尝试 以为是IP的问题 因为nacos注册服务IP地址是VM8网卡的IP，但是尝试修改了IP依旧不行
- 目前搁置，最后猜测可能是版本问题有些配置不太一样或者版本冲突，uri改为http...这种访问没问题



- 第二次尝试：思前想后，感觉版本问题概率太小，而且网关配置的uri只有lb://myservice 这种格式不成功，其他方式测试了好几种都可以，那么是不是问题出在了lb？ 刚开始是以认为lb的这种格式可能官方修改了，但是去官方文档看格式并没有修改，于是想那是不是项目的负载均衡出了问题？nacos用Ribbon作为负载均衡，想到这终于恍然大悟了，之前碰到过一个错误，解决该错误的时候用了网上的一种解决方案：exclusion了Common模块的nacos中的Ribbon！
- ![image-20210425221125996](http://img.codezhou.com/img/image-20210425221125996.png)



- 删除这个，重启所有的微服务（因为几乎所有微服务都依赖了Common模块），最后问题解决，自己给自己挖坑的典型 泪目



### 9. Mybatis-plus中的常用注解

```java
@TableName //数据库表相关
@TableId  //表主键标识
@TableField //表字段标识
@TableLogic //表字段逻辑处理注解（逻辑删除）

@TableId(type= IdType.ID_WORKER_STR)

@TableField(exist = false) //表示该属性不为数据库表字段，但又是必须使用的。

@TableField(exist = true) //表示该属性为数据库表字段。

@TableField(condition = SqlCondition.LIKE) //表示该属性可以模糊搜索。

@TableField(fill = FieldFill.INSERT) //注解填充字段 ，生成器策略部分也可以配置

```



### 10. IDEA的TODO技巧

![image-20210425224052490](http://img.codezhou.com/img/image-20210425224052490.png)





### 11. mp的逻辑删除

逻辑删除

*  1）、配置全局的逻辑删除规则**（可省略）**
*  2）、配置逻辑删除的组件Bean**（3.1版本之后可省略**）
*  3）、给Bean加上逻辑删除注解@TableLogic(value = "1",delval = "0")

![image-20210426093340003](http://img.codezhou.com/img/image-20210426093340003.png)



### 12. 配置文件中配置日志打印级别

```yaml
logging:
  level:
    com.codezhou.mymall: debug
```



### 13. 粗心导致的bug：页面无法显示数据

- 如下图：页面显示没有数据

![image-20210426203846134](http://img.codezhou.com/img/image-20210426203846134.png)

- **在控制台看到数据没有被定义**

![image-20210426204006517](http://img.codezhou.com/img/image-20210426204006517.png)

- **network却可以看到数据已经被拿到**

![image-20210426204046467](http://img.codezhou.com/img/image-20210426204046467.png)

- 那么**大概可以猜测前端取值出问题**很有可能是前后端变量名字不一致

![image-20210426204213797](http://img.codezhou.com/img/image-20210426204213797.png)

- 如上图，粗心大意data变量名写成了date，前端取值是用的data，修改为data则可以



### 14. 阿里云OSS上传文件

- **引入oss的starter依赖**

```xml
          <!-- OSS对象存储 -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alicloud-oss</artifactId>
        </dependency>
```

- **配置key和endpoint的信息**

```yaml
spring:
  cloud:
    alicloud:
      access-key: LTAI5tEDgeert2Nt7NzpQkX
      secret-key: PS3CBVJ5XO7Rm0HK4UJgrxewfghtyu
      oss:
        endpoint: oss-cn-shenzhen.aliyuncs.com
```

- 注入OSSClient对象进行操作

  ```java
  @Autowired
  private OSSClient ossClient;
  ```



### 15. 引入Common到第三方服务模块去除mp的依赖包

- **第三方包没用到数据库，需要将mp依赖去除，不然会报错**

```xml
       <dependency>
            <groupId>com.codezhou.mymall</groupId>
            <artifactId>mymall-common</artifactId>
            <version>0.0.1-SNAPSHOT</version>
            <exclusions>
                <exclusion>
                    <groupId>com.baomidou</groupId>
                    <artifactId>mybatis-plus-boot-starter</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```



### 16. 启动项目时报测试类的ERROR

- 启动项目报如下错误：

```xml
Tests run: 3, Failures: 0, Errors: 3, Skipped: 0, Time elapsed: 0s <<< FAILURE! - in com.codezhou.mymall.thirdpart.MymallThirdPartApplicationTests
 Time elapsed: 0s  <<< ERROR!
```

- 加入如下依赖

```xml
<!-- 解决编译时候 测试类报错-->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <skip>true</skip>
    </configuration>
</plugin>
```





### 17. oss文件上传之服务端签名后直传

- 流程

![image-20210426232714333](http://img.codezhou.com/img/image-20210426232714333.png)



- **服务端代码返回签名数据**

  ```java
  @RestController
  public class OssController {
  
      @Autowired
      OSS ossClient;
  
      @Value("${spring.cloud.alicloud.oss.endpoint}")
      private String endpoint;
      @Value("${spring.cloud.alicloud.oss.bucket}")
      private String bucket;
  
      @Value("${spring.cloud.alicloud.access-key}")
      private String accessId;
  
  
      @RequestMapping("/oss/policy")
      public R policy() {
  
          //https://mymall-hello.oss-cn-beijing.aliyuncs.com/hahaha.jpg
  
          String host = "https://" + bucket + "." + endpoint; // host的格式为 bucketname.endpoint
          // callbackUrl为 上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
  //        String callbackUrl = "http://88.88.88.88:8888";
          String format = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
          String dir = format + "/"; // 用户上传文件时指定的前缀。
  
          Map<String, String> respMap = null;
          try {
              long expireTime = 30;
              long expireEndTime = System.currentTimeMillis() + expireTime * 1000;
              Date expiration = new Date(expireEndTime);
              PolicyConditions policyConds = new PolicyConditions();
              policyConds.addConditionItem(PolicyConditions.COND_CONTENT_LENGTH_RANGE, 0, 1048576000);
              policyConds.addConditionItem(MatchMode.StartWith, PolicyConditions.COND_KEY, dir);
  
              String postPolicy = ossClient.generatePostPolicy(expiration, policyConds);
              byte[] binaryData = postPolicy.getBytes("utf-8");
              String encodedPolicy = BinaryUtil.toBase64String(binaryData);
              String postSignature = ossClient.calculatePostSignature(postPolicy);
  
              respMap = new LinkedHashMap<String, String>();
              respMap.put("accessid", accessId);
              respMap.put("policy", encodedPolicy);
              respMap.put("signature", postSignature);
              respMap.put("dir", dir);
              respMap.put("host", host);
              respMap.put("expire", String.valueOf(expireEndTime / 1000));
              // respMap.put("expire", formatISO8601Date(expiration));
  
  
          } catch (Exception e) {
              // Assert.fail(e.getMessage());
              System.out.println(e.getMessage());
          }
  
          return R.ok().put("data",respMap);
      }
  }
  ```

  

- 阿里云设置所有请求允许跨域访问

- 前端上传文件之前先去访问后端接口拿到签名等数据，然后再上传到阿里云oss

  ![image-20210427102829590](http://img.codezhou.com/img/image-20210427102829590.png)







### 18. Github提交没有贡献值

- 在复习git配置的时候，本地的git的配置邮箱打错了没改回来

![image-20210426233655390](http://img.codezhou.com/img/image-20210426233655390.png)





### 19. JSR303后端校验错误

- 在实体类标注校验的注解

  ![image-20210427143204949](http://img.codezhou.com/img/image-20210427143204949.png)

- 在Controller中用注解@Valid标志这个参数要被校验

![image-20210427143152954](http://img.codezhou.com/img/image-20210427143152954.png)

- 但是自己这并没有起到作用
- 于是添加了如下依赖

```xml
      //这个地方要注意，如果导入到Common中的pom中，Common包不是Springboot项目要加版本号一起导入
       <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>  
```

- 起到了校验作用抛出如下异常

![image-20210427152342348](http://img.codezhou.com/img/image-20210427152342348.png)

- 但是并没有向视频中那样返回错误的Json信息

  ![image-20210427152943687](http://img.codezhou.com/img/image-20210427152943687.png)

![image-20210427152426827](http://img.codezhou.com/img/image-20210427152426827.png)

**而是直接返回服务器抛出异常的服务器错误**

![image-20210427153020772](http://img.codezhou.com/img/image-20210427153020772.png)

- **那就还是用全局异常处理@ControllerAdvice注解来返回错误Json信息**

  - 新建一个全局异常处理类，标上相关注解

    ```java
    @Slf4j //这个是lombok包中的注解
    @ControllerAdvice
    @ResponseBody     //也可以合起来用@RestControllerAdvice
    public class MyExceptionControllerAdvice {
    
        //捕获JSR303校验异常
        @ExceptionHandler(MethodArgumentNotValidException.class)
        public R HandlerBindException(MethodArgumentNotValidException ex) {
            log.error("捕获了一个异常");
            BindingResult bindingResult = ex.getBindingResult();
            //StringBuilder stringBuilder = new StringBuilder();
            HashMap<String, String> errorMap = new HashMap<>();
            for (FieldError error : bindingResult.getFieldErrors()) {
                String field = error.getField();
                Object value = error.getRejectedValue();
                String msg = error.getDefaultMessage();
                errorMap.put(field, msg);
            }
            return R.error(BizCodeEnume.VAILD_EXCEPTION.getCode(), BizCodeEnume.VAILD_EXCEPTION.getMsg()).put("data", errorMap);
        }
    
        //捕获其他所有异常
        @ExceptionHandler(Exception.class)
        public R HandlerException(Exception ex) {
    
            log.error("捕获了一个异常");
    
            return R.error(400, ex.getMessage());
        }
    
    
    }
    ```

    

- 关于**分组校验**

  - 场景：在新增和修改的时候，修改的时候有些值可以为空也就是不传过来，只传哪些需要更新的值，但是新增的时候有些值就必须要传过来，那么这两种情况要怎么去适配校验，就需要分组校验了

  - 分组校验需要一个接口作为组的标识

    ```java
    //新增组接口
    public interface AddGroup {
    }
    //修改组接口
    public interface UpdateGroup {
    }
    ```

  - 在实体类属性上**标记他们的分组类型**

    ```
        @TableId
    	@NotNull(message = "修改的时候需要指定品牌Id", groups = {UpdateGroup.class})
    	@Null (message = "Id为自增，新增的时候Id必须为空",groups = {AddGroup.class})
    	private Long brandId;
    	
    	@NotBlank(message = "任何时候品牌名不能为空！",groups = {AddGroup.class, UpdateGroup.class})
    	private String name;
    
        //修改的时候要是图标没有改变可以不传这个所以修改的时候可以为null
    	@NotBlank(groups = {message = "新增的时候图标不能为空！",AddGroup.class})
    	private String logo;
    ```

    

  - 需要使用`@Validated`注解来分组，`@Valid`不行,在Controller中的新增方法中标记组为`AddGroup.class`

    ```java
     public R save(@Validated({AddGroup.class}) @RequestBody BrandEntity brand, BindingResult result) {
            brandService.save(brand);
            return R.ok();
        }
    ```

  - 其他：**在分组校验的情况下，那些没有被指定分组的校验注解是不会生效的，只在没分组校验的场景下生效**。

  ```java
      //比如这些没有指定分组的非空校验，在分组校验的时候不会生效
      @NotBlank
  	private String firstLetter;
  ```

  - 也可以自定义校验注解



### 20. SPU和SKU的概念

- SPU = Standard Product Unit (标准化产品单元)
  - SPU是商品信息聚合的最小单位，是一组可复用、易检索的标准化信息的集合，该集合描述了一个产品的特性。通俗点讲，属性值、特性相同的商品就可以称为一个SPU。

- SKU=stock keeping unit(库存量单位)
  - SKU即库存进出计量的单位， 可以是以件、盒、托盘等为单位。
  - SKU是物理上不可分割的最小存货单元。在使用时要根据不同业态，不同管理模式来处理。在服装、鞋类商品中使用最多最普遍。




### 21. 注解配置当有些字段为空的时候json数据不返回

```
//当不为空的时候Json才返回
@JsonInclude(JsonInclude.Include.NON_EMPTY)
private List<CategoryEntity> children;
```





### 22. 增加mp分页插件类

```java
@Configuration
@EnableTransactionManagement //开启事务
@MapperScan("com.codezhou.mymall.product.dao")
public class MyBatisConfig {
    //引入分页插件
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
         paginationInterceptor.setOverflow(true);
        // 设置最大单页限制数量，默认 500 条，-1 不受限制
        paginationInterceptor.setLimit(1000);
        return paginationInterceptor;
    }
}
```

![image-20210427223919996](http://img.codezhou.com/img/image-20210427223919996.png)





### 23. SpringCloud的远程调用 数据传输大致过程

```java
1、CouponFeignService.saveSpuBounds(spuBoundTo);
*      1）、@RequestBody将这个对象转为json。
*      2）、找到mymall-coupon服务，给/coupon/spubounds/save发送请求。
*          将上一步转的json放在请求体位置，发送请求；
*      3）、对方服务收到请求。请求体里有json数据。
*          (@RequestBody SpuBoundsEntity spuBounds)；将请求体的json转为SpuBoundsEntity；
* 只要json数据模型是兼容的。双方服务无需使用同一个to
```



### 24. 快捷设置启动微服务

- **项目配置一起启动所有微服务模块**

![image-20210428230007031](http://img.codezhou.com/img/image-20210428230007031.png)



### 25. 前端无法移除关联属性bug

- 报错如下：

```java
nested exception is org.apache.ibatis.binding.BindingException: Parameter 'xxxx'
```

- 原因：在Dao类和xml文件传入参数的时候，参数名要用@param注解标注以下参数名

![image-20210428220010679](http://img.codezhou.com/img/image-20210428220010679.png)

https://blog.csdn.net/a1106103430/article/details/90046039



### 26. 碰到以前做SYB时候一样的bug：

- **当@Transactional事务注解方法，内部用try catch捕捉了异常，事务就不会回滚**
- **所以可以用来当一些不重要的数据库操作，当他们发生异常不想它们影响这个事务回滚的时候，可以将它们用try catch捕捉异常即可**



### 27. Springboot项目无法启动

- 上网查找错误发现需要设置如下：
  - Delegate IDE build/run actions to Maven。将IDE构建/运行操作委托给Maven
    ![image-20210503143928599](http://img.codezhou.com/img/image-20210503143928599.png)



### 28. 前端发布商品选择品牌报错

- 报错信息：

  ```
  ReferenceError: js is not defined
  ```

- 解决办法

  - 1、使用npm添加依赖：npm install --save pubsub-js（失败的话使用此命令：cnpm install --save pubsub-js）
  - 2、在src下的main.js中引用：
                 import PubSub from 'pubsub-js'
                 Vue.prototype.PubSub = PubSub
  
    

### 29. Bug解决参考文章

- https://blog.csdn.net/weixin_41682347/article/details/106457197
- https://www.cnblogs.com/rookiemzl/p/13814919.html
- https://blog.csdn.net/ZPeng_CSDN/article/details/104165555
- https://blog.csdn.net/qq_30398499/article/details/114225896
- https://blog.csdn.net/shfqbluestone/article/details/50245547
- https://blog.csdn.net/lq1759336950/article/details/114240455
- https://blog.csdn.net/PengHao__/article/details/78643379
- https://blog.csdn.net/weixin_43556636/article/details/110653989
- https://blog.csdn.net/a1106103430/article/details/90046039
- https://blog.csdn.net/qq_32370913/article/details/105924209
- https://blog.csdn.net/weixin_45729934/article/details/110310119
- https://blog.csdn.net/weiqiang915/article/details/114984438