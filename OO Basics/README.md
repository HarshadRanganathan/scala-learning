[Class](#class)

- [Class Parameters](#class-parameters)

- [Additional Constructors](#additional-constructors)

- [Immutable Fields](#immutable-fields)

- [Mutable Fields](#mutable-fields)

- [Promote Class Parameters to Fields](#promote-class-parameters-to-fields)

- [Methods](#methods)

  - [Default Arguments](#default-arguments)

- [Operators](#operators)

  - [Infix Operators](#infix-operators)

  - [Prefix Operators](#prefix-operators)

  - [Define Operators](#define-operators)

- [Equality](#equality)

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
