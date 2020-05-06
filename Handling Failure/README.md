[Try](#try)

You can use try/catch in scala to handle failures.

```sbt
scala> def toInt(s: String): Int =
     |  try {
     |    s.toInt
     |  } catch {
     |    case _: NumberFormatException => 0
     |  }
toInt: (s: String)Int

scala> toInt("1")
res27: Int = 1

scala> toInt("s")
res28: Int = 0
```

## Try

```sbt
scala> import scala.util.{Try,Success,Failure}
import scala.util.{Try, Success, Failure}

scala> Try("100".toInt)
res30: scala.util.Try[Int] = Success(100)

scala> Try("s".toInt)
res31: scala.util.Try[Int] = Failure(java.lang.NumberFormatException: For input string: "s")

scala> Try("s".toInt) match {
     |   case Success(num) => num
     |   case Failure(_) => 0
     | }
res32: Int = 0
```

You can chain operations, catching exceptions along the way. `map` & `flatMap` can be called on Failure.

Only `NonFatal` exceptions are caught by Try.

```sbt
scala> Try("s".toInt).map(number => number.toString)
res33: scala.util.Try[String] = Failure(java.lang.NumberFormatException: For input string: "s")

scala> Try("1".toInt).map(number => number.toString)
res34: scala.util.Try[String] = Success(1)
```