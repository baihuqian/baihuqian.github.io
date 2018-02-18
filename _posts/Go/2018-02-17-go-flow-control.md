---
layout: "post"
title: "GoLang: Flow Control"
date: "2018-02-17 20:00"
tags: Go
---

# For loop
Go has only one looping construct, the `for` loop.

Unlike other languages like C, Java, or Javascript there are no parentheses surrounding the three components of the `for` statement and the braces { } are always required.

```go
for i := 0; i < 10; i++ {
	sum += i
}
```

The init and post statement are optional. When both are omitted, it is essentially a while loop, and the semicolons can be dropped:

```go
for sum < 1000 {
	sum += sum
}
```

Moreover, `for {  }` is the infinite loop.

## Range
The range form of the for loop iterates over a slice or map.

When ranging over a slice, two values are returned for each iteration: the first is the index, and the second is a copy of the element at that index.

```go
for index, value := range slice {
}
```

You can skip the index or value by assigning to _. If only the index is wanted, `, value` can be dropped entirely.

```go
for _, value := range slice {
}

for index := range slice {
}
```

For strings, the `range` does more work for you, breaking out individual Unicode code points by parsing the UTF-8. Erroneous encodings consume one byte and produce the replacement rune U+FFFD. `for pos, char := range "string"` returns each Unicode rune and its byte position.

# If
Go's `if` statements are like its `for` loops; the expression need not be surrounded by parentheses ( ) but the braces { } are required.

The braces for `else` statements are also required.

In the Go libraries, you'll find that when an `if` statement doesn't flow into the next statement—that is, the body ends in `break`, `continue`, `goto`, or `return`—the unnecessary `else` is omitted.

```go
f, err := os.Open(name)
if err != nil {
    return err
}
codeUsing(f)
```

This is an example of a common situation where code must guard against a sequence of error conditions. The code reads well if the successful flow of control runs down the page, eliminating error cases as they arise. Since error cases tend to end in `return` statements, the resulting code needs no `else` statements.

### If with a short statement
Like `for`, the `if` statement can start with a short statement to execute before the condition.

```go
if v := math.Pow(x, n); v < lim {
	return v
}
```
Variables declared by the statement are only in scope until the end of the `if`, including `else if` and `else`.

A more realistic usage is error handling:

```go
if err := process(); err != nil {
  return err
}
```

## Switch
Go's switch is more like `if-else-if-else` chains than C-style switch. The expressions need not be constants or even integers, as they can be any boolean expressions or even comma-separated lists. Switch cases evaluate cases from top to bottom, stopping when a case succeeds. A `default` can be supplied at the end.

A `case` body breaks automatically, unless it ends with a `fallthrough` statement.

```go
func unhex(c byte) byte {
    switch {
    case '0' <= c && c <= '9':
        return c - '0'
    case 'a' <= c && c <= 'f':
        return c - 'a' + 10
    case 'A' <= c && c <= 'F':
        return c - 'A' + 10
    }
    return 0
}

func shouldEscape(c byte) bool {
    switch c {
    case ' ', '?', '&', '=', '#', '+', '%':
        return true
    }
    return false
}
```

Like `if`, the expression need not be surrounded by parentheses ( ) but the braces { } are required. And a short statement can be supplied.

### Switch with no condition
Switch without a condition is the same as `switch true`.

This construct can be a clean way to write long if-then-else chains.

# Defer
A `defer` statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order. However, The **arguments** to the deferred function (which include the receiver if the function is a method) are evaluated when the `defer` executes, not when the call executes.
