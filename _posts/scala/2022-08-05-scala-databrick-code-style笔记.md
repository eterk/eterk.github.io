---
layout: post
title:  "scala databricks 编码规范阅读记录"
date:   2022-08-05 07:25:10 +0800
categories: note
tags: scala
---

[原文链接-移步阅读](https://github.com/databricks/scala-style-guide)

本文只记录一些作者本人之前不太了解或者违背较多的的编码规范
- 括号省略的问题
1. 有side-effect 的函数不能省略括号
2. 函数带括号调用时就带括号，不能省略（个人理解）
    Callsite should follow method declaration, i.e. if a method is declared with parentheses, call with parentheses. Note that this is not just syntactic. It can affect correctness when apply is defined in the return object.

    ```
    class Foo {
    def apply(args: String*): Int
    }

    class Bar {
    def foo: Foo
    }

    new Bar().foo  // This returns a Foo
    new Bar().foo()  // This returns an Int!
    ```
- 模式匹配问题
1. 不要去匹配一个类型的所有字段，这会让类型增加字段后需要同时修改模式匹配。

    ```
    case class Pokemon(name: String, weight: Int, hp: Int, attack: Int, defense: Int)
    case class Human(name: String, hp: Int)

    // Do NOT do the following, because
    // 1. When a new field is added to Pokemon, we need to change this pattern matching as well
    // 2. It is easy to mismatch the arguments, especially for the ones that have the same data types
    targets.foreach {
    case target @ Pokemon(_, _, hp, _, defense) =>
        val loss = sys.min(0, myAttack - defense)
        target.copy(hp = hp - loss)
    case target @ Human(_, hp) =>
        target.copy(hp = hp - myAttack)
    }

    // Do this:
    targets.foreach {
    case target: Pokemon =>
        val loss = sys.min(0, myAttack - target.defense)
        target.copy(hp = target.hp - loss)
    case target: Human =>
        target.copy(hp = target.hp - myAttack)
    }
    ```
- 匿名方法
  1. 避免过多的括号和大括号
    >> 这个我目前项目的代码就有这个问题
    
    ```
        // Correct
    list.map { item =>
    ...
    }

    // Correct
    list.map(item => ...)

    // Wrong
    list.map(item => {
    ...
    })

    // Wrong
    list.map { item => {
    ...
    }}

    // Wrong
    list.map({ item => ... })
    ```
- 语言特性

1. 样例类避免可变的var 出现.
   之前从java 转scala 的开发者可能不太习惯不可变类型，造成编码出现问题

    ```
    // This is OK
    case class Person(name: String, age: Int)

    // This is NOT OK
    case class Person(name: String, var age: Int)

    // To change values, use the copy constructor to create a new instance
    val p1 = Person("Peter", 15)
    val p2 = p1.copy(age = 16)
    ```

    ```
    case class Report(name:String,age:Int,score:Int,height:Int,weight:Int)

    object Report{

    def updateReport(report:Report,update:Int=>Report=>Report,value:Int)={
        update(value)(report)
    }

    }

    ```

2. apply 方法 应该定义在 object 中 且应该返回  companion class type

    ```
    object TreeNode {
    // This is OK
    def apply(name: String): TreeNode = ...

    // This is bad because it does not return a TreeNode
    def apply(name: String): String = ...
    }
    ```
3. 参数绑定 不要在构造代码块中使用 ，尤其是transient 中
    Destructuring bind (sometimes called tuple extraction)
    ```
    class MyClass {
    // This will NOT work because the compiler generates a non-transient Tuple2
    // that points to both a and b.
    @transient private val (a, b) = someFuncThatReturnsTuple2()
    }
```

4. 传名函数
Avoid using call by name(: => T). Use () => T explicitly

5. return 的使用
- 不要再闭包中使用return //todo 有实践研究一下
- 可以在终止循环中使用return,而不是 flag
6. 异常处理
- 使用 `scala.util.control.NonFatal` 替代 Throwable 或者 Exception
- 不要在API 中使用Try,也就是函数返回值是 Try(T),因为语义不详会导致多层嵌套，直接使用java 风格的trycatch
- 使用Option 替代null