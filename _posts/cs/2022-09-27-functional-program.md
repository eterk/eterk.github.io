---
layout: post
title:  "函数编程设计模式"
date:   2022-09-27 11:11:00 +0800
categories: 个人学习
tags : computer
---

### 观看(Functional Design Patterns - Scott Wlaschin)[https://www.youtube.com/watch?v=srQt1NAHYC0]笔记

许多需要在面向对象风格中专门处理的设计模式在函数式编程中，并不需要单独学习。
这些设计模式其实可以和语言底层抽象息息相关。比如在scala 中 Object 定义开始就是单例的，也就没有专门去写一个单例模式的必要。
`Scott Wlaschin`在演讲中介绍了许多函数式编程里面向对象设计模式的替代方案。
在面向对象编程中有七大准则：
1. 开闭原则(open/closed):对修改关闭，对添加功能开放;
2. 单一职责原则（single responsibility);
3. 依赖注入（Dependency Inversion);---> 使用部分应用函数来实现依赖注入
4. 接口分离(interface segregation):每一个接口应该只有一个方法---> 函数类型是接口
5. 里氏替换原则;

函数式编程的基本原则
1. 函数就是事物（ functions are things）
2. 皆可组合（composition everywhere）
3. 类型不是类（type are not classes）
 在函数式编程思想中,函数不在是类似于面向对象中一个对象的行为,它本身就是一个对象,一个实体。
 皆可组合这个原则让我很是震惊。前两天刚刚接触到了scala 类型编程的教程时就感觉这个类型组合很神奇，在`Scott Wlaschin` 的讲解中我对类型系统的理解更加深入。
 类型的概念更类似于同一类（有某一共同点）事物的集合的抽象，所以 类型 A 和 类型B 的交集也可以是一个新的类型A∩B。
在函数式编程原则的基础上

`Scott Wlaschin` 分享了一些函数编程的设计原则，这些原则可以让我们在日常的编程中写出更优质的代码。
- Strive for totality:争取全部，就是一个函数最好可以处理所有可能遇到的问题,而不是简单的 throw exception. 这点在java 编程中很普遍，随处可见的异常处理代码,程序常常运行在中间由于各种各样的问题就失败。而这个解决方案在函数编程的设计中就可以解决。
- Use static types for domainmodelling and documentation: 这个相信很多使用过动态类型语言的人都深有体会。又慢又容易出问题。

1. 连续的链条和厄运金字塔

![厄运金字塔类型代码](/assets/pic/pryamid_of_doom_code.png)

![厄运金字塔类型解决方案](/assets/pic//pryamid_of_doom_code_solve.png)



在演讲中个人收获最大的是 对monad monoid 的理解更清晰了一些。
在函数编程中这一块的概念晦涩难懂，但是非常的实用和值得花时间去思索。

monoid 法则
如果一类操作符合以下三个法则，那么这类操作就可以抽象为一个monid.
法则1（Closure） A[1] act A[2] => A[3] 
两个同类事物的结合的结果总是这类事物中的一个. 比如自然数的加法就符合这个法则

`the result of combining two things is always another one of the things`

编程用途： 规约操作

法则2(Associativity) (A[1] act A[2]) act A[3]=A[1] act (A[2] act A[3])
 当同时结合多个同类事物，任意组合的顺序不影响结果。比如数学中自然数加法的结合律就是这个法则在自然数加法中的概括
 ``
`When combining more than two things, which pairwise combination you do first doesn't matter`

编程用途： 并行操作,累加操作

法则3(Identity)  a act A[1]=A[1] a act A[2]=A[2]
存在一个特殊的事物0, 任何其它事物 和0结合都等于自身。 比如加法中的0
`Rule 3 (ldentity eIement):There is a special thing called "zero" such that when you combine any thing with "zero" you get the original thing back`

编程用途：用 这个identity 初始化空值或者不存在的值

在演讲中,`Scott Wlaschin` 介绍了Option 的使用方法。
他提出了一个叫 `Railway Oriented Programming` 我查了一下还真有这个概念。
在一般的Java 开发中 一个方法 他接受一个输入，有时