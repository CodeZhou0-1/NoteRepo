# 复习数据结构(1)--线性表的Java实现

## Ⅰ.线性表顺序存储的实现

```java
public class List {

        private int size = 0;       //线性表中数据元素的个数
        private Object[] elements; //数据元素数组

        //构造方法
        public List(int maxSize){
            elements = new Object[maxSize];
        }

        // 无参构造方法  默认大小为16
        public List(){
        elements = new Object[16];
       }
    
    
    //判断线性表是否包含数据元素e--循环加判断是否相等
    public boolean contains(Object e){
        int i = 0;
        while (i <= size) {
            if (e.equals(elements[i])) {
                return true;
            }
            i++;
        }
        return false;
    }
    
     //返回数据元素e在线性表中的序号
        public int indexOf(Object e){
            int i = 0;
            while (i <= size) {
                if (e.equals(elements[i])) {
                    return i;
                }
                i++;
            }
            return -1;
        }
    
    // 数组扩容
        private void expandSpace(){
            //创建新数组
            Object[] newObj = new Object[elements.length * 2];
            //将原数组的数据复制到新数组中  //这里开始将最后一个参数写成新数组的长度导致报错
            System.arraycopy(elements,0,newObj,0,elements.length);
            //将新数组赋值给原数组
            elements = newObj;
        }
    
    //添加数据元素e到线性表中
    public void add(Object e) throws RuntimeException {
        if(size == elements.length){
            //数组扩容
            expandSpace();
        }
        //这里开始写成++size，忘记size是元素个数而数组下标要比个数少1
        elements[size++] = e;
    }
    
    //将数据元素e插入到线性表中i号位置
        public void insert(int i, Object e) throws RuntimeException {
            //先判断数组i是否合法
            if(i < 0 || i > size)
                throw new RuntimeException("插入的序号越界");
            //判断是否需要扩容
            if(size == elements.length)
                expandSpace();
            for (int j = size; j > i; j--) {
                elements[j]=elements[j-1];
                //刚开始脑壳短路用这个思路，导致复制的都是一样的
               // elements[i+1]=elements[i];//i++;
            }
            elements[i] = e;

            size++;
        }
    
    //删除线性表中序号为i的元素，并返回之
        public Object remove(int i) throws RuntimeException{
            if(i < 0 || i > size)
                throw new RuntimeException("错误，指定的插入序号越界。");
            Object obj = elements[i];
            for (int j = i ; j < size-1; j++) {
                elements[j] = elements[j+1];
            }
            elements[--size] = null;
            return obj;
        }
    
    //将数据元素e插入到元素obj之前
        public boolean insertBefore(Object obj, Object e){
            int i = indexOf(obj);
            if(i < 0)
                return false;
            insert(i,e);
            return true;
        }

        //将数据元素e插入到元素obj之后
        public boolean insertAfter(Object obj, Object e){
            int i = indexOf(obj);
            if(i < 0)
                return false;
            insert(i+1,e);
            return true;
        }
    
    //删除线性表中第一个与e相同的元素
        public boolean remove(Object e){
            int i = indexOf(e);
            if(i < 0)
                return false;
            remove(i);
            return true;
        }

        //替换线性表中序号为i的数据元素为e，返回原数据元素
        public Object replace(int i,Object e) throws RuntimeException{
            if(i < 0 || i > size)
                throw new RuntimeException("错误，指定的插入序号越界。");
            Object obj = elements[i];
            elements[i] = e;
            return obj;
        }

        //返回线性表中序号为i的数据元素
        public Object get(int i) throws RuntimeException{
            if(i < 0 || i > size)
                throw new RuntimeException("错误，指定的插入序号越界。");
            return elements[i];
        }
    
    
    
}
```

## Ⅱ.线性表链式存储的实现

### LNode

```java
public class LNode {
        private Object data;
        private LNode next;

        public LNode(){
            this(null,null);
        }
        public LNode(Object data){
            this(data,null);
        }
        public LNode(Object data, LNode next){
            this.data = data;
            this.next = next;
        }

        public Object getData() {
            return data;
        }

        public void setData(Object data) {
            this.data = data;
        }

        public LNode getNext() {
            return next;
        }

        public void setNext(LNode next) {
            this.next = next;
        }
}
```

### LinkedList

