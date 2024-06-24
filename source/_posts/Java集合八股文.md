---
title: Java集合八股文
date: 2024-06-22 16:48:56
tags: 
   - Java
   - 八股文
categorizes: Java
cover: /img/JavaCollection.png
---

# Java集合

### 说说 List, Set, Queue, Map 四者的区别？

- `List`：存储的元素是有序的、可重复的。

- `Set`：存储的元素不可重复。
- `Queue`：按特定的排队规则来确定先后顺序，存储的元素是 有序的、可重复的。
- `Map`：使用键值对存储，`key`是无序的、不可重复的，`value`是无序的、可重复的。

## List

### ArrayList 和 Array （数组）的区别？

`ArrayList`内部基于动态数组实现，比静态数组使用起来更灵活：

- `ArrayList` 会根据实际存储的元素动态地扩容或缩容，而数组被创建之后就不能改变它的长度了。
- `ArrayList` 可以使用泛型来确保类型安全
- `ArrayList`中只能存储对象。对于基本数据类型，需要使用其对应的包装类。数组可以直接存储基本数据类型，页可以存储对象。
- `ArrayList` 支持插入、删除、遍历等常用操作，并提供了很多`API`，数组只能通过下标访问其中的元素，不具备动态添加、删除元素的能力。
- `ArrayList` 创建时不需要指定大小，而数组创建时必须指定大小。

### ArrayList 可以添加 null 吗？

`ArrayList`中可以存储任何类型的对象，包括`null`。

