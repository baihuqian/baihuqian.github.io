---
title: Learning Scala - Packges and Import
tags: Scala
---


Like Java, Scala uses packages provide namespace management, and import provides a shorter alias.

## Package
There are two major styles to add items to a package: `package` statement at the top of a file, like Java, or curly brace packaging style like C++ and C#:
```scala
package example {
	class Example1 {
		...
	}
}
```

A package can be defined in multiple files, and a file can contribute to multiple packages by using the curly brace style.

Package provides more consistent scope. Everything in parent scope is in scope so that package names can be *relative*, unlike Java.

Package names can be chained together using `.`. Everything in chained scope is no longer visible. For example, in `com.example.test` everything in `com` and `com.example` packages are not visible.

If all the code in one file belongs to the same package, you can use top-of-file notation:

```scala
package com.example.test
package people

class Person {
	...
}
```

Then `Person` class is in `com.example.test.people` package but everything in `com.example.test` is visible.

### Package objects
Package cannot include definitions of functions and variables. Package objects address this limitation.

Put the package object into a file called `package.scala` inside the package. The package object definition is like this (assuming package is called `com.exampleqian.test.model`):

```scala
// filename: src/com/exampleqian/test/model/package.scala
package com.exampleqian.test // model is not here

package object model {
    
}
```
Those defined in the package object can be directly accessed from within other classes, traits, objects in the package.

### Package Visibility
In Java, a class member without access quantifers are visible in the package containing the class. In Scala, you can use `private[packageName]` to expose access to a certain package.

## Import
Scala import is like Java, but more flexible. Imports can be anywhere, no just at the beginning of a file. The scope of import extends in the enclosing block. Scala uses `_` (instead of `*`) for wildcard.

Each Scala program implicitly imports `java.lang._` and `scala._` package, and `Predef` object. `Predef` object provides many implicit type conversions as well as methods like `println`, `readLine`, `assert`, and `require`.

It is possible to import multiple classes in one line: `import java.awt.{Color, Font}`.It is known as *import selector clause*.

Scala let you import members from a class as well. It is similar to Java *static import* (import static methods directly), but it is also able to import values.

### Renaming and Hiding
In order to avoid naming collisions in imports, Scala provides renaming and hiding.

Renaming: `import java.util.{HashMap => JavaHashMap}`. It is useful when importing several classes with the same name. Not only can you rename classes on import, but you can also rename class members as well.

Hiding: `import java.util.{HashMap => _, _}`. The selector `HashMap => _` hides `HashMap` instead of renaming it. So the above line imports everything except `HashMap`. Note the wildcard `_` must be at the end of the import clause.
