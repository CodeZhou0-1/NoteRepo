## 关于Java的File.separator


### file.separator的作用
- file.separator这个代表系统目录中的间隔符，说白了就是斜线，不过有时候需要双线，有时候是单线，你用这个静态变量就解决兼容问题了。

- 在Windows下的路径分隔符和Linux下的路径分隔符是不一样的，当直接使用绝对路径时，跨平台会暴出“No such file or diretory”的异常。

> 比如说要在temp目录下建立一个test.txt文件，在Windows下应该这么写：
> File file1 = new File ("C:\tmp\test.txt");
> 在linux下则是这样的：
> File file2 = new File ("/tmp/test.txt");


> 如果要考虑跨平台，则最好是这么写：
> File myFile = new File("C:" + File.separator + "tmp" + File.separator, "test.txt");

### 其他类似separator的静态字段

- separatorChar

```java
public static final char separatorChar
```


与系统有关的**默认名称分隔符**。此字段被初始化为包含系统属性 file.separator 值的第一个字符。在 UNIX 系统上，此字段的值为 '/'；在 Microsoft Windows 系统上，它为 '\'。

- separator

```java
public static final String separator
```


与系统有关的**默认名称分隔符**，为了方便，它被表示为一个字符串。此字符串只包含一个字符，即 separatorChar。

- pathSeparatorChar

```java
public static final char pathSeparatorChar
```


与系统有关的**路径分隔符**。此字段被初始为包含系统属性 path.separator 值的第一个字符。此字符用于分隔以路径列表 形式给定的文件序列中的文件名。在 UNIX 系统上，此字段为 ':'；在 Microsoft Windows 系统上，它为 ';'。

- pathSeparator

```java
public static final String pathSeparator
```


与系统有关的**路径分隔符**，为了方便，它被表示为一个字符串。此字符串只包含一个字符，即 pathSeparatorChar。


### 转自以下文章
https://www.cnblogs.com/zhxn/p/7206786.html