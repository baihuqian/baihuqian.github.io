---
layout: "post"
title: "GoLang: Formatted Printing"
date: "2018-03-03 22:54"
tags: Go
---

Formatted printing in Go uses a style similar to C's printf family but is richer and more general. The functions live in the `fmt` package and have capitalized names: `fmt.Printf`, `fmt.Fprintf`, `fmt.Sprintf` and so on. The string functions (`Sprintf` etc.) return a string rather than filling in a provided buffer.

You don't need to provide a format string. For each of `Printf`, `Fprintf` and `Sprintf` there is another pair of functions, for instance `Print` and `Println`. These functions do not take a format string but instead generate a default format for each argument. The `Println` versions also insert a blank between arguments and append a newline to the output while the `Print` versions add blanks only if the operand on *neither* side is a string. In this example each line produces the same output.

```go
fmt.Printf("Hello %d\n", 23)
fmt.Fprint(os.Stdout, "Hello ", 23, "\n")
fmt.Println("Hello", 23)
fmt.Println(fmt.Sprint("Hello ", 23))
```

The formatted print functions `fmt.Fprint` and friends take as a first argument any object that implements the `io.Writer` interface; the variables `os.Stdout` and `os.Stderr` are familiar instances.

The numeric formats such as `%d` do not take flags for signedness or size; instead, the printing routines use the type of the argument to decide these properties.

```go
var x uint64 = 1<<64 - 1
fmt.Printf("%d %x; %d %x\n", x, x, int64(x), int64(x)) // 18446744073709551615 ffffffffffffffff; -1 -1

```

If you just want the default conversion, such as decimal for integers, you can use the catchall format `%v` (for “value”); the result is exactly what `Print` and `Println` would produce. Moreover, that format can print any value, even arrays, slices, structs, and maps.

For maps the keys may be output in any order, of course. When printing a struct, the modified format `%+v` annotates the fields of the structure with their names, and for any value the alternate format `%#v` prints the value in full Go syntax.

The quoted string format is also available through `%q` when applied to a value of type `string` or `[]byte`. The alternate format `%#q` will use backquotes instead if possible. (The `%q` format also applies to integers and runes, producing a single-quoted rune constant.) Also, `%x` works on strings, byte arrays and byte slices as well as on integers, generating a long hexadecimal string, and with a space in the format (`% x`) it puts spaces between the bytes.

Another handy format is `%T`, which prints the type of a value.

```go
fmt.Printf("%T\n", timeZone) //map[string] int
```

If you want to control the default format for a custom type, all that's required is to define a method with the signature `String()` string on the type. Don't construct a `String` method by calling `Sprintf` in a way that will recur into your `String` method indefinitely. This can happen if the `Sprintf` call attempts to print the receiver directly as a string, which in turn will invoke the method again.
