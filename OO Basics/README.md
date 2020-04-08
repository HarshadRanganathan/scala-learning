[Class](#class)

 - [Class Parameters](#class-parameters)

 - [Additional Constructors](#additional-constructors)

 - [Immutable Fields](#immutable-fields)

 - [Mutable Fields](#mutable-fields)

 - [Promote Class Parameters to Fields](#promote-class-parameters-to-fields)

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