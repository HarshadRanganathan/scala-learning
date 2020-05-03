## For Loops

For Loops return Unit and execute side-effects.

Seq contains generators, filters and definitions.

Block may contain side effects and its result is ignored.

`for (seq) block`

```sbt
scala> for (n <- 1 to 6) print(n)
123456
scala> for (n <- 1 to 6 if n%2==0) print(n)
246
```

## For Expressions

for-expression isn't a loop but returns a result.

expr creates an element of result.

`for (seq) yield expr`

```sbt
scala> for (n <- 1 to 6 if n%2==0) yield (n + 1)
res6: IndexedSeq[Int] = Vector(3, 5, 7)

scala> :paste
// Entering paste mode (ctrl-D to finish)

for {
  n <- 1 to 3
  m <- 1 to n
} yield n * m

// Exiting paste mode, now interpreting.

res7: IndexedSeq[Int] = Vector(1, 2, 4, 3, 6, 9)
```