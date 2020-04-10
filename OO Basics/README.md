[Class](#class)

- [Class Parameters](#class-parameters)

- [Additional Constructors](#additional-constructors)

- [Immutable Fields](#immutable-fields)

- [Mutable Fields](#mutable-fields)

- [Promote Class Parameters to Fields](#promote-class-parameters-to-fields)

- [Methods](#methods)

  - [Default Arguments](#default-arguments)

- [Qualified Access Modifiers](#qualified-access-modifiers)

[Operators](#operators)

- [Infix Operators](#infix-operators)

- [Prefix Operators](#prefix-operators)

- [Define Operators](#define-operators)

[Equality](#equality)

[Singleton Objects](#singleton-objects)

- [Companion Objects](#companion-objects)

[Predef](#predef)

- [require](#require)

[Case Classes](#case-classes)

## Class

Defined with `class` keyword.

Class instance is created with `new` keyword.

- `AnyRef` is used if no superclass is extended explicitly.

- Each class gets a primary constructor automatically.

```sbt
scala> class Hello
defined class Hello

scala> val hello = new Hello
hello: Hello = Hello@31f5e580

scala> hello.toString
res9: String = Hello@31f5e580
```

### Class Parameters

Classes can have one or more parameters.

Class parameters are parameters of the primary constructor.

```sbt
scala> class HelloWorld(message: String) {
     |  print(message)
     | }
defined class HelloWorld

scala> val helloWorld = new HelloWorld
                        ^
       error: not enough arguments for constructor HelloWorld: (message: String)HelloWorld.
       Unspecified value parameter message.

scala> val helloWorld = new HelloWorld("Hello World")
Hello World
helloWorld: HelloWorld = HelloWorld@7fed3d50
```

### Additional Constructors

Use `def` followed by `this` to define additional constructors. In the end, primary constructor needs to be called.

```sbt
scala> class HelloWorld(message: String) {
     |  def this() = this("Hello World")
     |  print(message)
     | }
defined class HelloWorld

scala> var helloWorld = new HelloWorld
Hello World
helloWorld: HelloWorld = HelloWorld@263d93e6

scala> var helloWorld = new HelloWorld("Hello World")
Hello World
helloWorld: HelloWorld = HelloWorld@226e5812
```

### Immutable Fields

Fields are class members keeping state.

```sbt
scala> class HelloWorld {
     |  val message: String = "HelloWorld"
     | }
defined class HelloWorld

scala> new HelloWorld message
res0: String = HelloWorld

scala> (new HelloWorld).message
res1: String = HelloWorld
```

Immutable fields get compiled into

- private final field
- public getter method

```sbt
scala> :javap -p HelloWorld
Compiled from "<console>"
public class $line5.$read$$iw$$iw$HelloWorld {
  private final java.lang.String message;
  public java.lang.String message();
  public $line5.$read$$iw$$iw$HelloWorld();
}
```

### Mutable Fields

Define mutable fields using `var` keyword.

```sbt
scala> class HelloWorld {
     |  var message: String = "HelloWorld"
     | }
defined class HelloWorld

scala> val helloWorld = new HelloWorld
helloWorld: HelloWorld = HelloWorld@32502ed0

scala> helloWorld.message
res5: String = HelloWorld

scala> helloWorld.message = "Hi!"
mutated helloWorld.message

scala> helloWorld.message
res7: String = Hi!
```

Mutable fields get compiled into

- private field
- public getter method
- public setter method

```sbt
scala> :javap -p HelloWorld
Compiled from "<console>"
public class $line8.$read$$iw$$iw$HelloWorld {
  private java.lang.String message;
  public java.lang.String message();
  public void message_$eq(java.lang.String);
  public $line8.$read$$iw$$iw$HelloWorld();
}
```

### Promote Class Parameters to Fields

Add `val` or `var` before class parameter. This,

- creates a field
- Initializes the field with the value

```sbt
scala> class HelloWorld(message: String)
defined class HelloWorld

scala> val helloWorld = new HelloWorld("Hello World")
helloWorld: HelloWorld = HelloWorld@65662e3f

scala> helloWorld.message
                  ^
       error: value message is not a member of HelloWorld

scala> class HelloWorld(val message: String)
defined class HelloWorld

scala> val helloWorld = new HelloWorld("Hello World")
helloWorld: HelloWorld = HelloWorld@7302de5b

scala> helloWorld.message
res11: String = Hello World
```

### Methods

Methods are class members providing operations.

Use `def` to define a method followed by:

- Name
- Optional parameter list
- Optional type annotation
- Expression

Let's define a `minus` method for class `Time` which calculates difference between two times.

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)

class Time(val hours: Int, val minutes: Int) {
 val asMinutes: Int = hours * 60 + minutes

 def minus(that: Time): Int = asMinutes - that.asMinutes
}


// Exiting paste mode, now interpreting.

defined class Time

scala> val time1 = new Time(10, 10)
time1: Time = Time@60b43986

scala> val time2 = new Time(1, 1)
time2: Time = Time@7c0a0d1e

scala> time1.minus(time2)
res2: Int = 549
```

#### Default Arguments

```sbt
scala> class Time(val hours: Int = 0, val minutes: Int = 0)
defined class Time

scala> new Time().hours
res9: Int = 0

scala> new Time(1).hours
res10: Int = 1

scala> new Time(minutes=5).hours
res11: Int = 0
```

### Qualified Access Modifiers

Relax access up to a given entity through a qualifier.

Use `this` to restrict access to the instance.

```scala
package com.rharshad.train

// relax access up to package `train`
private[train] def minus(that: Time): Int = asMinutes - that.asMinutes

// restrict access to this instance
private[this] def minus(that: Time): Int = asMinutes - that.asMinutes
```

## Operators

Operators are methods used in operator notation.

- Dots and parentheses can be omitted

### Infix Operators

Methods with one parameter.

```sbt
scala> "Harshad Ranganathan".split(" ")
res3: Array[String] = Array(Harshad, Ranganathan)
```

### Prefix Operators

`+,-,!,~` can be used in prefix notation.

```sbt
scala> !true
res4: Boolean = false
```

### Define Operators

You can define operators and call them as methods.

Let's define `-` operator method in Time class.

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)

class Time(val hours: Int, val minutes: Int) {
  val asMinutes: Int = hours * 60 + minutes

  def minus(that: Time): Int = asMinutes - that.asMinutes

  def -(that: Time): Int = minus(that)
}

// Exiting paste mode, now interpreting.

defined class Time

scala> val time1 = new Time(10, 10)
time1: Time = Time@68227b67

scala> val time2 = new Time(1, 1)
time2: Time = Time@11e1c14a

scala> time1 - time2
res0: Int = 549
```

## Equality

Use `==` for equality and `!=` for inequality.

No difference between primitive and reference types.

Use `eq` or `ne` for checking identity.

Comparisons are `null-safe`.

```sbt
scala> 1 == 1
res3: Boolean = true

scala> new String("Harshad") == new String("Harshad")
res4: Boolean = true
```

## Singleton Objects

Create only one instance of class.

Access the object by it's name.

Singletons are first-class objects replacing static.

Use Cases:

- Companion Objects
- Util
- Applications

```sbt
scala> object HelloWorld {
     |   val message = "Hello World"
     | }
defined object HelloWorld

scala> HelloWorld.message
res1: String = Hello World
```

### Companion Objects

If a singleton object and a class/trait share the same name, package and file, then they are called companions.

Companions can access their private members.

In below example, we have a companion object Time with method `fromMinutes` which returns an instance of Time.

```scala
package com.rharshad.train

object Time {
  def fromMinutes(m: Int): Time = {
    val hours = m / 60
    val minutes = m % 60
    new Time(hours, minutes)
  }
}

class Time(val hours: Int = 0, val minutes: Int = 0) {
  val asMinutes: Int = hours * 60 + minutes

  def minus(that: Time): Int = asMinutes - that.asMinutes

  def -(that: Time): Int = minus(that)
}
```

```sbt
scala> val time = Time.fromMinutes(90)
time:com.rharshad.train.Time = com.rharshad.train.Time@79d4ed27

scala> time.hours
res0: Int = 1

scala> time.minutes
res1: Int = 30
```

## Predef

Standard library contains predef singleton object.

All of it's members are imported automatically.

### require

`require` method is used to check for preconditions.

```scala
class Time(val hours: Int = 0, val minutes: Int = 0) {

  require(hours >= 0 && hours < 24, "Invalid hours")
  require(minutes >= 0 && minutes < 60, "Invalid minutes")

  val asMinutes: Int = hours * 60 + minutes

  def minus(that: Time): Int = asMinutes - that.asMinutes

  def -(that: Time): Int = minus(that)
}
```

## Case Classes

You create case classes using `case` keyword.

`case` keyword adds several features:

- Create instances without `new` keyword

- Each case class has a companion object with `apply` method which creates an instance with `new` keyword

- Compiler creates toString, equals & hashCode implementations

- Classes are promoted to immutable fields

- `copy` method is added

- Pattern matching

Let's create a simple case class.

```scala
case class Train(kind: String, number: Int)
```

We decompile the case class to see the auto generated methods:

```sbt
scala> :javap -p Train
Compiled from "Train.scala"
public class com.rharshad.scalatrain.Train implements scala.Product,java.io.Serializable {
  private final java.lang.String kind;
  private final int number;
  public static scala.Option<scala.Tuple2<java.lang.String, java.lang.Object>> unapply(com.rharshad.scalatrain.Train);
  public static com.rharshad.scalatrain.Train apply(java.lang.String, int);
  public static scala.Function1<scala.Tuple2<java.lang.String, java.lang.Object>, com.rharshad.scalatrain.Train> tupled();
  public static scala.Function1<java.lang.String, scala.Function1<java.lang.Object, com.rharshad.scalatrain.Train>> curried();
  public scala.collection.Iterator<java.lang.String> productElementNames();
  public java.lang.String kind();
  public int number();
  public com.rharshad.scalatrain.Train copy(java.lang.String, int);
  public java.lang.String copy$default$1();
  public int copy$default$2();
  public java.lang.String productPrefix();
  public int productArity();
  public java.lang.Object productElement(int);
  public scala.collection.Iterator<java.lang.Object> productIterator();
  public boolean canEqual(java.lang.Object);
  public java.lang.String productElementName(int);
  public int hashCode();
  public java.lang.String toString();
  public boolean equals(java.lang.Object);
  public com.rharshad.scalatrain.Train(java.lang.String, int);
}
```

As you can see it has several methods added such as `apply`, `toString`, `copy` etc.

Let's see how useful these features are.

```sbt
// create instance without new keyword
scala> val train = Train("CARGO", 2)
// toString shows instance data instead of object reference
train: com.rharshad.scalatrain.Train = Train(CARGO,2)

// creates new instance with updated value
scala> train.copy(number = 3)
res0: com.rharshad.scalatrain.Train = Train(CARGO,3)
```

Why not all classes are case classes ?

- Overhead in bytecode size

- Cannot inherit a case class from another one

Case classes are best suited for value objects/