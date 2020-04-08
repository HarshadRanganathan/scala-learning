[Immutable Values](#immutable-values)

[Mutable Variables](#mutable-values)

[Type Inference](#type-inference)

## Immutable Values

An immutable value is defined with `val` keyword. Cannot be re-assigned to new value.

```sbt
scala> val message = "Hello World"
message: String = Hello World

scala> message = "Hi"
               ^
       error: reassignment to val
```

## Mutable Variables

An mutable variable is defined with `var` keyword. Can be re-assigned to new value.

```sbt
scala> var message = "Hello World"
message: String = Hello World

scala> message = "Hello World 2"
mutated message

scala> print(message)
Hello World 2
```

## Type Inference

- Compiler can infer type
- For public API, you should provide types explicitly

```sbt
// type inferred as string
scala> val message = "Hello World"
message: String = Hello World

// type inference returns error
scala> val message: Int = "Hello World"
                         ^
       error: type mismatch;
        found   : String("Hello World")
        required: Int
```