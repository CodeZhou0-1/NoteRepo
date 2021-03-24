 # Java集合类复习

### Ⅰ. List，Set，Map的各自特点

- `List`：存储的元素是有序的，且允许元素重复
- `Set` ：存储的元素不允许重复，HashSet的元素存储是无序的，不按照插入顺序存储，LinkedSet和TreeSet都可以按照添加的顺序进行遍历
- `Map`： 存储一对键值对，key不允许重复，如果重复插入则会替换Value的值

---

- `List`
  - `Arraylist`： 线程不安全，底层是Object[]数组，适用于修改和查找次数多的场景
  - `LinkedList`：线程不安全， 底层是双向链表(JDK1.6 之前为循环链表，JDK1.7 取消了循环)，适合于增加和删除多的场景
  - `Vector`：线程安全，底层是Object[]数组

- `Set`
   - `HashSet`: 存储元素无序且唯一，基于 HashMap 实现的，底层采用 HashMap 来保存元素
   - `LinkedHashSet`：LinkedHashSet 是 HashSet 的子类，并且其内部是通过 LinkedHashMap 来实现的。
   - `TreeSet`：存储的元素有序，红黑树(自平衡的排序二叉树)


- `Map`
  - `HashMap`： JDK1.8 之前 HashMap 由数组+链表组成的，数组是 HashMap 的主体，JDK1.8 以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间
LinkedHashMap： LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。详细可以查看：《LinkedHashMap 源码详细分析（JDK1.8）》
  - `Hashtable`： 数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的,线程安全的，只不过基本不采用这个，而是用CurrentHashMap，HashMap 可以存储 null 的 key 和 value，但 null 作为键只能有一个，null 作为值可以有多个；HashTable 不允许有 null 键和 null 值，否则会抛出 NullPointerException。
  - `TreeMap`：可以对集合中的元素根据键排序的，相比于HashMap来说 TreeMap 主要多了对集合中的元素根据键排序的能力以及对集合内元素的搜索的能力，默认是按 key 的升序排序，不过我们也可以指定排序的比较器。示例代码如下：
 ```java
  public class Person {
    private Integer age;

    public Person(Integer age) {
        this.age = age;
    }

    public Integer getAge() {
        return age;
    }


    public static void main(String[] args) {
     //在创建TreeMap对象的时候传入Comparator对象并实现它的compare方法
        TreeMap<Person, String> treeMap = new TreeMap<>(new Comparator<Person>() {
            @Override
            public int compare(Person person1, Person person2) {
                int num = person1.getAge() - person2.getAge();
                return Integer.compare(num, 0);
            }
        });
        treeMap.put(new Person(3), "person1");
        treeMap.put(new Person(18), "person2");
        treeMap.put(new Person(35), "person3");
        treeMap.put(new Person(16), "person4");
        treeMap.entrySet().stream().forEach(personStringEntry -> {
            System.out.println(personStringEntry.getValue());
        });
    }
}

 ```



### Ⅱ. 关于集合类的一些问题
- 1、什么是无序性？无序性不等于随机性 ，**无序性是指存储的数据在底层数组中并非按照数组索引的顺序添加 ，而是根据数据的哈希值决定的。**

- 2、什么是不可重复性？**不可重复性是指添加的元素按照 equals()判断时 ，返回 false，需要同时重写 equals()方法和 HashCode()方法。**

- 3、 散列表如何查重？**当把对象加入HashSet时，HashSet 会先计算对象的hashcode值来得到索引值判断对象加入的位置，如果该索引位置没有元素则HashSet会认为没有元素重复而直接插入，但是如果发现该索引位置由对象存在，这时会调用equals()方法来检查 hashcode 相等的对象是否真的相同**。如果两者相同，HashSet 就不会让加入操作成功。所以这也是为什么在重写equals方法的时候，要先重写hashCode方法的原因

- 4、关于集合类遍历？ Set通常使用迭代器和增强for循环，Map的遍历一般通过keySet()方法得到key然后遍历或者enterySet()方法

### Ⅲ. 练习

![QQ图片20210324221752.png](http://codezhou.com/upload/2021/03/QQ%E5%9B%BE%E7%89%8720210324221752-10551c0c7ba04426b71a03c5dc7db98c.png)

- **第一次输出**：输出P1和P2，因为remove(p1)方法没有删除到p1，原因是p1的name字段已经被修改了，remove方法是根据哈希码计算索引位置删除该索引位置的对象，但是Person类的hashCode方法已经被重写为根据id和name计算，所以删除时候计算的哈希码与原先存储时候的哈希码不一样了，删除到其他索引位置去了，并没有删除p1

- **第二次输出**：输出三个对象，因为第三次添加的时候，计算出的索引位置并没有存放其他数据，跟它现在name和id相同的p1位置是根据修改前的name计算出的索引来存储的，所以添加成功

- **第三次输出**：输出四个对象，因为第四次添加的时候，计算出索引位置和p1相同，但是调用equals方法去比较的时候，name不相同，于是链接到了p1的后面，添加成功