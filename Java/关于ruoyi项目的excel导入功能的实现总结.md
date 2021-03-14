# 关于ruoyi项目的excel导入功能的实现总结 

### 1. 添加如下代码

 ![image.png](http://codezhou.com/upload/2020/12/image-2aaa9b9169bd4c7f9a1582dd8486a6a5.png)
### 2. 发现++userList++中添加进去的数据为null的Bug
 > - 在测试一下代码的时候发现代码中返回的userList为的中的数据都是null
```java
        InputStream is = new FileInputStream(new File("D:\\test.xlsx"));
        ExcelUtil<ImportTest> util = new ExcelUtil<ImportTest>(ImportTest.class);
        List<ImportTest> userList = util.importExcel("import_test",is);
```
### 3. 调试 Bug
![image.png](http://codezhou.com/upload/2020/12/image-a429bbdd1d594bf1a1b53787ade576ad.png)

> - 3.1 Debug方法importExcel
> ![image.png](http://codezhou.com/upload/2020/12/image-95cfce0cece646d9a127a05f08f46c1d.png)
> - 3.2 发现此方法前面的代码都正常执行，没有问题，最后在将从excel中取到的值保存到单个对象的时候没有保存成功。
> ![image.png](http://codezhou.com/upload/2020/12/image-27fac5d42f754bb5aac4f51165f9d699.png)
> - 3.3 找到出错点之后就开始Debug方法invokeSetter
> ![image.png](http://codezhou.com/upload/2020/12/image-b26cab6dbbcf48038250b4e1136791f1.png)
> - 3.4 最后修改set方法名后即成功保存数据到userlist中


 ### 4. 更改 set get 方法名之后 显示页面出 Bug
> - 更改set和get方法名之后，显示页面无法正常显示数据。第一反应应该是修改了set和get方法名导致的，应该是xml文件中的sql语句的实现用的是正常的set和get方法名称。于是再自动生成一遍set和get方法
 ### 5. 其他
> - 后端添加代码中，原先生成的service类里面没有importUser方法，需要再把那个方法中加入service里面
> - 还有个是否更新覆盖已经存在的数据的按钮，测试下行不行得通