---
title: "Queue Stack Heap PriorityQueue Deque"
layout: post
date: 2019-04-01 11:13
#image: /assets/images/markdown.jpg
headerImage: false
tag:
- Data Structure
star: true
category: blog
author: Yingbo Yuan
description: Difference between Queue Stack Heap PriorityQueue Deque
---
研究Java中Queue Stack Heap PriorityQueue Deque的使用和区别

---

### Queue
Queue接口与List、Set同一级别，都是继承了Collection接口。LinkedList实现了Queue接 口。Queue接口窄化了对LinkedList的方法的访问权限（即在方法中的参数类型如果是Queue时，就完全只能访问Queue接口所定义的方法 了，而不能直接访问LinkedList的非Queue的方法），以使得只有恰当的方法才可以使用。BlockingQueue 继承了Queue接口。

队列通常（但并非一定）以 FIFO（先进先出）的方式排序各个元素。不过PriorityQueue和 LIFO 队列（或堆栈）例外，前者根据提供的比较器或元素的自然顺序对元素进行排序，后者按 LIFO（后进先出）的方式对元素进行排序。无论使用哪种排序方式，队列的头 都是调用 `remove()` 或 `poll()` 所移除的元素。在 FIFO 队列中，所有的新元素都插入队列的末尾。其他种类的队列可能使用不同的元素放置规则。每个 Queue 实现必须指定其顺序属性。

Queue 主要方法：

`E element()`获取，但是不移除此队列的头。

`boolean offer(E e)`将指定的元素插入此队列（如果立即可行且不会违反容量限制），当使用有容量限制的队列时，此方法通常要优于 add(E)，后者可能无法插入元素，而只是抛出一个异常。

`E peek()`获取但不移除此队列的头；如果此队列为空，则返回 null。

`E poll()`获取并移除此队列的头，如果此队列为空，则返回 null。

Queue使用时要尽量避免Collection的`add()`和`remove()`方法，而是要使用`offer()`来加入元素，使用`poll()`来获取并移出元素。它们的优点是通过返回值可以判断成功与否，`add()`和`remove()`方法在失败的时候会抛出异常。 如果要使用前端而不移出该元素，使用`element()`或者`peek()`方法。poll和peek方法出错进返回null。因此，向队列中插入null值是不合法的。

例子：
{% highlight java %}
Queue<String> queue=new LinkedList<String>();
queue.offer("Hello");
queue.offer("World!");
queue.offer("你好？");
System.out.println(queue.size()); //3
for(String str:queue){
    System.out.printf(str + "  ");//Hello World ! 你好？
}
System.out.printf("\n");
System.out.println(queue.size());//3
String str;
while((str=queue.poll()) != null){
    System.out.printf(str + "  ");//Hello World ! 你好？
}
System.out.println();
System.out.println(queue.size());//0
{% endhighlight %}

---

### Stack
Stack继承Vector类，它通过五个操作对类 Vector 进行了扩展。 Stack是后进先出的。 Stack提供了通常的 `push` 和 `pop` 操作，以及取Stack顶点的 `peek` 方法、测试Stack是否为空的 `empty` 方法、在Stack中查找项并确定到Stack顶距离的 `search` 方法。

例子：
{% highlight java %}
Stack<String> stack = new Stack<String>();
System.out.println("now the satck is "+isEmpty(stack));
stack.push("1");
stack.push("2");
stack.push("3");
stack.push("4");
stack.push("5");
stack.push("6");
System.out.println("now the stack is "+isEmpty(stack));
System.out.println(stack.peek());//查看堆栈顶部的对象，并返回该对象，但不从堆栈中移除它。
System.out.println(stack.pop());
System.out.println(stack.pop());
System.out.println(stack.search("3"));//，此方法返回最近的目标对象距堆栈顶部出现位置到堆栈顶部的距离；
{% endhighlight %}

---

### Heap
系统一般在内存中划分出两种不同的内存空间，一种是Stack(栈),一种是heap(堆)

它们的主要区别是：

stack是有结构的，每个区块按照一定次序存放，可以明确知道每个区块的大小；heap是没有结构的，数据可以任意存放。因此，stack的寻址速度要快于heap。

每个线程分配一个stack，每个进程分配一个heap，也就是说，stack是线程独占的，heap是线程共用的。

stack创建的时候，大小是确定的，数据超过这个大小，就发生stack overflow错误，而heap的大小是不确定的，需要的话可以不断增加。

如果栈内存没有可用的空间存储方法调用和局部变量，JVM会抛出java.lang.StackOverFlowError。
而如果是堆内存没有可用的空间存储生成的对象，JVM会抛出java.lang.OutOfMemoryError。

使用-Xss设置内存中栈的大小，使用-Xms设置最小堆内存，使用-Xmx设置最大堆内存。

数据存放的规则是：只要是局部的、占用空间确定的数据，一般都存放在stack里面，否则就放在heap里面。

栈的优势是，存取速度比堆要快，仅次于直接位于CPU中的寄存器。但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。堆的优势是可以动态地分配内存大小，生存期也不必事先告诉编译器，Java的垃圾收集器会自动收走这些不再使用的数据。但缺点是，由于要在运行时动态分配内存，存取速度较慢。

### Priority Queue

PriorityQueue是一个比较标准的Queue实现类，之所以说它是比较标准的Queue实现，而不是绝对标准的Queue实现是因为：PriorityQueue保存队列元素的顺序并不是按加入队列的顺序，而是按队列元素的大小进行重新排序。因此当调用peek方法活着pull方法来取出队列中的元素时，并不是取出最先进入队列的元素，而是取出队列中最小的元素。从这个意义上看，PriorityQueue已经违反了队列的最基本原则:先进先出(FIFO)。

例子：
{% highlight java %}
PriorityQueue<Integer> pq = new PriorityQueue<Integer>();
            pq.offer(3);
            pq.offer(-6);
            pq.offer(9);
            System.out.println(pq);//打印结果为[-6, 3, 9]
            System.out.println(pq.peek());//打印结果为-6
            System.out.println(pq.poll());//打印结果为-6
{% endhighlight %}

---

### Deque

Deque 是 Double ended queue (双端队列) 的缩写。
Deque 继承自 Queue,直接实现了它的有 LinkedList, ArayDeque, ConcurrentLinkedDeque 等。
Deque 支持容量受限的双端队列，也支持大小不固定的。一般双端队列大小不确定。
Deque 接口定义了一些从头部和尾部访问元素的方法。比如分别在头部、尾部进行插入、删除、获取元素。和 Queue 类似，每个操作都有两种方法，一种在异常情况下直接抛出异常奔溃，另一种则不会抛异常，而是返回特殊的值，比如 false, null

