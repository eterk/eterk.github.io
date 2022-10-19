---
layout: post
title:  "scalaTest 文档阅读笔记"
date:   2022-10-17 07:25:10 +0800
categories: note
tags: scala
---

(阅读文档后的笔记)[https://www.scalatest.org/user_guide]
### 风格
scalatest 有若干测试风格的接口,选择适合自己项目的接口即可

无论选择哪一种类型的接口,都应该在接口名称中表达好自己测试用例的简要说明

### 使用自己的测试接口
将常用的接口对象就行封装,然后使得自己常用的接口 不用多次重复引用.
```scala
package com.mycompany.myproject

import org.scalatest._
import flatspec._
import matchers._

abstract class UnitSpec extends AnyFlatSpec with should.Matchers with
  OptionValues with Inside with Inspectors
```
### 使用断言


- `assume` to conditionally cancel a test;
- `fail` to fail a test unconditionally;
- `cancel` to cancel a test unconditionally;
- `succeed` to make a test succeed unconditionally;
- `intercept` to ensure a bit of code throws an expected exception and then make assertions about the exception;
- `assertDoesNotCompile` to ensure a bit of code does not compile;
- `assertCompiles` to ensure a bit of code does compile;
- `assertTypeError` to ensure a bit of code does not compile because of a type (not parse) error;
- `withClue` to add more information about a failure.

`ScalaTest` 有自己的断言接口 `org.scalatest.Assertions`
`Assertions` 中的`assert` 覆盖了 PreDef 中的`assert`,抛出TestFailedException 替换了AssertionError,有次提供了更多信息.assert 使用了 marco 的技术

提供了`===` 操作符 可以自定义 对象相等性

```scala
# 约等效于assert((a-b)==2)
val ```a = 5
val b = 2
assertResult(2) {
  a - b
}
```
`assertTypeError`,`assertDoesNotCompile`,`assertCompiles` 这几类断言只能处理字符串内的代码

使用assume 来决定测试是否正常跑

`assume(database.isAvailable)`

### 标记你的测试
使用这个方式调用测试
`(new StackSpec).execute()`
`scalaTest`可以使用`ignore` 标记来标记不需要的测试

创建自己的Tag 
```
import org.scalatest.Tag
object DbTest extends Tag("com.mycompany.tags.DbTest")
```

可以根据标签决定测试的时候使用那些标签
跑测试的方法

1. From sbt	use ScalaTest's Framework
2. From Maven use the ScalaTest Maven Plugin
3. On the command line	use the Runner
4. In the Scala interpreter	invoke execute or use the ScalaTest shell
5. Via Ant	use the ScalaTestAntTask
6. From IntelliJ IDEA use the ScalaTest support in Scala plugin
7. From the Scala IDE for Eclipse	use the ScalaTest Eclipse plugin
8. From NetBean use JUnitRunner

### Sharing fixtures
当一些固定的代码需要多个测试共享的时候,就需要
- 重构代码
  1. 提供获取固定对象的方法 `extract`
  2. 固定接口的对象
  3. 使用`loan pattern`
- 重写`withFixture` 
   
- 混入`before-and-after` 接口来减少代码重复

### 共享测试
有时候需要对不同的固定对象跑同样的测试

### 使用`matchers`
定义了一堆比较用的东西,比较花哨
`import org.scalatest.matchers.should.Matchers._`

### 使用`Mock`
`scalaTest` 支持一些Mock 库
`org.scalamock.scalatest.MockFactory` 混入自己定义的测试类
- 函数`mock`
```
val m = mockFunction[Int, String]
m expects (42) returning "Forty two" once
```
- 接口`mock`
  只支持`trait` 或者`interface`
- 通用`mock`
