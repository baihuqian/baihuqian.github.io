---
layout: "post"
title: "GoLang: Flow Control"
date: "2018-02-17 20:00"
tags: Go
---


# Control
## For loop
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

### Range
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

## If
Go's `if` statements are like its `for` loops; the expression need not be surrounded by parentheses ( ) but the braces { } are required.

The braces for `else` statements are also required.

### If with a short statement
Like `for`, the `if` statement can start with a short statement to execute before the condition.

```go
if v := math.Pow(x, n); v < lim {
	return v
}
```
Variables declared by the statement are only in scope until the end of the `if`.

## Switch
A `case` body breaks automatically, unless it ends with a `fallthrough` statement.

Switch cases evaluate cases from top to bottom, stopping when a case succeeds. A `default` can be supplied at the end.

Like `if`, the expression need not be surrounded by parentheses ( ) but the braces { } are required. And a short statement can be supplied.

### Switch with no condition
Switch without a condition is the same as `switch true`.

This construct can be a clean way to write long if-then-else chains.

## Defer
A `defer` statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.
