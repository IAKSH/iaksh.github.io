---
title: "读深入浅出Rust 指针"
date: 2024-02-29T14:49:57Z
draft: true
tags: ["Rust"]
author: "IAKSH"
---

虽然但是Rust实际上是允许你使用指针的，不过大多都是经过包装过的，相对更安全的指针，并且在编译时会对其进行远比C/C++严格的检查。
<!--more-->
实际上就是C++中出现过的所有权机制。

# 0x00 | 指针
`Box<T>` 指向T类型的，具有所有权的指针，有权释放内存
`&T` 指向类型T的借用指针（实际上就是引用，不过实现方式与C++不同），无权写入，无权释放内存
`&mut T` 指向类型T的mut型借用指针，和上面的那个区别是这个可以写入
`*const T` 指向T类型的只读裸指针。就是C/C++意义上的指针，无权读写
`*mut T` 和上面那个的区别是有权读写

# 0x01 | 智能指针
`Rc<T>` 指向类型T的引用计数指针，共享所有权，线程不安全
`Arc<T>` 指向类型T的原子型引用计数指针，共享所有权，线程安全。
`Cow<'a,T>` 写时复制指针。可能是借用指针，也可能是有所有权的指针。