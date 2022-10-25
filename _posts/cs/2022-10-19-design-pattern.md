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
根据设计模式的意图分类，可以把设计模式分为三类
1. 创建型模式(create)
2. 构造型模式(related)
3. 行为型模式(communicate)

在个人学习设计模式过程中，发现虽然有过多次学习，但是过不了多久就会忘记。最初学习是为了应对面试，后面学习为了软考，其实最终实际用途是再个人工作和开发过程中使用。许多设计模式因为翻译的问题，术语比较晦涩，比如享元模式，桥接，或者由于术语和生活中的一些例子比较接近的缘故会产出一些不太正确的思想。在设计模式学习中，个人发现许多的设计模式思想在我尚未系统学习之前在开发过程中已经简单使用了，和实际的设计模式的区别在于，我可能没有定义最规范的设计模式接口，而导致我虽然用了设计模式的思想，但在实际应用中，代码仍避免不了不断的修改和层次不清晰等问题，在学习理论过后，个人的编程实践中应该会有更好的提升。如果对设计模式的掌握评分的话，可能是由多方面的维度的。
1. 实际开发中结合业务的应用
2. 在理论掌握中，精确的掌握设计模式的各个目标概念。明晰多个容易混淆模式的区别
3. 在实际开发中按照标准设计模式的模型合理建模
4. 综合使用多种设计模式以达到最终目的
5. 在使用设计模式的前提下，写出更好维护，健壮，高内聚低耦合的实际代码
这些维度只要在任意方面有太大短板，就不能发挥出设计模式应有的价值

下面是一些具体的设计模式的一些笔记

![示意图](/assets/pic/design_pattern.png)
### 创建性模式
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
### 构建型模式
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
        装饰器容易让自身成为 [上帝对象](https_en.wikipedia.org/?url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FGod_object),摘自维基百科:
            > Most of such a program's overall functionality is coded into a single "all-knowing" object, which kkvkkmaintains most of the information about the entire program, and also provides most of the methods for manipulating this data. Because this object holds so much data and requires so many methods, its role in the program becomes god-like (all-knowing and all-encompassing) 
 - 享元模式
   - 对象的常量模式被称为`内在状态`,其他对象只能读取,不能改变.对象可以被外部改变的称为`外在状态`.
   - 享元模式建议不在对象中存储外在状态,而是将其传递给依赖他的特殊方法.方便复用.
   - 将外在状态封装在flyweight中,在flyweight 可以创建操纵目标对象的方法
   - 在flyweight容器中 中集合大量常用的fkyweight,需要使用时按照逻辑去工厂中取,如果工厂中没有就自己创建.
   - 个人理解享元模式主要用于优化程序的空间使用情况,但会增加程序的复杂度
 ![模式结构](https://refactoringguru.cn/images/patterns/diagrams/flyweight/structure.png)
  
- 代理模式
    - 代理模式提供一种替换原来接口的功能,却不改变原来的接口的方式.
    - ![模式结构](https://refactoringguru.cn/images/patterns/diagrams/proxy/structure-indexed-2x.png)
    - 比如对模拟数据库的查询缓存就是一种代理,当本地缓存区有符合条件的已缓存数据时,直接使用本地缓存数据,当没有时则查询数据库集群的数据
    - 现实世界中,使用电子支付和使用纸币支付或者信用卡支付 就是对商家付款行为的代理
  ### 行为模式
    - 责任链模式(chain of responsibility)
      - ![模式结构](https://refactoringguru.cn/images/patterns/diagrams/chain-of-responsibility/structure-indexed-2x.png)
      - 类比于现实中，寻找售后客服电话时，客服根据实际问题将用户的问题一层层细化最终传递给最终解决的负责人手里
      - 适合场景
          1.程序不知道使用哪一个处理方法可以处理时（一系列的试错）
          2. 程序必须按照一定顺序处理时
          3. 当处理顺序可能会在运行时改变时
  ### 命令模式 （Command）
    ![模式结构图](https://refacetoringguru.cn/images/patterns/diagrams/command/structure-indexed-2x.png)
        - 核心组成角色，有触发者，接收者，命令类
        - 可以将触发者和执行者解耦
  ### 迭代器模式(Iterator)
     ![模式结构图](https://refactoringguru.cn/images/patterns/diagrams/iterator/structure-2x.png)
        - 隐藏集合的底层结构,提供对类似集合对象的一个遍历操作的封装,可以保证每一个节点只访问一次
        - 根据不同的目的,可以对一个集合的不同遍历方式的抽象
        - 比如对于range 结构,可以在不初始化所有元素的前提下对range 进行有效的封装
  ### 中介模式(Mediator)

  ![模式结构图](https://refactoringguru.cn/images/patterns/diagrams/mediator/structure-2x.png)
      - spring mvc 架构中的 c(Controler) 就是中介者模式的典型应用
[源码](/assets/code/mediator.zip)参考网站中的 java 笔记小程序 
