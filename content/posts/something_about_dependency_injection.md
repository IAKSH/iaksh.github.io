---
title: "something_about_dependency_injection"
date: 2024-02-29T15:05:19Z
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

在复用代码时，尽量使用has-a（组合），而非is-a（public继承）。
一来has-a不会将基类的所有api保留在子类，避免了“鸵鸟非鸟”的问题。
二是子类能在逻辑上与实际的基类解耦。子类依赖接口，而不是实际的基类。

+ 依赖注入（dependency inject，DI）
之前已经提过了，”子类“应当依赖一个固定的接口，而不是某一个指定的类。这样，子类的设计着力点就从依赖的具体类的设计转回了子类本身。
然而这样会带来一个额外的问题：接口是否应该定义如何初始化该类？综合而言，答案是否定的。但如果接口没有规定如何初始化，那么子类又应该如何初始化一个未知初始化方式的成员类实例呢？

答案是DI。让子类所依赖的这个类在子类的外部构造，在这之后使用子类的setter将其绑定到子类中。

至于这个被依赖的类具体如何在子类外部被构造，我目前的想法是做一个工厂模式。当然，还是得看实际情况，如果并不需要太过泛化，直接调用构造函数就好。

DI实际上将一个类的依赖顺序倒置了：
在不使用DI，直接在类中镶嵌其所依赖的类时，整个构造是从顶部开始的，由顶部逐渐向其所依赖的底部进行。这个过程是自动化的，而且几乎是固定死了的。它为我们带来便利的同时，向我们隐瞒了实际上的依赖顺序。

而DI将这个过程倒置了，在构造顶部的类之前，我们需要从最底部的依赖开始，手动的构建整个依赖体系。

# 从维基上抄的一部分关于DI的内容

> 注：编程语言层次下，“接收方”为对象和 class，“依赖”为变量。在提供服务的角度下，“接收方”为客户端，“依赖”为服务。

## 0x00 | 四个基本概念
+ 服务：任何提供了某种功能的类
+ 客户：使用服务的类
+ 接口：客户不应该知道服务实现的细节，只需要知道服务的名称和API
+ 注入器：负责把服务引入给客户，叫法有很多：Injector / Assembler / Container / Provider / Factory
依赖注入把对象构建和对象注入分开，因此创建对象的 new 关键字也可以消失了。

## 0x01 | 实现方式
+ 建钩子注入（构造注入？）：依赖由客户对象的构造函数传入
+ Setter注入：客户对象只能暴露一个能接收依赖的setter方法。
+ 接口注入：依赖的接口提供一个注入器方法，该方法会把依赖注入到任意一个传给它的客户。客户实现一个setter接口，可设置依赖。

## 0x02 | 我的理解
DI实际上就是尝试使客户类的设计关注点回归到类本身，而非其所依赖的类。
认识到了一个新的词 ”关注点分离“。