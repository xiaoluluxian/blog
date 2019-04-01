---
title: "Map Set List"
layout: post
date: 2019-02-22 11:13
#image: /assets/images/markdown.jpg
headerImage: false
tag:
- Data Structure
star: true
category: blog
author: Yingbo Yuan
description: Difference between Set(Hash), Map(Hash), List(ArrayList, LinkedList)
---
研究Java中的各种集合之间的区别以及如何使用。

---

<div class="side-by-side">
    <div class="">
        <img class="image" src="http://images0.cnblogs.com/blog/682514/201411/161043454007612.png" alt="Alt Text">
        <figcaption class="caption">集合结构图</figcaption>
    </div>
</div>

### General

Java 中有两个接口，一个是Collection，另一个是Map。Set和List都是Collection的子类(Set and List implement from Collection)。在Java中，我们不可以直接使用Collection，使用的都是继承自Collection的子类（Set，List）。

Collection主要方法:

`boolean add(Object o)`添加对象到集合

`boolean remove(Object o)`删除指定的对象

`int size()`返回当前集合中元素的数量

`boolean contains(Object o)`查找集合中是否有指定的对象

`boolean isEmpty()`判断集合是否为空

`Iterator iterator()`返回一个迭代器

`boolean containsAll(Collection c)`查找集合中是否有集合c中的元素

`boolean addAll(Collection c)`将集合c中所有的元素添加给该集合

`void clear()`删除集合中所有元素

`void removeAll(Collection c)`从集合中删除c集合中也有的元素

`void retainAll(Collection c)`从集合中删除集合c中不包含的元素

### Set

* Set 继承自 Collection。Set中不允许存在两个同样的元素。假如我们有两个元素，e1和e2。如果 `e1.equals(e2)==true`，且e1已经被add到Set中了，那么  `Set.add(e2)`将不会把e2 加入到Set中。
* 最多只能有一个null元素
* 在Set中，object是没有顺序的。
* 一般来说如果Object不属于Java基本库，我们需要覆盖（override）父类的方法  `equals()`

HashSet和TreeSet是两个最常见的继承自Set的类。


### List

* List 继承自 Collection。List中允许重复的对象。
* List中可以存在多个null元素
* List中的Object都是有顺序的，输出的顺序即是输入的顺序

LinkedList, ArrayList







