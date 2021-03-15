## 反射中的getDeclaredFields和getDeclaredMethods方法

### 前言
   后端保存多个图片路径到数据库，要获取储存图片的对象的图片字段一个个保存调用对应的Set方法，以及需要根据前端传来的特定的字符串改变相应图片字段的图片路径，需要通过反射获取对象的图片字段和Set方法。

### Ⅰ. getDeclaredXxxx和getXxxx的区别
- getFields()与getDeclaredFields()区别：
> getFields()只能访问类中声明为公有的字段,私有的字段它无法访问.
> getDeclaredFields()能访问类中所有的字段,与public,private,protect无关  

 - getMethods()与getDeclaredMethods()区别：
 > getMethods()只能访问类中声明为公有的方法,私有的方法它无法访问,能访问从其它类继承来的公有方法.
 > getDeclaredFields()能访问类中所有的字段,与public,private,protect无关,不能访问从其它类继承来的方法  

### Ⅱ. getDeclaredFields的使用
```java
  
//拿到类的所有属性字段
  Field[] fields = one75.getClass().getDeclaredFields();
  //遍历属性字段，拿到image相关的属性
  for (Field field : fields) {
      field.setAccessible(true);
      String name = field.getName();
      if (name.substring(0, 3).equals("ima")) {
          String imageUrl = (String)field.get(one75);
          if (StringUtils.isBlank(imageUrl)) {
              field.set(one75,url);
              break;
          }
      }

```
- 拿到所有属性字段遍历之后，因为字段都是Private要访问每个属性需要先调用setAccessible(true)方法才能访问。
- getName方法得到属性的名称用来判断该属性是否为图片字段。
- 用field.get(one75)方法拿到该字段值判断其是否为空
- 接着往字段里面设置值调用field.set(one75,url)方法，方法参数:第一个为该对象实例，第二个为要设置的值。


### Ⅲ. getDeclaredMethods方法的使用
```java
 
String[] split = key.split("=");
 String methodName = "set" + split[0];
 Method me = Townone21.class.getDeclaredMethods(methodName, String.class);
 Townone21 townone21 = new Townone21();
 townone21.setuId(id);
 me.invoke(townone21, key);
 townone21Service.updateTownone21dev(townone21);
 if (split.length >= 2) {
     String replace = split[1].replace(text, profile);
     FileUploadUtils.delAllFile(replace);
 }

```
- 根据methodName得到类的Set方法
- me.invoke(townone21, key)调用该方法，方法参数：第一个为该对象实例，第二个为调用Set方法的方法参数
- 也可以用getDeclaredFields得到字段来Set值，但是感觉那样麻烦一点