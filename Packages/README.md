Packages organize code bases.

```scala
package com.rharshad
```

### Package Import

Use `Import` keyword to import packages.

```sbt
scala> import com.rharshad.Train

// use _ to import all members of a package
scala> import com.rharshad._

// import multiple members with {...}
scala> import scala.concurrent.{Future, Promise}
import scala.concurrent.{Future, Promise}

// Alias imported members
scala> import java.sql.{Date => SqlDate}
import java.sql.{Date=>SqlDate}

// Scope imports within methods
scala> def method = {
     |     import java.sql.{Date => SqlDate}
     | }
```