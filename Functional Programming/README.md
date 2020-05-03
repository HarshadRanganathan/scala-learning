[Function Literals](#function-literals)

[Function Values](#function-values)

[flatMap](#flatMap)

[filter](#filter)

## Function Literals

Function literal is an anonymous function.

Useful to pass function as an argument to a method.

`_` stands for one function parameter.

```sbt
scala> val numbers = Vector(1,2,3,4,5,6)
numbers: scala.collection.immutable.Vector[Int] = Vector(1, 2, 3, 4, 5, 6)

scala> numbers.map((number: Int) => number + 1)
res2: scala.collection.immutable.Vector[Int] = Vector(2, 3, 4, 5, 6, 7)

scala> numbers.map(number => number + 1)
res3: scala.collection.immutable.Vector[Int] = Vector(2, 3, 4, 5, 6, 7)

scala> numbers.map(_ + 1)
res4: scala.collection.immutable.Vector[Int] = Vector(2, 3, 4, 5, 6, 7)
```

## Function Values

Scala has first class functions. Function values are objects.

Assign function values to variables.

Pass function values as arguments to higher order functions.

Standard library contains Function1 to Function22.

All function types define an apply method.

```sbt
scala> val add: Int => Int = number => number + 1
add: Int => Int = $$Lambda$4460/954248976@4e990cf

scala> numbers.map(add)
res7: scala.collection.immutable.Vector[Int] = Vector(2, 3, 4, 5, 6, 7)

scala> val add = (number: Int) => number + 1
add: Int => Int = $$Lambda$4467/2084983604@6604fab6

scala> numbers.map(add)
res8: scala.collection.immutable.Vector[Int] = Vector(2, 3, 4, 5, 6, 7)
```

## Methods as Functions

If the compiler expects a function but you give a method then the method gets lifted into a function.

```sbt
scala> def add(number: Int): Int = number +1
add: (number: Int)Int

scala> val f: Int => Int = add
f: Int => Int = $$Lambda$4468/1702400130@643410ef

scala> val f = add
               ^
       error: missing argument list for method add
       Unapplied methods are only converted to functions when a function type is expected.
       You can make this conversion explicit by writing `add _` or `add(_)` instead of `add`.

scala> val f = add _
f: Int => Int = $$Lambda$4469/1182719231@3a88e861

scala> numbers.map(f)
res9: scala.collection.immutable.Vector[Int] = Vector(2, 3, 4, 5, 6, 7)
```

## flatMap

Expands each element of collection into the result.

```sbt
scala> val v = Vector("Hello World", "Scala Learning")
v: scala.collection.immutable.Vector[String] = Vector(Hello World, Scala Learning)

scala> v.map(str => str.split(" "))
res1: scala.collection.immutable.Vector[Array[String]] = Vector(Array(Hello, World), Array(Scala, Learning))

scala> v.map(str => str.split(" ")).flatten
res3: scala.collection.immutable.Vector[String] = Vector(Hello, World, Scala, Learning)

scala> v.flatMap(str => str.split(" "))
res4: scala.collection.immutable.Vector[String] = Vector(Hello, World, Scala, Learning)
```

## filter

predicate function returning a boolean value.

```sbt
scala> val numbers = Vector(1,2,3)
numbers: scala.collection.immutable.Vector[Int] = Vector(1, 2, 3)

scala> numbers.filter(_ % 2 == 0)
res0: scala.collection.immutable.Vector[Int] = Vector(2)

scala> numbers.filterNot(_ % 2 == 0)
res1: scala.collection.immutable.Vector[Int] = Vector(1, 3)
```