```java
public class LinkedList {
   
    private LNode head;    //单链表头结点引用
    private int size;
    public LinkedList(){
        size = 0;
        head = new LNode();
    }
    // 辅助方法：获取数据元素e所在结点的前驱结点
    private LNode getPreNode(Object e){
        LNode p = head;
        while(p.getNext() != null){
            if (e.equals(p.getNext().getData())) {
                return p;
            }
            p = p.getNext();
        }
        return null;
    }
    // 辅助方法：获取序号为0<=i<size的元素所在结点的前驱结点
    private LNode getPreNode(int i){
        LNode p = head;
        for(; i > 0; i--)
            p = p.getNext();
        return p;
    }
    //获取序号为0<=i<size的元素所在结点
    private LNode getNode(int i){
        LNode p = head.getNext();
        for(; i > 0; i--)
            p = p.getNext();
        return p;
    }
    //返回线性表的大小，即数据元素的个数。
    public int getSize(){
        return size;
    }
    //如果线性表为空返回true，否则返回false。
    public boolean isEmpty(){
        return size == 0;
    }
    //判断线性表是否包含数据元素e
    public boolean contains(Object e){
        LNode p = head.getNext();
        while(p != null){
            if(Objects.equals(p.getData(),e))
                return true;
            else
                p = p.getNext();
        }
        return false;
    }
    //返回数据元素e在线性表中的序号
    public int indexOf(Object e){
        LNode p = head.getNext();
        int index = 0;
        while(p != null){
            if(Objects.equals(p.getData(),e))
                return index;
            else{
                index++;
                p = p.getNext();
            }
        }
        return -1;
    }
    //将数据元素e插入到线性表中i号位置
    public void insert(int i, Object e) throws RuntimeException {
        if(i < 0 || i > size)
            throw new RuntimeException("错误，指定的插入序号越界。");
        LNode p = getPreNode(i);
        LNode q = new LNode(e,p.getNext());
        p.setNext(q);
        size++;
    }
    //将数据元素e插入到元素obj之前
    public boolean insertBefore(Object obj, Object e){
        LNode p = getPreNode(obj);
        if(p != null){
            LNode q = new LNode(e,p.getNext());
            p.setNext(q);
            size++;
            return true;
        }
        return false;
    }
    //将数据元素e插入到元素obj之后
    public boolean insertAfter(Object obj, Object e){
        LNode p = head.getNext();
        while(p != null){
            if(Objects.equals(p.getData(),obj)){
                LNode q = new LNode(e,p.getNext());
                p.setNext(q);
                size++;
                return true;
            }
            else p = p.getNext();
        }
        return false;
    }
    //默认尾部添加元素
    public void add(Object obj) { insert(getSize(),obj);}
    //删除线性表中序号为i的元素，并返回之
    public Object remove(int i) throws RuntimeException{
        if(i < 0 || i > size)
            throw new RuntimeException("错误，指定的插入序号越界。");
        LNode p = getPreNode(i);
        Object obj = p.getNext().getData();
        p.setNext(p.getNext().getNext());
        size--;
        return obj;
    }
   
    //返回线性表中序号为i的数据元素
    public Object get(int i) throws RuntimeException{
        if(i < 0 || i > size)
            throw new RuntimeException("错误，指定的插入序号越界。");
        LNode p = getNode(i);
        return p.getData();
    }
    //测试
    //打印输出所有结点
    public void print() {
        LNode node = head.getNext();
        while (node != null) {
            System.out.print(node.getData() + " ");
            node = node.getNext();
        }
        System.out.println();
    }
    public static void main(String[] args) {
        LinkedList list = new LinkedList();
        list.add("aaa");
        list.add("bbb");
        list.add("ccc");
        list.add("ddd");
        list.add("eee");
        list.print();
        System.out.println(list.indexOf("ccc"));
        System.out.println(list.get(3));
        System.out.println(list.remove(2));
        list.print();
        list.insert(1,"abc");
        list.print();
        list.replace(1,"abcd");
        list.print();
    }
}
```
## Ⅲ.总结
- list线性表的顺序存储，简单的由一个Object数组存储数据和一个Size表示线性表的长度
- add添加方法：这里 `size++` 开始写成 `++size` 了
```java
   //添加数据元素e到线性表中
    public void add(Object e) throws RuntimeException {
        if(size == elements.length){
            //数组扩容
            expandSpace();
        }
        //这里开始写成++size，忘记size是元素个数而数组下标要比个数少1
        elements[size++] = e;
    }
```
- expandSpace数组扩容方法:
```java
        // 数组扩容
        private void expandSpace(){
            //创建新数组
            Object[] newObj = new Object[elements.length * 2];
            //将原数组的数据复制到新数组中  //这里开始将最后一个参数写成新数组的长度导致报错
            System.arraycopy(elements,0,newObj,0,elements.length);
            //将新数组赋值给原数组
            elements = newObj;
        }
```
- 顺序线性表中较难的两个方法删除和插入方法：
```java
 //将数据元素e插入到线性表中i号位置
        public void insert(int i, Object e) throws RuntimeException {
            //先判断数组i是否合法
            if(i < 0 || i > size)
                throw new RuntimeException("插入的序号越界");
            if(size == elements.length)
                expandSpace();
            for (int j = size; j > i; j--) {
                elements[j]=elements[j-1];
                //刚开始脑壳短路用这个思路，导致复制的都是一样的
               // elements[i+1]=elements[i];//i++;
            }
            elements[i] = e;

            size++;
        }

//删除线性表中序号为i的元素，并返回之
        public Object remove(int i) throws RuntimeException{
            if(i < 0 || i > size)
                throw new RuntimeException("错误，指定的插入序号越界。");
            Object obj = elements[i];
            for (int j = i ; j < size-1; j++) {
                elements[j] = elements[j+1];
            }
            elements[--size] = null;
            return obj;
        }
```

- 有这些基本方法之后，其他方法直接调用就比较好实现了
```java
//将数据元素e插入到元素obj之前
        public boolean insertBefore(Object obj, Object e){
            int i = indexOf(obj);
            if(i < 0)
                return false;
            insert(i,e);
            return true;
        }

```