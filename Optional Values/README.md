[Pattern Matching](#pattern-matching)

[map](#map)

[flatMap](#flatMap)

Optional class is an ADT with two possible types:

[1] Some case class wrapping a value

[2] None singleton object

```sbt
scala> Some("Hello World")
res2: Some[String] = Some(Hello World)

scala> None
res3: None.type = None
```

## Pattern Matching

You can handle an optional value with pattern matching.

```sbt

scala> val languages = Map("s" -> "scala", "j" -> "java")
languages: scala.collection.immutable.Map[String,String] = Map(s -> scala, j -> java)

scala> languages.get("s")
res4: Option[String] = Some(scala)

scala> languages.get("r")
res5: Option[String] = None

scala> def findLanguage(s: String): String = {
     |   languages.get(s) match {
     |      case Some(language) => language
     |      case None => s"No language starting with ${s}"
     |   }
     | }
findLanguage: (s: String)String

scala> findLanguage("s")
res8: String = scala
```

## map

Option offers you a set of high order functions for them, allowing you to be more productive and not need to use pattern matching every time.

The function map is a high order function defined on Option that takes a function f as its parameter:

 - If the optional value is present, it applies the function f to it and return it wrapped as optional value;

 - If the optional instance is None, it returns it without applying any function.

```sbt
scala> val languages = Map("s" -> "scala", "j" -> "java")
languages: scala.collection.immutable.Map[String,String] = Map(s -> scala, j -> java)

scala> languages.get("s").map(_.toLowerCase)
res14: Option[String] = Some(scala)

scala> languages.get("k").map(_.toLowerCase)
res19: Option[String] = None
```

This is similar to:

```scala
def map[B](f: A => B): Option[B] =
     this match {   //#A
       case Some(a) => Some(f(a))
       case None => None
     }
```

## flatMap

Option offers you a set of high order functions for them, allowing you to be more productive and not need to use pattern matching every time.

The function map is a high order function defined on Option that takes a function f as its parameter:

 - If the optional value is present, it applies the function f to it. The function f returns an optional value, and you don’t need to wrap the result in an optional value

 - If the optional instance is None, it returns it without applying any function.

```sbt
scala> val languages = Map("s" -> "scala", "j" -> "java")
languages: scala.collection.immutable.Map[String,String] = Map(s -> scala, j -> java)

scala> languages.get("s").flatMap(language => Some(language.toLowerCase))
res17: Option[String] = Some(scala)


scala> languages.get("k").flatMap(language => Some(language.toLowerCase))
res18: Option[String] = None

```

This is similar to:

```scala
def flatten: Option[A] =
     this match {
       case Some(opt) => opt
       case None => None
      }
```
