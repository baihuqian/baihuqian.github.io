---
layout: "post"
title: "GoLang: Basic Types"
date: "2018-02-17 19:59"
tags: Go
---
# Basic types

Go's basic types are

* bool
* string
* int  int8  int16  int32  int64
* uint uint8 uint16 uint32 uint64 uintptr
* byte // alias for uint8
* rune // alias for int32, represents a Unicode code point
* float32 float64
* complex64 complex128

Variable declarations may be "factored" into blocks, as with import statements.

```go
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)
```

The `int`, `uint`, and `uintptr` types are usually 32 bits wide on 32-bit systems and 64 bits wide on 64-bit systems. When you need an integer value you should use int unless you have a specific reason to use a sized or unsigned integer type.

## Type conversions

The expression `T(v)` converts the value `v` to the type `T`. Unlike in C, Go assignment between items of different type requires an explicit conversion.

## Type inference
When declaring a variable without specifying an explicit type (either by using the `:=` syntax or `var =` expression syntax), the variable's type is inferred from the value on the right hand side. When the right hand side of the declaration is typed, the new variable is of that same type. But when the right hand side contains an untyped numeric constant, the new variable may be an `int`, `float64`, or `complex128` depending on the precision of the constant.

## Pointer
The type `*T` is a pointer to a `T` value. Its zero value is `nil`.

```go
var p *int
```

The `&` operator generates a pointer to its operand.

```go
i := 42
p = &i
```

The `*` operator denotes the pointer's underlying value.

```go
fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
```

Unlike C, Go has no pointer arithmetic.
