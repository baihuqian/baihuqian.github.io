---
layout: "post"
title: "Go-Interfaces"
date: "2018-02-17 20:04"
tags: Go
---

# Interface
An *interface type* is defined as a set of method signatures. A value of interface type can hold any value that implements those methods.

A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword. Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.

Under the covers, interface values is a tuple of a value and a concrete type: `(value, type)`. An interface value holds a value of a specific underlying concrete type. Calling a method on an interface value executes the method of the same name on its underlying type.

## Interface values with `nil` underlying values

If the concrete value inside the interface itself is `nil`, the method will be called with a `nil` receiver. In some languages this would trigger a null pointer exception, but in Go it is common to write methods that gracefully handle being called with a nil receiver.

Note that an interface value that holds a `nil` concrete value is itself non-nil.

## `Nil` interface values

A `nil` interface value holds neither value nor concrete type. Calling a method on a `nil` interface is a run-time error because there is no type inside the interface tuple to indicate which concrete method to call.

## The empty interface

The interface type that specifies zero methods is known as the empty interface:`interface{}`.

An empty interface may hold values of any type as every type implements at least zero methods. Empty interfaces are used by code that handles values of unknown type. For example, fmt.Print takes any number of arguments of type `interface{}`.

## Type assertions
A type assertion provides access to an interface value's underlying concrete value.

```go
t := i.(T)
```

This statement asserts that the interface value `i` holds the concrete type `T` and assigns the underlying `T` value to the variable `t`. If i does not hold a `T`, the statement will trigger a panic.

To test whether an interface value holds a specific type, a type assertion can return two values: the underlying value and a boolean value that reports whether the assertion succeeded.

```go
t, ok := i.(T)
```

If `i` holds a `T`, then `t` will be the underlying value and `ok` will be true. If not, `ok` will be false and t will be the zero value of type T, and no panic occurs. It's similar to reading from a map.

## Type switches

A type switch is a construct that permits several type assertions in series. A type switch is like a regular switch statement, but the cases in a type switch specify types (not values), and those values are compared against the type of the value held by the given interface value.

```go
switch v := i.(type) {
case T:
    // here v has type T
case S:
    // here v has type S
default:
    // no match; here v has the same type as i
}
```

The declaration in a type switch has the same syntax as a type assertion `i.(T)`, but the specific type `T` is replaced with the keyword `type`.

This switch statement tests whether the interface value `i` holds a value of type `T` or `S`. In each of the `T` and `S` cases, the variable `v` will be of type `T` or `S` respectively and hold the value held by `i`. In the default case (where there is no match), the variable `v` is of the same interface type and value as `i`.

## Common interfaces
### Stringers

One of the most ubiquitous interfaces is Stringer defined by the fmt package.

```go
type Stringer interface {
    String() string
}
```

A Stringer is a type that can describe itself as a string. The fmt package (and many others) look for this interface to print values.

### Errors

Go programs express error state with error values. The error type is a built-in interface similar to `fmt.Stringer`:

```go
type error interface {
    Error() string
}
```

(As with `fmt.Stringer`, the `fmt` package looks for the error interface when printing values.)

Functions often return an error value, and calling code should handle errors by testing whether the error equals `nil`.

```go
i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
    return
}
fmt.Println("Converted integer:", i)
```

A nil error denotes success; a non-nil error denotes failure.

### Readers

The `io` package specifies the `io.Reader` interface, which represents the read end of a stream of data. The Go standard library contains many implementations of these interfaces, including files, network connections, compressors, ciphers, and others.

The io.Reader interface has a Read method:

```go
func (T) Read(b []byte) (n int, err error)
```

Read populates the given byte slice with data and returns the number of bytes populated and an error value. It returns an `io.EOF` error when the stream ends.
