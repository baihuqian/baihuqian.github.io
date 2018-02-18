---
layout: "post"
title: "GoLang: Structures and Methods"
date: "2018-02-17 20:03"
tags: Go
---

Go isn't an object-oriented (OO) language, so it doesn't have objects nor inheritance and thus, doesn't have the many concepts associated with OO such as polymorphism and overloading.

What Go does have are structures, which can be associated with methods. Go also supports a simple but effective form of composition. Overall, it results in simpler code, but there'll be occasions where you'll miss some of what OO has to offer.

# Structs
A `struct` is a collection of fields. And a `type` declaration associates the name with the struct.

```go
type Vertex struct {
	X int
	Y int
}
```
Struct fields are accessed using a dot.

```go
v0 := Vertex {
  X: 1,
  Y: 2, // This comma is required
}
v1 := Vertex{1, 2}
v2 := Vertex{X: 1}  // Y:0 is implicit
v3 := Vertex{}      // X:0 and Y:0
v1.X = 4
```

We don't have to set all or even any of the fields when creating a value of the structure. Unassigned fields get a zero value.

Despite the lack of constructors, Go does have a built-in `new` function which is used to allocate the memory required by a type. The result of `new(X)` is the same as `&X{}`.

## Getters and Setters
Go doesn't provide automatic support for getters and setters. If you have a field called `owner` (lower case, unexported), the getter method should be called `Owner` (upper case, exported), not `GetOwner`. The use of upper-case names for export provides the hook to discriminate the field from the method. A setter function, if needed, will likely be called `SetOwner`.

## Anonymous Fields
It is possible to create structs with fields which contain only a type without the field name. These kind of fields are called anonymous fields. Even though an anonymous fields does not have a name, by default the name of a anonymous field is the name of its type. For example,

```go
type Person struct {  
    string
    int
}
```

has two fields with name `string` and `int`.

# Pointer
The type `*T` is a pointer to a `T` value. Its zero value is `nil`.

```go
var p *int
```

The `&` operator generates a pointer to its operand (it's called the *address of* operator).

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

To access the field `X` of a struct when we have the struct pointer `p` we could write `(*p).X`. However, the language permits us instead to write just `p.X`.

## Pointer v.s. Value
As you write Go code, it's natural to ask yourself *should this be a value, or a pointer to a value?* There are two pieces of good news. First, the answer is the same regardless of which of the following we're talking about:

* A local variable assignment
* Field in a structure
* Return value from a function
* Parameters to a function
* The receiver of a method

Secondly, if you aren't sure, use a pointer.

# Method
Go does not have classes. However, you can define methods on types. A method is a function with a special *receiver* argument. The receiver appears in its own argument list between the `func` keyword and the method name.

```go
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

A method is just a function with a receiver argument.

You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as int).

## Pointer receivers
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

# Composition
Go supports composition, which is the act of including one structure into another. In some languages, this is called a trait or a mixin. Composition can be achieved in Go is by embedding one struct type into another. For example,

```go
type author struct {  
    firstName string
    lastName  string
    bio       string
}

func (a author) fullName() string {  
    return fmt.Sprintf("%s %s", a.firstName, a.lastName)
}

type post struct {  
    title     string
    content   string
    author
}

func (p post) details() {  
    fmt.Println("Title: ", p.title)
    fmt.Println("Content: ", p.content)
    fmt.Println("Author: ", p.author.fullName())
    fmt.Println("Bio: ", p.author.bio)
}
```
Note the anonymous field `author` in the `post` struct. This field denotes that `post` struct is composed of `author`. Now `post` struct has access to all the fields and methods of the `author` struct. Thus you can do `p.fullName()` besides `p.author.fullName()`. However, `post` struct can always "overwrite" the `fullname()` method defined for `author`:

```go
func (p post) fullName() string {  
    return fmt.Sprintf("Author: %s %s", p.author.firstName, p.author.lastName)
}
```
