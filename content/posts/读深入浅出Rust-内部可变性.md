---
title: "读深入浅出Rust 内部可变性"
date: 2024-02-29T14:49:24Z
draft: true
tags: ["Rust"]
author: "IAKSH"
---

主要是使用标准库中的`Cell`,`RefCell`,`UnsafeCell`实现的。
<!--more-->
实际上就是用模板把类型包装了一下，保证了对其的读写的线程安全。
主要就是用来解决多引用对同一值的读写权问题，主要的应用场景应该就是多线程。

用`XXCell`包装的数据无论是否为mut，都可变。
