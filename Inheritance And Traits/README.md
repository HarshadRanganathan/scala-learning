
[Inheritance](#inheritance)

- [Subclass](#subclass)

    - [Inherited Method](#inherited-method)

    - [Superclass Constructor](#superclass-constructor)

    - [Overriding](#overriding)

    - [Accessing Super Class Members](#accessing-super-class-members)
  
- [Final Classes](#final-classes)

- [Sealed Classes](#sealed-classes)

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