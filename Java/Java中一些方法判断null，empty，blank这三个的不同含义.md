# Java中一些方法判断null，empty，blank这三个的不同含义

## 一 . Null的含义

> - null指**对象没有初始化，也就是说没有引用指向这个对象**
> - null通常是我们在进行数据库的查询操作时，查询结果首先用object != null，进行非空判断，然后再进行其他的业务逻辑，这样可以避免出现空指针异常。


## 二 . isEmpty()
> isEmpty() 此方法可以使用于字符串，数组，集合。
> 首先看一下源码：
```java
public boolean isEmpty() {
       return value.length == 0; 
}
```

> - 这里是根据对象的长度进行判断，使用这个方法时，先要排除对象不为null，当对象为null时，调用isEmpty方法就会报空指针异常。
> - 据源码可知要想要此方法返回true，也就是一个当对象的长度为0，如字符串，数组等，就会返回true。
> - **需要注意的是当字符串对象内容为空格的时候，字符串的长度是不为0的，所以当判断一些字符串需要有数据的时候空格应该也要被否认，下面有一种方法（isNotBlank）就是用来处理这种情形的。**


## 三 . StringUtils：isNotEmpty、isNotBlank
> StringUtils是一个经常用来处理字符串的一个工具类，它需要导入一下依赖坐标来使用 
```java
<dependency>
<groupId>commons-lang</groupId>
<artifactId>commons-lang</artifactId>
<version>2.6</version>
</dependency>
```



###  isNotEmpty: 判断字符串不为空串

```java
* StringUtils.isNotEmpty(null) = false
* StringUtils.isNotEmpty("") = false
* StringUtils.isNotEmpty(" ") = true 多个空格也不是空串
* StringUtils.isNotEmpty("bob") = true
* StringUtils.isNotEmpty(" bob ") = true
```




### isNotBlank: 判断字符串内容不为空串且不为空白串
```java
* StringUtils.isNotBlank(null) = false
* StringUtils.isNotBlank("") = false
* StringUtils.isNotBlank(" ") = false 即使有多个空格，也属于空白串
* StringUtils.isNotBlank("bob") = true
* StringUtils.isNotBlank(" bob ") = true
```