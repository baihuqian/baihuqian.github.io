---
layout: "post"
title: "GoLang: Functions"
date: "2018-02-17 20:01"
tags: Go
---

A function can take zero or more arguments, and return any number of values. Notice that return types are defined last.

```go
func add(x int, y int) int {
	return x + y
}

func swap(x, y string) (string, string)
```

When two or more consecutive named function parameters share a type, you can omit the type from all but the last. For example, `x int, y int` can be written as `x, y int`.

Types of multiple return values are enclosed in parentheses.

Sometimes, you only care about one of the return values. In these cases, you assign the other values to `_`:

```go
_, exists := power("goku")
if exists == false {
  // handle this error case
}
```

This is more than a convention. `_`, the blank identifier, is special in that the return value isn't actually assigned. This lets you use `_` over and over again regardless of the returned type.

## Varargs
Type `...T` can be used for a function's final argument to specify that an arbitrary number of parameters of type `T` can appear in the argument list. The argument variable `v` is of type `[]T`.

A variable `v` of type `[]T` can be passed as a list of arguments using `v...` instead of a single slice argument.

### Named return values

Go's return values may be named. If so, they are treated as variables defined at the top of the function, and initialized to the zero values for their types. These names should be used to document the meaning of the return values.

A return statement without arguments returns the named return values. This is known as a "naked" return. Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions.

For example,

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
```

## Function values
Functions are values too. They can be passed around just like other values. Function values may be used as function arguments and return values.

### Function closure
Go functions may be closures. A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.

For example, the `adder` function returns a closure. Each closure is bound to its own sum variable.
```go
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}
```
## Function Type

Functions are first-class types:

```go
type Add func(a int, b int) int
```

which can then be used anywhere -- as a field type, as a parameter, as a return value.

```go
package main

import (
  "fmt"
)

type Add func(a int, b int) int

func main() {
  fmt.Println(process(func(a int, b int) int{
      return a + b
  }))
}

func process(adder Add) int {
  return adder(1, 2)
}
```

Using functions like this can help decouple code from specific implementations much like we achieve with interfaces.
