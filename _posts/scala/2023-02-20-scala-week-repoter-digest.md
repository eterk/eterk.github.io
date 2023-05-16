---
layout: post
title:  "scala 周报阅读摘要"
date:   2023-02-20 07:25:10 +0800
categories: note
tags: scala
---

### Enum 的一些实现

```scala

# option 1

sealed trait OrderStatus {
    def id: String 
}

object OrderStatus {
  case object Draft extends OrderStatus {
    def id: String = "DRAFT"
  }
  case object Submitted extends OrderStatus {
    def id: String = "SUBMITTED"
  }
  case object Delivered extends OrderStatus {
    def id: String = "DELIVERED"
  }
}

# otion 2
sealed trait OrderStatus {
    import OrderStatus._

    def id: String = this match {
      case Draft     => "DRAFT"
      case Submitted => "SUBMITTED"
      case Delivered => "DELIVERED"
    }
}

object OrderStatus {
  case object Draft     extends OrderStatus
  case object Submitted extends OrderStatus
  case object Delivered extends OrderStatus
}

# otion 3
sealed abstract class OrderStatus(val id: String)

object OrderStatus {
  case object Draft     extends OrderStatus
  case object Submitted extends OrderStatus
  case object Delivered extends OrderStatus
}


# otion 4
enum OrderStatus(val id: String) {
  case Draft     extends OrderStatus("DRAFT")
  case Submitted extends OrderStatus("SUBMITTED")
  case Delivered extends OrderStatus("DELIVERED")
}
```