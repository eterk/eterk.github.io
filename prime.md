### 使用Sieve_of_Eratosthenes 计算素数





最终目标是 个人电脑（16G内存 CPU 10代I7）实现 1到100亿以内的素数按顺序的IO输出。[2022-08-02]



#### 2022年8月2日

初步思路是先写出可以计算一定范围的素数的函数，然后对100亿进行分段生成素数，然后分区写出所有素数；

`Int=>Int` 计算 正整数 n以内素数的个数

   根据[Sieve of Eratosthenes - Wikipedia](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) 的伪代码实现了我自己的初始版本

```
# scala
def countPrimes(n: Int): Int = {
      val container = collection.mutable.Seq.fill(n)(true)
      var i = 2
      while (i * i < n) {
        if (container(i)) {
          var j = i * i
          while (j < n) {
            container.update(j, false)
            j += i
          }
        }
        i += 1
      }
      (2 until n).count(container)
    }
```

对算法进行简单分析，将两次循环解耦得到第二个版本

```
    def countPrimesV1(n: Int): Int = {
      val container = collection.mutable.Seq.fill(n)(true)
      val seqI = 2 to Math.floor(Math.sqrt(n)).toInt
      val validSeqI =
        seqI.filter(i => {
          val lower = seqI.filter(j => i > j)
          !lower.exists(k => (i % k) == 0)
        })
      validSeqI.foreach(i => {
        var next = i * i
        while (next < n) {
          if (container(next)) {
            container.update(next, false)
          }
          next += i
        }
      })
      (2 until n).count(container)
    }
```

使用scala 比较管用的集合函数api 写了一个版本，太慢了

```scala
	import java.util.Math
    def flatMapSet(n: Int): Int = {
      val seqI = (2 to Math.floor(Math.sqrt(n)).toInt)
      val container =
        seqI.filter(i => {
          val lower = seqI.filter(j => i > j)
          !lower.exists(k => (i % k) == 0)
        })
          .flatMap(i => {
            i * i to n by i
          })
          .toSet
      n - container.size - 1
    }
```



使用其它的数据结构复刻了一下第一个版本，效率都远远第一第一个版本

```scala 
def seqPar(n: Int): Int = {
      val container = collection.mutable.Seq.fill(n)(true).par
      val seqI = 2 to Math.floor(Math.sqrt(n)).toInt
      val validSeqI =
        seqI.filter(i => {
          val lower = seqI.filter(j => i > j)
          !lower.exists(k => (i % k) == 0)
        })
      validSeqI.foreach(i => {
        var next = i * i
        while (next < n) {
          if (container(next)) {
            container.update(next, false)
          }
          next += i
        }
      })
      (2 until n).count(x => container(x))
    }

    def set(n: Int): Int = {
      val container = collection.mutable.Set.empty[Int]
      val seqI = 2 to Math.floor(Math.sqrt(n)).toInt
      val validSeqI =
        seqI.filter(i => {
          val lower = seqI.filter(j => i > j)
          !lower.exists(k => (i % k) == 0)
        })
      validSeqI.foreach(i => {
        var next = i * i
        while (next < n) {
          container.add(next)
          next += i
        }
      })
      n - container.size - 2
    }



    def map(n: Int): Int = {
      val container = collection.mutable.Map((2 to n).map(x => (x, true)): _*)
      var i = 2
      while (i * i < n) {
        if (container(i)) {
          var j = i * i
          while (j < n) {
            container.update(j, false)
            j += i
          }
        }
        i += 1
      }
      (2 until n).count(container)
    }
```

在写计算素数的函数个数中简单了解了一下 使用牛顿迭代法计算平方根的算法

下面是代码实现

``` scala
  
/**
* v :需要计算平方根的数
* digit ：表示具体精度
**/
def sqrt(v: Int, digit: Int) = {
    val min = Math.pow(10, -digit)

    @tailrec
    def sqrt_(r: Double): Double = {
      if (min > ((r + v / r) / 2 - r).abs) return r
      sqrt_((r + v / r) / 2)
    }

    sqrt_(v)
  }
```

