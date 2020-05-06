
[Wildcard Pattern](#wildcard-pattern)

[Variable Pattern](#variable-pattern)

[Typed Pattern](#typed-pattern)

[Constant Pattern](#constant-pattern)

[Stable Identifiers](#stable-identifiers)

[Tuple Pattern](#tuple-pattern)

[Constructor Pattern](#constructor-pattern)

[Sequence Pattern](#sequence-pattern)

[Pattern Alternatives](#pattern-alternatives)

[Pattern Binders](#pattern-binders)

[Pattern Guards](#pattern-guards)

[Vals With Patterns](#vals-with-patterns)

[Patterns in Generators](#patterns-in-generators)

Expressions are matched against a pattern.

Match expressions return a value and there is no fallthrough.

`MatchError` is thrown if no pattern matches.

```text
expr match {
  case pattern1 => result1
  case pattern2 => result2
}
```

## Wildcard Pattern

Use _ as wildcard to match everything.

```sbt
:paste
// Entering paste mode (ctrl-D to finish)

def matching(any: Any): String = any match {
   case _ => s"$any is no time"
}

// Exiting paste mode, now interpreting.

matching: (any: Any)String

scala> matching(1)
res0: String = 1 is no time
```

## Variable Pattern

Use an identifier to capture the value.

```sbt
:paste
// Entering paste mode (ctrl-D to finish)

def matching(any: Any): String = any match {
   case x => s"$x is no time"
}

// Exiting paste mode, now interpreting.

matching: (any: Any)String

scala> matching(1)
res0: String = 1 is no time
```

## Typed Pattern

Match on certain types.

```sbt
:paste
// Entering paste mode (ctrl-D to finish)

def matching(any: Any): String = any match {
   case time: Time => s"It is ${time.hours}:${time.minutes}"
   case _ => s"$any is no time"
}

// Exiting paste mode, now interpreting.

matching: (any: Any)String

scala> matching(Time(1, 30))
res0: String = It is 1:30
```

## Constant Pattern

Use stable identifier to match a constant.

```sbt
:paste
// Entering paste mode (ctrl-D to finish)

def matching(any: Any): String = any match {
   case "12:00" => s"It is noon"
   case _ => s"$any is no time"
}

// Exiting paste mode, now interpreting.

matching: (any: Any)String

scala> matching("12:00")
res0: String = It is noon
```

## Stable Identifiers

Stable identifiers are literals, identifiers for vals/singleton objects enclosed in backticks.

```sbt
:paste
// Entering paste mode (ctrl-D to finish)

val highNoon = "12:00"

def matching(any: Any): String = any match {
  case `highNoon` => s"It is Noon"
  case _ => s"${any} is not a time"
}

// Exiting paste mode, now interpreting.

highNoon: String = 12:00
matching: (any: Any)String

scala> matching("12:00")
res0: String = It is Noon
```

## Tuple Pattern

Use tuple syntax to match and decompose tuples.

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)

def matching(any: Any): String = any match {
  case (x, "12:00") => s"It is Noon"
  case _ => s"${any} is not a time"
}

// Exiting paste mode, now interpreting.

matching: (any: Any)String

scala> matching(("dusk", "12:00"))
res1: String = It is Noon
```

## Constructor Pattern

Use constructor syntax to match and decompose case classes.


```sbt
:paste
// Entering paste mode (ctrl-D to finish)

def matching(any: Any): String = any match {
   case Time(12, 0) => s"High Noon"
   case _ => s"$any is no time"
}

// Exiting paste mode, now interpreting.

matching: (any: Any)String

scala> matching(Time(12, 0))
res0: String = High Noon
```

## Sequence Pattern

Use sequence constructors, prepend or append operators to match and decompose sequences.

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)

def matching[A](seq: Seq[A]): String = seq match {
 case Seq(1, 2, 3) => "1 to 3"
 case x +: Nil => s"Only element is $x"
 case _ :+ x => s"Last element is $x"
 case Nil => "Empty sequence"
}

// Exiting paste mode, now interpreting.

matching: [A](seq: Seq[A])String

scala> matching(Seq(1, 2, 3))
res3: String = 1 to 3

scala> matching(1 to 3)
res4: String = 1 to 3

scala> matching(Vector(1))
res5: String = Only element is 1

scala> matching(Vector(1, 2, 3))
res6: String = 1 to 3

scala> matching(Vector(1, 2, 3, 4))
res7: String = Last element is 4
```

## Pattern Alternatives

```sbt
:paste
// Entering paste mode (ctrl-D to finish)

def matching(any: Any): String = any match {
   case "00:00" | "12:00" => s"Midnight or noon"
   case _ => s"$any is no time"
}

// Exiting paste mode, now interpreting.

matching: (any: Any)String

scala> matching("12:00")
res0: String = Midnight or noon
```

## Pattern Binders

Use `@` to bind a pattern to a variable.

```sbt
:paste
// Entering paste mode (ctrl-D to finish)

def matching(any: Any): String = any match {
   case hour @ Time(_, 0) => s"${hour} with 0 minutes"
   case hour @ Time(_, m) => s"${hour} with ${m} minutes"
   case _ => s"$any is no time"
}

// Exiting paste mode, now interpreting.

matching: (any: Any)String

scala> matching(Time(12, 0))
res0: String = 12 with 0 minutes
```

## Pattern Guards

Use `if` keyword to define a pattern guard.

```sbt
:paste
// Entering paste mode (ctrl-D to finish)

def matching(any: Any): String = any match {
   case Time(h, m) if h >= 12 => s"It is ${h}:{m} pm"
   case Time(h, m) => s"It is ${h}:{m} am"
   case _ => s"$any is no time"
}

// Exiting paste mode, now interpreting.

matching: (any: Any)String

scala> matching(Time(12, 00))
res0: String = It is 12:00 pm
```

## Vals With Patterns

Use patterns to define vals.

```sbt
scala> val (h, m) = (12, 0)
h: Int = 12
m: Int = 0
```

## Patterns in Generators

Patterns can simplify generators involving tuples.

```sbt
scala> for((m, n) <- Vector(1 -> 2, 2 -> 3)) yield m + n
res0: scala.collection.immutable.Vector[Int] = Vector(3, 5)
```