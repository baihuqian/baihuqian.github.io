---
layout: "post"
title: "GoLang: Variables"
date: "2018-02-17 19:58"
tags: Go
---

Go is statically typed. It uses C-family syntax, but with some differences. To declare the type of a variable, C-family uses prefix:

```c
int x;
```

However, readability is not very good as we can't read from left to right. Scala use colon syntax with suffix:

```scala
x: Int
```

Go uses suffix by dropping the colon:

```go
x int
```

The `var` statement declares a variable, or a list of variables. Note the type is at last.

```go
var power int
```

By default, Go assigns a zero value to variables. Integers are assigned `0`, booleans `false`, strings `""` and so on. Initial value can also be assigned at declaration, one per variable:

```go
var power int = 9000
var c, python, java = true, false, "no!"
```

If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

A `var` statement can be at package or function level. Inside a function, the `:=` short assignment statement can be used to declare a variable and assign initial value to it. The type in `:=` statement is inferred.

Like imports, Go won't let you have unused variables. Declared by unused variables will cause compile error.

## Constants
Constants are declared like variables, but with the `const` keyword.

```go
const Pi = 3.14
```

Constants cannot be declared using the := syntax.

Numeric constants are high-precision values. An untyped constant takes the type needed by its context, so a numeric constant can be passed to functions as int or float64 without any explict conversion.
