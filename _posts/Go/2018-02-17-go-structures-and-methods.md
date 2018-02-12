---
layout: "post"
title: "GoLang: Structures and Methods"
date: "2018-02-17 20:03"
tags: Go
---

Go isn't an object-oriented (OO) language, so it doesn't have objects nor inheritance and thus, doesn't have the many concepts associated with OO such as polymorphism and overloading.

What Go does have are structures, which can be associated with methods. Go also supports a simple but effective form of composition. Overall, it results in simpler code, but there'll be occasions where you'll miss some of what OO has to offer.

## Structs
A `struct` is a collection of fields. And a `type` declaration associates the name with the struct.

```go
type Vertex struct {
	X int
	Y int
}
```
Struct fields are accessed using a dot.

```go
v := Vertex{1, 2}
v.X = 4
```

### Pointers to structs
To access the field `X` of a struct when we have the struct pointer `p` we could write `(*p).X`. However, the language permits us instead to write just `p.X`.

### Struct literals
A struct literal denotes a newly allocated struct value by listing the values of its fields. You can list just a subset of fields by using the `Name: ` syntax, and the order of named fields is irrelevant. The special prefix `&` returns a pointer to the struct value.

```go
v1 = Vertex{1, 2}  // has type Vertex
v2 = Vertex{X: 1}  // Y:0 is implicit
v3 = Vertex{}      // X:0 and Y:0
p  = &Vertex{1, 2} // has type *Vertex
```

## Method
Go does not have classes. However, you can define methods on types. A method is a function with a special *receiver* argument. The receiver appears in its own argument list between the `func` keyword and the method name.

```go
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

A method is just a function with a receiver argument.

You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as int).

### Point receivers
You can declare methods with pointer receivers. This means the receiver type has the literal syntax `*T` for some type `T`. (Also, `T` cannot itself be a pointer such as `*int`.)

```go
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```

Methods with pointer receivers can modify the value to which the receiver points. Since methods often need to modify their receiver, pointer receivers are more common than value receivers. With a value receiver, the method operates on a copy of the original value. This is the same behavior as for any other function argument.

Methods with pointer receivers can be called on values directly, i.e. `v.Scale(5)` is the same as `(&v).Scale(5)` when `Scale` has a pointer receiver. Methods with value receivers take either a value or a pointer as the receiver when they are called, i.e. `p.Abs()` is interpreted as `(*p).Abs()`.

Pointer receivers are more convenient. The first reason is so that the method can modify the value that its receiver points to. The second is to avoid copying the value on each method call. In general, all methods on a given type should have either value or pointer receivers, but not a mixture of both.
