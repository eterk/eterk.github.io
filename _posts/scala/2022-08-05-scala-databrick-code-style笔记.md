---
layout: post
title:  "scala databricks 编码规范阅读记录"
date:   2022-08-05 07:25:10 +0800
categories: note
tags: scala
---

[原文链接-移步阅读](https://github.com/databricks/scala-style-guide)

### 记录
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
- 使用Option 替代null(For performance sensitive code, prefer null over Option, in order to avoid virtual method calls and boxing. Label the nullable fields clearly with Nullable.)
```
class Foo {
  @javax.annotation.Nullable
  private[this] var nullableField: Bar = _
}
```

- Traversal and zipWithIndex
    1. Use while loops instead of for loops or functional transformations 
`
- Traits and Abstract Classes
    1. 如果定义了具体方法的 trait ,将其改为abstract class .保持java 中可以使用
- 不要使用多参数列表
- Varargs
    1. 谨慎可变参数方法的的重载，当参数为0时会造成语义不详
    2. @scala.annotation.varargs 可以在编译时添加 java 版本的 Array() 方法和 scala 的 Seq 类型的参数
    3. 抽象多参数方法 在java 中不可用
    ```
        class Database {
        @scala.annotation.varargs
        def remove(elems: String*): Unit = ...

         // Adding this will break source compatibility for no-arg remove() call.
        <!-- @scala.annotation.varargs -->
        <!-- def remove(elems: People*): Unit = ... -->
        
        // The following is OK.
        @scala.annotation.varargs
        def remove(elem: People, elems: People*): Unit = ...
        }
    ```

### 个人理解
对整篇编码规范的阅读下来，发现编码规范主要处理以下几方面问题
1. 编译后的代码的稳定性
    类似方法签名在重载过程中导致模棱两可效果
1. 与java 的兼容性
    `abstract class` 替代 `trait`,显式添加`varargs` 标记
1. 代码的可读性
    比如在匿名函数使用中避免过多的 小括号大括号嵌套, Long 类型的字面量使用大写的L 而不是小写的容易和1混淆的l
    编码过程中不同类型变量命名遵顼约定的规范等。apply 方法定义在object中
1. 性能问题
    使用while  替代 map for 等循环，性能敏感区块使用 null 替代None 等
### 以前的笔记
1. 命名约定
2. 1行长度100字符以内
3. 避免使用中缀表示法
4. 避免使用多余的小括号和花括号
5. 实现抽象方法使用oveerride 修饰符
6. 避免使用按名传参
7. 尾递归使用注解@tailrec
8. 不要捕获Throwable 或 Exception 使用 scala.util.control.NonFatal
9. 如果一个值可能为空 使用 Option
10. 使用private[this]
11. 性能（在有性能要求的时候）
a. 使用while 而非for 或者map,foreach ，
b. 使用microbenchmark
c. 使用null 而非 Option
d. 优先使用JAVA 集合
12. java 通用性
a. 默认方法实现的trait 无法在 java中使用，使用 抽象类代替
b. 类型别名
c. 默认参数值
d. 多参数列表
e. 可变参数，使用@scala.annotation.varargs
13. 优先使用 nanoTime,则可以保证是单调递增的
14. 优先使用经过良好测试的方法

编码规范可以理解开发团队在长期的生产实践中总结的经验，遵循这些规范可以避免在后续的团队开发协作中形成统一风格，避坑