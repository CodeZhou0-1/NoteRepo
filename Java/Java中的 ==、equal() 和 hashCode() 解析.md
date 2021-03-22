# Java中的 `==`、`equal()` 和 `hashCode()` 解析

### Ⅰ. `==`和`equal()`的区别

- `==`：判断的是两个对象是否是同一个对象，即实际比较他们的内存地址是否指向同一个对象（**基本类型比较的是值是否相等，引用数据类型比较的是内存地址**）

- `equal()`：方法存在于Object类中，所有类都可以重写这个方法，如果不重写实际效果比较的也是两个对象是否是同一个对象，与`==`效果一样

`equal()`方法在Object中的源码:

```java
public boolean equals(Object obj) {
     return (this == obj);
}
```
- 所以当我们想比较两个实例对象的内容相同（或者部分内容相同）即返回true而不是比较内存地址是否一样的时候，我们则可以重写 `equals` 方法

- `String` 类就重写了 `equals` 方法，使得当字符串内容相同的时候就返回true
```java
public boolean equals(Object anObject) {
   // 先直接判断是否是同一个实例对象
    if (this == anObject) {
        return true;
    }
   // 再判断比较的对象是否是String类,都相同再开始比较字符串内容
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
       // 判断字符串长度是否相同
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```
**那么当我们重写 `equals()` 为什么总是伴随着重写 `hashCode()` 方法呢?**

### Ⅱ.  `equals()` 和 `hashCode()` 的联系
- `hashCode()` 方法返回的是一个int类型的哈希码，也称为散列码，哈希码的作用是确定该对象在哈希表中的索引位置，方便快速查找该对象

- `hashCode()` 方法也在Object中，当我们需要将某个类放到散列表(**HashSet, Hashtable, HashMap**等等)中的时候，那么此时哈希码就起到了作用

- 在往散列表中添加数据的时候：
  - 第一步：先会获取该对象的哈希码和散列表中已存在对象的哈希码进行比较，**如果哈希码不相同则认为数据不重复直接添加**，如果相同则执行第二步
  - 第二步：如果哈希码相同那么才会调用`equals()`方法来比较两个对象是否相同
  - 注意：**两个对象哈希码不相同代表两个对象一定不是同一个对象或者值不相同，两个对象哈希码相同却不能判断他们两个对象或者值相同**

- 所以由上面我们可以知道：当我们只重写了`equals()`方法的时候，每次 `hashCode()` 方法都会通过对象的内存地址 来计算哈希码导致只要对象内存地址不一样都判断为true，那么就不会再调用`equals()`方法来进行判断


### Ⅲ. 结论
  综上，当我们要将类放入散列表中的时候，如果想通过比较类的内容或者部分内容来判断两个对象是否相同的时候，不仅要重写`equals()` 方法也要重写 `hashCode()`方法

