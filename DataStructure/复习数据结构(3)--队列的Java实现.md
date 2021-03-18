# 复习数据结构(3)--队列的Java实现

## Ⅰ. 队列的顺序存储
```java
package com.zhou.datastruct.list;

public class Queue {
    private Object[] elements;
    private int capacity;   //数组的大小
    private int front;      //队首指针，指向队首
    private int rear;       //队尾指针，指向队尾后一个位置
    public Queue(){
        this(10);
    }
    public Queue(int cap){
        this.capacity = cap + 1;  //多一个不用的空间方便判断队满和队空
        elements = new Object[capacity];
        front = rear = 0;
    }
    //返回队列的大小
    public int getSize(){
        return (rear - front + capacity) % capacity;
    }
    //判断队列是否为空
    public boolean isEmpty(){
        return front == rear;
    }
    // 判断队列是否已满
    public boolean isFull(){
        return front == (rear+1) % capacity;
    }
    //数据元素e入队
    public void enqueue(Object e) throws RuntimeException{
        if(getSize() == capacity - 1)
            throw new RuntimeException("错误：队列已满");
        elements[rear] = e;
        rear = (rear + 1) % capacity;//每次取模是为了变成循环队列防止假溢出
    }
    //队首元素出队
    public Object dequeue() throws RuntimeException{
        if(isEmpty())
            throw new RuntimeException("错误：队列为空");
        Object obj = elements[front];
        elements[front] = null;
        front = (front + 1) % capacity;
        return obj;
    }
    //取队首元素，但不出队
    public Object peek() throws RuntimeException{
        if(isEmpty())
            throw new RuntimeException("错误：队列为空");
        return elements[front];
    }
    //测试
    public static void main(String[] args) {
        Queue queue = new Queue(6) ;
        queue.enqueue("a");
        queue.enqueue("a1");
        queue.enqueue("a2");
        queue.enqueue("a3");
        queue.enqueue("a4");
        queue.enqueue("a5");
        System.out.println(queue.getSize());
        System.out.println(queue.isEmpty());
        System.out.println(queue.isFull());
        System.out.println(queue.peek());
        System.out.println(queue.dequeue());
        System.out.println(queue.peek());
    }
}
```

### Ⅱ. 队列的链式存储实现
```java
package com.zhou.datastruct.list;

public class LinkedQueue {
  
    private LNode front;  //队头节点
    private LNode rear;  //队尾
    private int size;   //队列大小
    public LinkedQueue(){
        front = new LNode();
        rear = front;
        size = 0;
    }

    //返回队列的大小
    public int getSize(){
        return size;
    }

    //判断队列是否为空
    public boolean isEmpty(){
        return size == 0;
    }

    //数据元素e入队
    public void enqueue(Object e){
        LNode p = new LNode(e);
        rear.setNext(p);
        rear = p;
        size++;
    }

    //队首元素出队
    public Object dequeue() throws RuntimeException{
        if(size < 1)
            throw new RuntimeException("错误：队列为空");
        LNode p = front.getNext();
        front.setNext(p.getNext());
        size--;
        if(size < 1)
            rear = front;    //如果队列为空，rear指向头结点
        return p.getData();
    }

    //取队首元素，但不出队
    public Object peek() throws RuntimeException{
        if(size < 1)
            throw new RuntimeException("错误：队列为空");
        return front.getNext().getData();
    }


    //测试
    public static void main(String[] args) {
        LinkedQueue queue = new LinkedQueue() ;
        queue.enqueue("a");
        queue.enqueue("b");
        queue.enqueue("c");
        queue.enqueue("d");
        queue.enqueue("e");
        queue.enqueue("f");

        System.out.println(queue.getSize());
        System.out.println(queue.peek());
        System.out.println(queue.dequeue());
        System.out.println(queue.dequeue());
        System.out.println(queue.peek());
    }
}
```
### Ⅲ. 思考
- 主要在于顺序队列：
   - 队列由：**队头，队尾和数组大小**三个基本属性组成
   - **队头和队尾每次+1之后要取模**：是为了将队列变为循环队列，这样就不会发生假溢出
   - **数组大小比实际输入的大小大1**：因为当队列变成循环队列之后，队空和队满的判断条件都是队头==队尾即 `rear = front`，空一个数据元素不去用，是为了判断队空和队满，此时队空的判断条件为 `rear = front`，队满的判断条件为 `(rear+1) % capacity = front` 为队满
   - ![image.png](http://codezhou.com/upload/2021/03/image-19a0c4c1d5684756a7d39af48f772c8b.png)
 - 链式队列
   - 两个节点一个队头一个队尾再加上数组大小size
   - 队头相当于头节点，队尾一直指向最后一个元素（顺序队列队尾是指向最后一个元素后面一个元素），当入队时：` rear.setNext(p);  rear = p; size++;`,出队时：`LNode p = front.getNext(); front.setNext(p.getNext());`