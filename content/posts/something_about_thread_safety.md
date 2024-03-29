---
title: "something_about_thread_safety"
date: 2024-02-29T15:06:18Z
# weight: 1
# aliases: ["/first"]
tags: ["Others"]
author: "IAKSH"
# author: ["Me", "You"] # multiple authors
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "比较早期的一篇文章，可能存在问题"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
editPost:
    URL: "https://github.com/IAKSH/iaksh.github.io/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

# 0x00 | 基本意义
当某个功能在多线程环境中被调用时，能够正确地处理多线程之间的共用变量，使程序能够按照预期正确执行。

如果一个功能（比如函数）在执行过程中需要修改一些公共变量，而且其功能是连续的，这一次的功能会受到上一次执行后对公用变量的修改的影响。
也就是说这个功能是受调用顺序影响的。
那么这个功能就不是线程安全的。
因为在多线程环境下，无法确定线程间调用功能的顺序。各个线程将会相互干扰。

# 0x01 | 锁
常见的就是mutex，互斥锁。
这是一种十分暴力的解决方案，简单易用，但是容易让程序退化回单线程。

# 0x02 | 原子操作
就目前而言，原子操作已经上升为CPU的一套指令集了。
原子操作通过硬件电路实现了一套基本没有开销的线程安全实现方案。
C++标准库中提供了相关API