### Ⅳ. 参考文章
[Java hashCode() 和 equals()的若干问题解答](https://www.cnblogs.com/skywang12345/p/3324958.html)# Java中的 `==`、`equal()` 和 `hashCode()` 解析

### Ⅰ. `==`和`equal()`的区别

- `==`：判断的是两个对象是否是同一个对象，即实际比较他们的内存地址是否指向同一个对象（**基本类型比较的是值是否相等，引用数据类型比较的是内存地址**）

- `equal()`：方法存在于Object类中，所有类都可以重写这个方法，如果不重写实际效果比较的也是两个对象是否是同一个对象，与`==`效果一样

`equal()`方法在Object中的源码:

```java
public boolean equals(Object obj) {
     return (this == obj);
}
```
- 所以当我们想比较两个实例对象的内容相同（或者部分内容相同）即返回true而不是比较内存地址是否一样的时候，我们则可以重写 `equals` 方法

- `String` 类就重写了 `equals` 方法，使得当字符串内容相同的时候就返回true
```java
public boolean equals(Object anObject) {
   // 先直接判断是否是同一个实例对象
    if (this == anObject) {
        return true;
    }
   // 再判断比较的对象是否是String类,都相同再开始比较字符串内容
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
       // 判断字符串长度是否相同
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```
**那么当我们重写 `equals()` 为什么总是伴随着重写 `hashCode()` 方法呢?**

### Ⅱ.  `equals()` 和 `hashCode()` 的联系
- `hashCode()` 方法返回的是一个int类型的哈希码，也称为散列码，哈希码的作用是确定该对象在哈希表中的索引位置，方便快速查找该对象

- `hashCode()` 方法也在Object中，当我们需要将某个类放到散列表(**HashSet, Hashtable, HashMap**等等)中的时候，那么此时哈希码就起到了作用

- 在往散列表中添加数据的时候：
  - 第一步：先会获取该对象的哈希码和散列表中已存在对象的哈希码进行比较，**如果哈希码不相同则认为数据不重复直接添加**，如果相同则执行第二步
  - 第二步：如果哈希码相同那么才会调用`equals()`方法来比较两个对象是否相同
  - 注意：**两个对象哈希码不相同代表两个对象一定不是同一个对象或者值不相同，两个对象哈希码相同却不能判断他们两个对象或者值相同**

- 所以由上面我们可以知道：当我们只重写了`equals()`方法的时候，每次 `hashCode()` 方法都会通过对象的内存地址 来计算哈希码导致只要对象内存地址不一样都判断为true，那么就不会再调用`equals()`方法来进行判断


### Ⅲ. 结论
  综上，当我们要将类放入散列表中的时候，如果想通过比较类的内容或者部分内容来判断两个对象是否相同的时候，不仅要重写`equals()` 方法也要重写 `hashCode()`方法

### Ⅳ. 参考文章
[Java hashCode() 和 equals()的若干问题解答](https://www.cnblogs.com/skywang12345/p/3324958.html)# Java中的 `==`、`equal()` 和 `hashCode()` 解析

### Ⅰ. `==`和`equal()`的区别

- `==`：判断的是两个对象是否是同一个对象，即实际比较他们的内存地址是否指向同一个对象（**基本类型比较的是值是否相等，引用数据类型比较的是内存地址**）

- `equal()`：方法存在于Object类中，所有类都可以重写这个方法，如果不重写实际效果比较的也是两个对象是否是同一个对象，与`==`效果一样

`equal()`方法在Object中的源码:

```java
public boolean equals(Object obj) {
     return (this == obj);
}
```
- 所以当我们想比较两个实例对象的内容相同（或者部分内容相同）即返回true而不是比较内存地址是否一样的时候，我们则可以重写 `equals` 方法

- `String` 类就重写了 `equals` 方法，使得当字符串内容相同的时候就返回true
```java
public boolean equals(Object anObject) {
   // 先直接判断是否是同一个实例对象
    if (this == anObject) {
        return true;
    }
   // 再判断比较的对象是否是String类,都相同再开始比较字符串内容
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
       // 判断字符串长度是否相同
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```
**那么当我们重写 `equals()` 为什么总是伴随着重写 `hashCode()` 方法呢?**

### Ⅱ.  `equals()` 和 `hashCode()` 的联系
- `hashCode()` 方法返回的是一个int类型的哈希码，也称为散列码，哈希码的作用是确定该对象在哈希表中的索引位置，方便快速查找该对象

- `hashCode()` 方法也在Object中，当我们需要将某个类放到散列表(**HashSet, Hashtable, HashMap**等等)中的时候，那么此时哈希码就起到了作用

- 在往散列表中添加数据的时候：
  - 第一步：先会获取该对象的哈希码和散列表中已存在对象的哈希码进行比较，**如果哈希码不相同则认为数据不重复直接添加**，如果相同则执行第二步
  - 第二步：如果哈希码相同那么才会调用`equals()`方法来比较两个对象是否相同
  - 注意：**两个对象哈希码不相同代表两个对象一定不是同一个对象或者值不相同，两个对象哈希码相同却不能判断他们两个对象或者值相同**

- 所以由上面我们可以知道：当我们只重写了`equals()`方法的时候，每次 `hashCode()` 方法都会通过对象的内存地址 来计算哈希码导致只要对象内存地址不一样都判断为true，那么就不会再调用`equals()`方法来进行判断


### Ⅲ. 结论
  综上，当我们要将类放入散列表中的时候，如果想通过比较类的内容或者部分内容来判断两个对象是否相同的时候，不仅要重写`equals()` 方法也要重写 `hashCode()`方法

### Ⅳ. 参考文章
[Java hashCode() 和 equals()的若干问题解答](https://www.cnblogs.com/skywang12345/p/3324958.html)# Java中的 `==`、`equal()` 和 `hashCode()` 解析

### Ⅰ. `==`和`equal()`的区别

- `==`：判断的是两个对象是否是同一个对象，即实际比较他们的内存地址是否指向同一个对象（**基本类型比较的是值是否相等，引用数据类型比较的是内存地址**）

- `equal()`：方法存在于Object类中，所有类都可以重写这个方法，如果不重写实际效果比较的也是两个对象是否是同一个对象，与`==`效果一样

`equal()`方法在Object中的源码:

```java
public boolean equals(Object obj) {
     return (this == obj);
}
```
- 所以当我们想比较两个实例对象的内容相同（或者部分内容相同）即返回true而不是比较内存地址是否一样的时候，我们则可以重写 `equals` 方法

- `String` 类就重写了 `equals` 方法，使得当字符串内容相同的时候就返回true
```java
public boolean equals(Object anObject) {
   // 先直接判断是否是同一个实例对象
    if (this == anObject) {
        return true;
    }
   // 再判断比较的对象是否是String类,都相同再开始比较字符串内容
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
       // 判断字符串长度是否相同
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```
**那么当我们重写 `equals()` 为什么总是伴随着重写 `hashCode()` 方法呢?**

### Ⅱ.  `equals()` 和 `hashCode()` 的联系
- `hashCode()` 方法返回的是一个int类型的哈希码，也称为散列码，哈希码的作用是确定该对象在哈希表中的索引位置，方便快速查找该对象

- `hashCode()` 方法也在Object中，当我们需要将某个类放到散列表(**HashSet, Hashtable, HashMap**等等)中的时候，那么此时哈希码就起到了作用

- 在往散列表中添加数据的时候：
  - 第一步：先会获取该对象的哈希码和散列表中已存在对象的哈希码进行比较，**如果哈希码不相同则认为数据不重复直接添加**，如果相同则执行第二步
  - 第二步：如果哈希码相同那么才会调用`equals()`方法来比较两个对象是否相同
  - 注意：**两个对象哈希码不相同代表两个对象一定不是同一个对象或者值不相同，两个对象哈希码相同却不能判断他们两个对象或者值相同**

- 所以由上面我们可以知道：当我们只重写了`equals()`方法的时候，每次 `hashCode()` 方法都会通过对象的内存地址 来计算哈希码导致只要对象内存地址不一样都判断为true，那么就不会再调用`equals()`方法来进行判断


### Ⅲ. 结论
  综上，当我们要将类放入散列表中的时候，如果想通过比较类的内容或者部分内容来判断两个对象是否相同的时候，不仅要重写`equals()` 方法也要重写 `hashCode()`方法

### Ⅳ. 参考文章
[Java hashCode() 和 equals()的若干问题解答](https://www.cnblogs.com/skywang12345/p/3324958.html)