---
layout: post
title:  "设计模式学习"
date:   2022-10-17 11:11:00 +0800
categories: 个人学习
tags : program
---

### 使用统一建模语言描述类之间的关系

  - 关联
  - 继承      
  - 实现
1. 类之间的关系
    - inheritance:
       - is_a
       - 抽象类用斜体字母表示
       - 子指向父的空心箭头
   - realization:
       - 实现是接口和具体实现的关系,或者蓝图和实现细节的关系
   - association:
     - verb       
     - 关联关系直接使用直线开箭头表示
     - 简单关联: - peer class 或者有关联的类之间     
     - Cardinality(基线):1-1,1-n,n-m,orderd,0..1,*
     - Aggregation:
       - part of
       - 对象间有不同的生命周期
       - 使用未填充的菱形表示
     - Composition:
       - Aggregation 中的特例,part destroyed when the whole destroyed
       - 使用填充菱形(filled diamond )箭头表示
     - Dependecy:
       - 一种关联的特殊类型
       - 一个类变换引起另一个类的变化，反过来不成立，这就说明这个类依赖于另一个类
       - 使用虚线开箭头（dashed line open arrow）表示
2. 示例图
   - 清单系统
    ![清单系统](https://cdn-images.visual-paradigm.com/guide/uml/uml-class-diagram-tutorial/17-class-diagram-example-order-system.png)
   - 图形用户接口
    ![图形用户接口](https://cdn-images.visual-paradigm.com/guide/uml/uml-class-diagram-tutorial/18-uml-class-diagram-example-gui.png)

[原文链接](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/uml-class-diagram-tutorial/)