---
title: "读深入浅出Rust DST"
date: 2024-02-29T14:50:05Z
draft: true
tags: ["Rust"]
author: "IAKSH"
---

Rust中的数组定义方式如下：
<!--more-->
```rust
let array0 = [1,2,3,4,5];
let array1: [i32;5] = [1,2,3,4,5];

// 批量初始化
let array2 = [114; 500];
let array3: [i32;500] = [514; 500];
```
也可以有多维数组，懒得写了，你应该能看懂。

为了解决C/C++中常见的越界问题，Rust对DST（Dynamic Sized Type）使用了胖指针。
实际上就是为DST的指针添加用于表示长度的格外4个字节，这导致DST的指针会比其他指针大至少4字节，所以叫做胖指针。

具体的边界检查通常在trait的impl中。

以前写C的时候就在想类似的问题，这的确是比较符合直觉的解决方案，这部分格外的内存和性能开销大多数时候是能够接受的。

当然，如果你真的在乎这部分性能损失，可以尝试减少或者根本不使用”索引“，转而使用迭代器，这也是Rust所希望你做的。