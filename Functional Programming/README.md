[Function Literals](#function-literals)

[Function Values](#function-values)

[Difference Between val and def When Creating Functions](#difference-between-val-and-def-when-creating-functions)

- [Case Expressions](#case-expressions)

- [andThen, compose and toString](#andThen,-compose-and-toString)

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

## Difference Between val and def When Creating Functions

A `def` method is a method in a class while `val` functions are a concrete instances of Function0 through Function22 (objects).

Few notable differences:

[1] `val` functions can be used as case expressions without requiring match keyword.

[2] `val` functions are objects that have methods like andThen, compose and toString.
 
### Case Expressions

Using case expressions with val functions and anonymous classes without requiring the leading match.

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)

val f: Any => Any = {
 case i: Int => println("Int")
 case d: Double => println("Double")
 case _ => println("Other")
}

// Exiting paste mode, now interpreting.

f: Any => Any = $$Lambda$4398/1439582458@20de9fe0

scala> f(1)
Int
res0: Any = ()

scala> f(3.2)
Double
res1: Any = ()
```

### andThen, compose and toString

```sbt
scala> val applyTax = (amount: Double) => amount + 30
applyTax: Double => Double = $$Lambda$4416/753534868@1d55a169

scala> val applyDiscount = (amount: Double) => amount - 10
applyDiscount: Double => Double = $$Lambda$4417/1274252598@607bdefd

scala> f.toString
res3: String = $$Lambda$4398/1439582458@20de9fe0

scala> (applyDiscount compose applyTax)(50.0)
res9: Double = 70.0

scala> (applyDiscount andThen applyTax)(50.0)
res10: Double = 70.0
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