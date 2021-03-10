##   Integer与int类型的互相以及同类比较
```java
    public static void main(String[] args) {
        int i1 = 127;
        Integer i2 = new Integer(127);
        Integer i3 = 127;
        Integer i4 = 127;
        Integer i5 = new Integer(500);
        Integer i6 = new Integer(500);
        Integer i7 = 500;
        Integer i8 = 500;
        System.out.println(i1 == i2);//true  当与i1基本类型比较的时候i2自动拆箱为基本类型，相当于基本类型之间的比较
        System.out.println(i1 == i3);//true  同上
        System.out.println(i2 == i3);//false 两个不同对象之间的比较
        System.out.println(i3 == i4);//true  i3和i4范围在[-128~128)之间的时候且以直接赋值方式生成对象，那么指向同一个已经缓存好的Integer对象
        System.out.println(i5 == i6);//false 两个不同对象之间的比较
        System.out.println(i7 == i8);//false 范围不在[-128~128)之间，两个不同对象直接的比较


    }

```

### 由此代码块开始分析：
### 1.1 先分析Integer之间的比较：
> - ****++当在-128到127范围的值，且以i1 = 127这样直接赋值的方式生成的Interger对象，只要直接赋值的值相同那么返回的Integer对象是同一个（下面1.4原理解释了这个现象）++****，结果自然为true，
> - **++当不在-128到127范围，不管是new还是以i1 = 127这样直接赋值的方式生成，都相当于是new Integer()来new一个对象++**，生成的对象不是同一个，所以结果为false。


### 1.2 接着分析Integer与int类型的比较
> - Integer是int的封装类，**++当Integer与int进行==比较时，Integer就会自动拆箱成一个int类型，所以相当于两个基本类型进行比较++**，这里的Integer,不管是直接赋值，还是new创建的对象，只要跟int比较就会自动拆箱为int类型。

### 1.3 大致总结：
> - **++int与int、int与Integer的比较，只要值相等就为true；++**
> - **++Integer与Integer的比较，当它们范围在[-128，128）内的时候且是以 Interger i = xx(xx为一个数字)直接赋值的方式生成时，那么当它们值相同时，返回的对象也是同一个对象，所以结果为true。++**
> - 如果只是想比较Integer对象的值是否相同那么建议用equals方法进行值比较

### 1.4 原理：(1.4原理参考了这个[博客](https://blog.csdn.net/u011035407/article/details/81173075))
#### 问题：为什么Integer当它们范围在[-128，128）内的时候且是以 Interger i = xx(xx为一个数字)直接赋值的方式生成时，返回的对象是同一个对象呢？
> 解答：
>
> - 在源码中可以看出一开始就一次性生成了-128到127直接的Integer类型变量存储在cache[]中，**++对于-128到127之间的int类型，返回的都是同一个Integer类型对象。++**

> - 所以整个工作过程就是：**++Integer.class在装载（Java虚拟机启动）时，其内部类型IntegerCache的static块即开始执行，实例化并暂存数值在-128到127之间的Integer类型对象。当自动装箱int型值在-128到127之间时，即直接返回IntegerCache中暂存的Integer类型对象。++**

> - 为什么Java这么设计？我想是出于效率考虑，因为自动装箱经常遇到，尤其是小数值的自动装箱；而如果每次自动装箱都触发new，在堆中分配内存，就显得太慢了；所以不如预先将那些常用的值提前生成好，自动装箱时直接拿出来返回。哪些值是常用的？就是-128到127了。