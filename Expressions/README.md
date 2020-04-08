[Expressions](#expressions)

  - [Code Blocks](#code-blocks)

[Multiline Expressions](#multiline-expressions)

## Expressions

Expression gets evaluated to a value.

- No curly braces needed for single line expressions

- Type annotations can be omitted

- No dots or parentheses needed

- Semicolons can be omitted

- No return statement needed

```sbt
scala> if(1 == 1) true else false
res4: Boolean = true

scala> if("Scala" startsWith "S") "Scala is a lightweight language" else "Scala is not Java"
res8: String = Scala is a lightweight language
```

### Code Blocks

Code blocks are expressions.

Return value is determined by last expression in the block.

Keep opening braces on the same line.

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)
// Code block expression
val sum = {
 val x = 1
 val y = 2
 x + y
}

// Exiting paste mode, now interpreting.

sum: Int = 3

scala> sum
res6: Int = 3
```

## Multiline Expressions

Expressions can span multiple lines. Use dangling operators (+)

```sbt
scala> :paste
// Entering paste mode (ctrl-D to finish)

val message = "Hello" +
"World"

// Exiting paste mode, now interpreting.

message: String = HelloWorld
```