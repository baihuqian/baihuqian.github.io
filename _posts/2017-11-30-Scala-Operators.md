---
title: Learning Scala - Operators and Statements
tags: Scala
---

## Operators
Infix operators: `a identifier b`, such as `1 to 10, 1-> 10`. They're binary operators.

Unary operators: `a identifier` is the same as `a.identifier()`. For example, `1 toString`.

`+,-,!,~` are allowed as *prefix* operators. So be careful when breaking code into multiple lines at `+` and `-`: They have to be at the **end** of the line, not the beginning of the line!

```scala
x
+y
```
evaluates to `y`, but
```scala
x +
y
```
evaluates to `x + y`.

Assignment operators: have the form `operator=`. `a operator= b` is the same as `a = a operator b`. `<=`, `>=`, `!=` are not. Scala does not have ++ or -- operators, but it has += and -=.

## Block statement
The curly bracket `{}` is optional. It is required when multiple statements are inside if/else branch or loop statement that spans multiple lines, or on the same line separated by semicolons.

If needed, Scala programmers favor the Kernighan & Ritchie brace style:

```scala
if (n > 0) {
	r = r * n
	n -= 1
}
```

The result of a block statement is the value of the last expression. It is useful when initialization of a `val` takes multiple steps.

