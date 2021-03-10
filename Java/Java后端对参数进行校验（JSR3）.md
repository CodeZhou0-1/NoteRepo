## Java后端对参数进行校验（JSR3）

### 一. 前言：后端参数校验的必要性

> **即使在前端对数据进行校验的情况下，我们还是要对传入后端的数据再进行一遍校验，避免用户绕过浏览器直接通过一些 HTTP 工具(比如postman)直接向后端请求一些违法的数据。**



### 二. 参数校验的技术实现：JSR

#### 2.0 JSR的介绍

> - JSR(Java Specification Requests） 是一套 JavaBean 参数校验的标准，**其中定义了很多有关参数校验的常用注解，我们可以直接将这些注解加在我们 JavaBean 的属性上面**，这样就可以在需要校验的时候进行校验了。
> - SpringBoot 项目的 spring-boot-starter-web 依赖中已经有相关依赖包，不需要再引用。



#### 2.1 一些常用的JSR参数检验注解

> - @NotEmpty 被注释的字符串的不能为 null 也不能为空
>
> - @NotBlank 被注释的字符串非 null，并且必须包含一个非空白字符
>
> - @Null 被注释的元素必须为 null
> - @NotNull 被注释的元素必须不为 null
>
> - @AssertTrue 被注释的元素必须为 true
>
> - @AssertFalse 被注释的元素必须为 false
>
> - @Pattern(regex=,flag=)被注释的元素必须符合指定的正则表达式
>
> - @Email 被注释的元素必须是 Email 格式。
>
> -  @Min(value)被注释的元素必须是一个数字，其值必须大于等于指定的最小值
>
> - @Max(value)被注释的元素必须是一个数字，其值必须小于等于指定的最大值
>
> - @DecimalMin(value)被注释的元素必须是一个数字，其值必须大于等于指定的最小值
>
> - @DecimalMax(value) 被注释的元素必须是一个数字，其值必须小于等于指定的最大值
>
> - @Size(max=, min=)被注释的元素的大小必须在指定的范围内
>
> - @Digits (integer, fraction)被注释的元素必须是一个数字，其值必须在可接受的范围内
>
> - @Past被注释的元素必须是一个过去的日期• @Future 被注释的元素必须是一个将来的日期
>
> • ......

#### 2.2 在实体类上添加以上校验的注解

```java
@Data
public class User {

@NotNull(message = "Id不能为空")
private String Id;

@Size(max = 33)
@NotNull(message = "name不能为空")
private String name;

@Pattern(regexp = "((^Man$|^Woman$|^UGM$))", message = "sex值不在可选范围")
@NotNull(message = "sex不能为空")
private String sex;

@Email(message = "email格式不正确")
@NotNull(message = "email不能为空")
private String email;

}
```

> **属性message：当不满足校验条件的时候，返回的错误提示信息内容。**

#### 2.3 在需要校验的JavaBean对象旁加上@Valid注解

```java
@RestController
@RequestMapping("/user")
public class UserController {
@PostMapping("/returnUser")
public ResponseEntity<User> getUser(@RequestBody @Valid User user) {
return ResponseEntity.ok().body(user);
}
}
```

#### 2.4 直接检验方法中的非JavaBean参数(PathVariables 和 RequestParameters)

> - **要记得在类上加上 Validated 注解了，这个参数可以告诉 Spring 去校验方法参数。**

```java
@RestController
@RequestMapping("/user")
@Validated
public class UserController {

@GetMapping("/returnUser/{id}")
public ResponseEntity<Integer> getUserByID(@Valid @PathVariable("id") @Max(value = 5,message = "超过 id 的范围了") Integer id) {
return ResponseEntity.ok().body(id);
}
}
```

#### 2.5 如何全局处理校验失败时的异常以及返回错误提示信息

> - 跟全局异常处理的相关注解：
>
>   @ControllerAdvice :注解定义全局异常处理类
>
>   @ExceptionHandler :注解声明异常处理方法
>
>   **如果校验不通过即方法参数不对的情况下，就会抛出MethodArgumentNotValidException，我们适当的处理好这个异常即可返回给前端错误提示信息**

##### 2.5.1 处理此异常的示例代码：

```java
@ControllerAdvice
@ResponseBody
public class GlobalExceptionHandler {

/**
* 请求参数异常处理
*/
@ExceptionHandler(MethodArgumentNotValidException.class)
public Reslut handleMethodArgumentNotValidException(MethodArgumentNotValidException ex) {
         
        BindingResult bindingResult = ex.getBindingResult();
        StringBuilder stringBuilder = new StringBuilder();
        for (FieldError error : bindingResult.getFieldErrors()) {
            String field = error.getField();
            Object value = error.getRejectedValue();
            String msg = error.getDefaultMessage();
            String message = String.format("错误字段：%s，错误值：%s，原因：%s；", field, value, msg);
            stringBuilder.append(message).append("\r\n");
        }
        return Result.error(MsgDefinition.ILLEGAL_ARGUMENTS.codeOf(), stringBuilder.toString());
    }
}
}
```

> - **在异常处理代码中拿到错误提示信息，并设置在返回结果集中返回到前端页面。**返回结果及Result可根据自身情况定义。

#### 2.6 @Validated和@Valid的区别

> 1. `@Valid`：标准JSR-303规范的标记型注解，用来标记验证属性和方法返回值，进行级联和递归校验
> 2. `@Validated`：`Spring`的注解，是标准`JSR-303`的一个变种（补充），提供了一个分组功能，可以在入参验证时，根据不同的分组采用不同的验证机制
> 3. 在`Controller`中校验方法参数时，使用@Valid和@Validated并无特殊差异（若不需要分组校验的话）
> 4. `@Validated`注解可以用于类级别，用于支持Spring进行方法级别的参数校验。`@Valid`可以用在属性级别约束，用来表示**级联校验**。
> 5. `@Validated`只能用在类、方法和参数上，而`@Valid`可用于方法、**字段、构造器**和参数上