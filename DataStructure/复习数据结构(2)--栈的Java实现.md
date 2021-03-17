 # 复习数据结构(2)--栈的Java实现

### Ⅰ.栈的顺序存储实现
```java
package com.zhou.datastruct.list;

public class Stack {

    private final int LEN = 6;  //数组的默认大小
    private Object[] elements;  //数据元素数组
    private int top;            //栈顶指针

    public Stack() {
        top = -1;   //top=-1表示空栈
        elements = new Object[LEN];
    }

    //返回堆栈的大小
    public int getSize() {
        return top + 1;
    }

    //判断堆栈是否为空
    public boolean isEmpty() {
        return top < 0;
    }

    //数据元素e入栈
    public void push(Object e) {
        if (getSize() >= elements.length)
            expandSpace();
        elements[++top] = e;
    }

    //数组扩容
    private void expandSpace() {
        Object[] a = new Object[elements.length * 2];
        System.arraycopy(elements,0,a,0,elements.length);
        elements = a;
    }

    //栈顶元素出栈
    public Object pop() throws RuntimeException {
        if (getSize() < 1)
            throw new RuntimeException("错误，堆栈为空");
        Object obj = elements[top];
        elements[top--] = null;
        return obj;
    }

    //取栈顶元素，但不出栈
    public Object peek() throws RuntimeException {
        if (getSize() < 1)
            throw new RuntimeException("错误，堆栈为空");
        return elements[top];
    }

    //测试
    public void print() {
        for (int i = top; i >= 0; i--) {
            System.out.print(elements[i] + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        Stack stack = new Stack();
        stack.push("a");
        stack.push("b");
        stack.push("c");
        stack.push("d");
        stack.push("e");
        stack.print();
        System.out.println(stack.peek());
        System.out.println(stack.pop());
        System.out.println(stack.pop());
        System.out.println(stack.peek());
        stack.print();
        System.out.println(stack.getSize());
    }
}

```

### Ⅱ.栈的链式存储实现
```java
package com.zhou.datastruct.list;

public class LinkedStack {
    
    private LNode top;    //链表首结点引用
    private int size;
    public LinkedStack(){
        top = null;
        size = 0;
    }

    //返回堆栈的大小
    public int getSize(){
        return size;
    }

    //判断堆栈是否为空
    public boolean isEmpty(){
        return size == 0;
    }

    //数据元素e入栈
    public void push(Object e){
        LNode q = new LNode(e,top);  //插入到首元节点前面
        top = q;  //将新插入的节点设为首元节点
        size++;
    }

    //栈顶元素出栈
    public Object pop() throws RuntimeException {
        if(size < 1)
            throw new RuntimeException("错误，堆栈为空");
        Object obj = top.getData();
        top = top.getNext();
        size--;
        return obj;
    }

    //取栈顶元素，但不出栈
    public Object peek() throws RuntimeException{
        if(size < 1)
            throw new RuntimeException("错误，堆栈为空");
        return top.getData();
    }


    //测试
    public void print(){
        LNode p = top;
        while(p != null){
            System.out.print(p.getData() + " ");
            p = p.getNext();
        }
        System.out.println();
    }

    public static void main(String[] args) {
        LinkedStack stack = new LinkedStack();
        stack.push("aaa");
        stack.push("bbb");
        stack.push("ccc");
        stack.push("ddd");
        stack.push("eee");

        stack.print();
        System.out.println(stack.peek());
        System.out.println(stack.pop());
        System.out.println(stack.peek());
        stack.print();
        System.out.println(stack.getSize());
    }


}
```
### Ⅲ. 思考
- 顺序栈：数组加栈顶标志，栈顶标志可以从0或者-1开始，入栈的时候判断是否扩容后直接插入到数组++top的位置，出栈的时候直接弹出最后一个，栈顶再-1
- 顺序栈再考虑下数组的默认大小和数组扩容的问题
- 链式栈：一个首元节点加栈的size字段，**入栈**的时候将新节点插入到最前面的位置：`newObj = new LNode(e,top)`和`top = newObj`两个语句实现再将size+1.**出栈**的时候，将第一个节点往后移一位：`top = top.getNext`再size-1