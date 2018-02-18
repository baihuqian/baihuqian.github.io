---
layout: "post"
title: "GoLang: Error Handling"
date: "2018-02-18 14:07"
tags: Go
---

Go's preferred way to deal with errors is through return values, not exceptions. Go programs express error state with error values. The error type is a built-in interface:

```go
type error interface {
    Error() string
}
```

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

More commonly, we can create our own errors by importing the `errors` package and using it in the `New` function:

```go
import (
  "errors"
)


func process(count int) error {
  if count < 1 {
    return errors.New("Invalid count")
  }
  ...
  return nil
}
```

There's a common pattern in Go's standard library of using error variables. For example, the `io` package has an `EOF` variable which is defined as:

```go
var EOF = errors.New("EOF")
```

Go does have `panic` and `recover` functions. `panic` is like throwing an exception while `recover` is like `catch`; they are rarely used.
