[Vector](#vector)

[Sequence](#sequence)

[Set](#set)

[List](#list)

[Tuple](#tuple)

[Map](#map)

[Collection Methods](#collection-methods)

- [Concatenate](#concatenate)

- [Conversion](#conversion)

- [Size](#size)

- [Contains](#contains)

- [Retrieve Elements](#retrieve-elements)

- [Add Elements](#add-elements)

- [Zip](#zip)

- [GroupBy](#groupBy)

## Vector

```sbt
Vector(1,2,3)
res0: scala.collection.immutable.Vector[Int] = Vector(1, 2, 3)
```

## Sequence

```sbt
scala> Seq(1,2,3)
res1: Seq[Int] = List(1, 2, 3)

scala> Seq(1,2,"3")
res2: Seq[Any] = List(1, 2, 3)
```

## Set

Collection that contains no duplicates and is unordered.

```sbt
scala> Set(1,2,3)
res3: scala.collection.immutable.Set[Int] = Set(1, 2, 3)

scala> Set(1,2,'a')
res4: scala.collection.immutable.Set[Int] = Set(1, 2, 97)

scala> Set(1,2,"a")
res5: scala.collection.immutable.Set[Any] = Set(1, 2, a)
```

## List

```sbt
scala> List(1,2,3)
res6: List[Int] = List(1, 2, 3)
```

## Tuples

Standard library contains Tuple1 to Tuple22.

```sbt
scala> (1, "a")
res30: (Int, String) = (1,a)

scala> Tuple2(1, "a")
res31: (Int, String) = (1,a)

scala> 1 -> "a"
res32: (Int, String) = (1,a)

scala> 1 -> "b" -> "a"
res33: ((Int, String), String) = ((1,b),a)
```

## Map

```sbt
scala> val m = Map(1 -> "a", 2 -> "b")
m: scala.collection.immutable.Map[Int,String] = Map(1 -> a, 2 -> b)
```

## Collection Methods

### ++

Concatenate collections

```sbt
scala> val l1 = List(1,2,3)
l1: List[Int] = List(1, 2, 3)

scala> val l2 = List(4,5,6)
l2: List[Int] = List(4, 5, 6)

scala> l1 ++ l2
res7: List[Int] = List(1, 2, 3, 4, 5, 6)
```

### Conversion

```sbt
scala> List(1,2,3).toSet
res10: scala.collection.immutable.Set[Int] = Set(1, 2, 3)
```

### Size

```sbt
scala> List(1,2,3).size
res11: Int = 3

scala> List(1,2,3).isEmpty
res12: Boolean = false
```

### Contains

```sbt
scala> List(1,2,3).contains(2)
res13: Boolean = true

scala> List(1,2,3).contains("a")
res14: Boolean = false
```

### Retrieve Elements

```sbt
scala> List(1,2,3).head
res15: Int = 1

scala> List(1,2,3).tail
res16: List[Int] = List(2, 3)

scala> List().head
java.util.NoSuchElementException: head of empty list
  at scala.collection.immutable.Nil$.head(List.scala:592)
  at scala.collection.immutable.Nil$.head(List.scala:591)
  ... 36 elided

scala> List().headOption
res18: Option[Nothing] = None

scala> List(1,2,3).headOption
res19: Option[Int] = Some(1)

scala> List(1,2,3).take(2)
res20: List[Int] = List(1, 2)

scala> List().take(2)
res21: List[Nothing] = List()

scala> val m = Map(1 -> "a", 2 -> "b")
m: scala.collection.immutable.Map[Int,String] = Map(1 -> a, 2 -> b)

scala> m.get(0)
res34: Option[String] = None

scala> m.get(1)
res35: Option[String] = Some(a)

scala> val pack = Tuple5(1,2,3,4,5)
pack: (Int, Int, Int, Int, Int) = (1,2,3,4,5)

scala> pack._5
res6: Int = 5
```

### Add Elements

```sbt
scala> List(1,2,3) :+ 4
res26: List[Int] = List(1, 2, 3, 4)

scala> 0 +: List(1,2,3)
res27: List[Int] = List(0, 1, 2, 3)

scala> Set(1,2,3) + 4
res29: scala.collection.immutable.Set[Int] = Set(1, 2, 3, 4)
```

### Zip

```sbt
scala> val l1 = List(1,2,3)
l1: List[Int] = List(1, 2, 3)

scala> val l2 = List("a","b","c")
l2: List[String] = List(a, b, c)

scala> l1 zip l2
res22: List[(Int, String)] = List((1,a), (2,b), (3,c))
```

### GroupBy

```sbt
scala> List(1,2,3,4,5,6).groupBy(i => i%2 == 0)
res24: scala.collection.immutable.Map[Boolean,List[Int]] = HashMap(false -> List(1, 3, 5), true -> List(2, 4, 6))
```