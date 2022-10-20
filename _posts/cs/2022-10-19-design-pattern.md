---
layout: post
title:  "设计模式学习"
date:   2022-10-17 11:11:00 +0800
categories: 个人学习
tags : program
---

设计模式是软件开发过程中一些常见问题的解决方案,有助于解决代码设计过程中出现的问题.
23个设计模式最早由四位开发者的《设计模式： 可复用面向对象软件的基础》 中完整的阐释,由于书名太长,后来人们将其简称为四人组的书(Gang of Four).
设计模式相当于定义了一套解决方案的抽象,不同于算法那样明确定义解决觉最终目的的一系列步骤。而设计模式倾向于解决方案的高层次的描述。
设计模式不同于库这样实现好的代码而是一套一般性概念。
设计模式包含的四个核心概念是 1. 意图2. 动机3.结构4.实现
关于设计模式存在一些争议：
1.当编程语言确实必备的抽象功能时，人们才需要设计模式来补充。比如 lamda 函数替换了大部分可能需要使用策略模式的情形.scala 中的object 解决了需要手动实现单例模式的问题.
1. 设计模式必须合理使用.`如果你只有一把铁锤， 那么任何东西看上去都像是钉子。`

根据应用的层面可以吧模式分为 惯用技巧(最基础最底层的)和构架模式(最通用最高层的).
根据设计模式的意图分类
可以把设计模式分为三类
1. 创建型模式(create)
2. 构造型模式(related)
3. 行为型模式(communicate)

![示意图](/assets/pic/design_pattern.png)
1.创建性模式
- 单例模式设计意图(Singleton)
1. 保证一个类只有一个实例
2. 为该实例提供全局访问点
- 工厂模式的一些概念辨析(factory method)
  1. 静态工厂方法不基于继承,仅仅是提够了几个便捷的构建方法,严格意义上不是工厂模式
  2. 构建方法可以有很多种类别,只要可以产生新对象的方法就可以理解为构建方法
  3. 工厂模式一定是基于继承
- 抽象工厂模式(abstract factory)
  1. 主要用于一系列类似对象的构建,比如`家具`和`装饰风格风格`,定义多组具体风格的工厂产生特定风格的家具,再使用抽象工厂方法来构建决定使用什么风格.个人理解抽象工厂模式是对工厂的工厂
- 原型模式(Prototype)
  1. 首先需要一个原型对象,然后对原型对象进行修改赋值,来达到创建新对象的方式
- 生成器模式(Builder)
  1. 类似于一个生产流水线,可以根据模块化生产,最后组装成一个复杂的产品,在未最终创建产品(出厂)之前,组装的都不是产品
  2. 可以设置管理员类,使用该类生成一些常见的对象结构
2. 构建型模式
   - 适配器模式(Adapter)
   1. 适用场景:
        a.希望使用某个类,但是类的接口于其他代码不兼容(xml 生成图表,但是json )
        b.需要复用一些类,他们处于同一个继承体系,并且他们有额外有一些共同的签名的方法,但是这些方法又有所区别(? 需要一个实例说明)
   - 桥接模式(Bridge)
    1. 将一个实体的控制权转移到一个抽象关系中（类似于遥控器和不同具体设备的关系，GUI和具体系统实现的关系
    2. 常常用在跨平台适配中
    - 组合模式
    1. 树形结构的模式存在时可以使用组合模式来实现.组合模式分为容器(根节点)和实体(叶子节点)
    - 装饰器模式(Decorator)
    1. 装饰器类实现装饰对象的接口
    2. 无需修改代码的情况下即可使用对象， 且希望在运行时为对象新增额外的行为， 可以使用装饰模式
    3. 在继承无法实现功能的时候,跳过final 限制实现扩展功能
   - 外观模式(Facade)
     1. `god object` 
  3. ![上帝对象](https_en.wikipedia.org/?url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FGod_object)

摘自维基百科
   > Most of such a program's overall functionality is coded into a single "all-knowing" object, which maintains most of the information about the entire program, and also provides most of the methods for manipulating this data. Because this object holds so much data and requires so many methods, its role in the program becomes god-like (all-knowing and all-encompassing) 
