
[Inheritance](#inheritance)

- [Subclass](#subclass)

    - [Inherited Method](#inherited-method)

    - [Superclass Constructor](#superclass-constructor)

    - [Overriding](#overriding)

    - [Accessing Super Class Members](#accessing-super-class-members)
  
- [Final Classes](#final-classes)

- [Sealed Classes](#sealed-classes)

- [Abstract Classes](#abstract-classes)

- [Traits](#traits)

     - [Mix-in Rules](#mix-in-rules)

          - [Inheritance Hierarchy](#inheritance-hierarchy)

          - [Concrete Members](#concrete-members)

## Inheritance

Each class, except for Any, has exactly one superclass.

- Non private members are not inherited

- Non final members can be overridden

### Subclass

`extends` defines a subclass.

`AnyRef` is used as superclass if you omit `extends`.

You can only extend exactly one class.

```sbt
scala> class Animal
defined class Animal

scala> class Bird extends Animal
defined class Bird
```

#### Inherited Method

```sbt
scala> class Animal {
     |  def eat() = println("eat")
     | }
defined class Animal

scala> class Bird extends Animal {
     | eat()
     | }
defined class Bird

scala> val b = new Bird
eat
b: Bird = Bird@3b725066

scala> b.eat
eat
```

#### Superclass Constructor

Subclass must call superclass constructor with extends.

Superclasses are initialized first.

```sbt
scala> class Animal(name: String) {
     |   def eat() = println("eat")
     | }
defined class Animal

scala> class Bird(name: String) extends Animal(name) {
     |  eat()
     | }
defined class Bird

scala> val b = new Bird("Eagle")
```

### Overriding

Use `override` keyword to override a member.

Use `final` to prevent a method from being overridden.

Note: `override` keyword is optional.

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)

class Animal {
 def eat() = println("Yum yum")
}

class Bird extends Animal {
  override def eat() = println ("Beep")
}

// Exiting paste mode, now interpreting.

defined class Animal
defined class Bird

scala> val b = new Bird
b: Bird = Bird@6eb0bab

scala> b.eat()
Beep
```

### Accessing Super Class Members

Use `super` keyword to access super class members.

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)

class Animal {
 def eat() = println("Yum yum")
}

class Bird extends Animal {
  override def eat() = {
     super.eat()
     println("Beep")
  }
}

// Exiting paste mode, now interpreting.

defined class Animal
defined class Bird

scala> val b = new Bird
b: Bird = Bird@21d9ac3c

scala> b.eat()
Yum yum
Beep
```

## Final Classes

Use `final` to prevent a class from being extended.

```sbt
scala> final class Animal
defined class Animal

scala> class Bird extends Animal
                          ^
       error: illegal inheritance from final class Animal
```

## Sealed Classes

Used to create Algebraic Data Type (ADT).

Sealed classes can be extended from only within same file.

Compiler enforced restriction of number of possible subtypes.

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)

sealed class Animal

class Bird extends Animal

class Fish extends Animal

// Exiting paste mode, now interpreting.

defined class Animal
defined class Bird
defined class Fish

scala> val b = new Bird
b: Bird = Bird@5b799c0b

scala> class Dog extends Animal
                         ^
       error: illegal inheritance from sealed class Animal
```

## Abstract Classes

Abstract classes cannot be instantiated and have a mix of abstract and concrete members.

```sbt
scala> abstract class Animal {
     |  def eat(): Unit
     | }
defined class Animal

scala> class Bird extends Animal {
     | }
             ^
       error: class Bird needs to be abstract. Missing implementation for:
         def eat(): Unit // inherited from class Animal

scala> class Bird extends Animal {
     |  override def eat() = println("eat")
     | }
defined class Bird
```

Another Example:

```scala
sealed abstract class TrainInfo {
  def number: Int
}

// implicit override of number member
case class InterCityExpress(number: Int, hasWifi: Boolean = false) extends TrainInfo
case class RegionalExpress(number: Int) extends TrainInfo
case class BavarianRegional(number: Int) extends TrainInfo
```

## Traits

Inherit from exactly one superclass and mix-in multiple traits.

Traits may contain concrete and abstract members.

Traits are abstract and cannot contain parameters.

Traits extend exactly one super class.

`extends` creates either a sub-class or mixes in first trait.

`with` mixes in subsequent traits.

```sbt
:paste
// Entering paste mode (ctrl-D to finish)

abstract class Animal

class Bird extends Animal {
  def fly: String = "I'm flying"
}

trait Swimmer {
  def swim: String = "I'm Swimming"
}

class Fish extends Animal with Swimmer
class Duck extends Bird with Swimmer

// Exiting paste mode, now interpreting.

defined class Animal
defined class Bird
defined trait Swimmer
defined class Fish
defined class Duck
```

### Mix-in Rules

#### Inheritance Hierarchy

Traits must respect inheritance hierarchy i.e. a subclass must not extend two incompatible super classes.

```sbt

scala> class Component
defined class Component

scala> trait IntegratedScreen extends Component
defined trait IntegratedScreen

scala> class Computer
defined class Computer

scala> class Laptop extends Computer with IntegratedScreen
                                          ^
       error: illegal inheritance; superclass Computer
        is not a subclass of the superclass Component
        of the mixin trait IntegratedScreen
```

#### Concrete Members

If multiple traits define the same member then you must `override`.

To be able to use override, either trait has to extend the other (or) create a common parent with an abstract member.

```sbt
scala> trait Swimmer { def move = "swimming" }
defined trait Swimmer

scala> trait Flyer { def move = "flying" }
defined trait Flyer

scala> class Duck extends Swimmer with Flyer
             ^
       error: class Duck inherits conflicting members:
         def move: String (defined in trait Swimmer) and
         def move: String (defined in trait Flyer)
         (note: this can be resolved by declaring an `override` in class Duck.)
```