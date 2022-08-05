layout: post
title:  "scala databricks 编码规范阅读记录"
date:   2022-08-05 07:25:10 +0800
categories: note
tags: scala
---



本文只记录一些作者本人之前不太了解的编码规范
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
