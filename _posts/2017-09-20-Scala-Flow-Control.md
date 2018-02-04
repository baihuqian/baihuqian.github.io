---
title: Learning Scala - Flow Control
tags: Scala
---

Scala's flow control is more advanced than Java.

* TOC
{:toc}

## Condition
Scala provides the same `if/else` keywords as Java. They return a value, which is the value of the expression that is true after evaluation. For example,

```scala
val s = if (x > 0) 1 else -1
```
	
is the same as
	
```scala
if (x > 0) s = 1 else s = -1
```
	
In the first form, it can initialize a `val` but in the second form `s` needs to be a `var`.

The expression is similar to `?:` operator in C++ and Java, but if/else in Scala can have statements inside.

An `if` statement without the `else` clause can yield a `()` if evaluation fails. So `if (x > 0) 1` is equivalent to `if (x > 0) 1 else ()`.

There are no `switch` statement in Scala. Use pattern matching instead. `@switch` can be applied to improve performance from decision tree to branch table at compile time, with a few constraints.

## Loop
Scala has the same `while` and `do-while` loops as Java and C++:

```scala
while (n > 0) {
	r = r * n
	n -= 1
}

do {
	r = r * n
	n -= 1
} while (n > 0)
```
	
`for` loop is more like Python:

```scala
for (i <- 1 to n)
	r = r * i
```
Ranges created with the `<-` symbol in `for` loops are refereed to as *generators*. There are no `break` and `continue` keywords in Scala.

Note `i to n` returns a `Range` from 1 to n inclusive, and `<-` makes `i` traverse all values of the expression at its right side.

In Scala, it is possible to apply a function to all values in a sequence at once, reducing the use of loops.

### Advanced `for` Loop Usage
#### Multiple counters
Multiple *generators* of the form `variable <- expression` separated by semicolon:

```scala
for (i <- 1 to 3; j <- 1 to 3) print(i + j)
```

It is preferred to write it in this form:

```scala
for {
    i <- 1 to 3
    j <- 1 to 3
} print(i + j)
```

#### Guard
Each generator can have a *guard*, a Boolean condition by `if`:

```scala
for (i <- 1 to 3; j <- 1 to 3 if i != j) print(i + j)
```

No semicolon before the `if`. 

You can define variables to use in `for` without the `var` keyword:

```scala
for (i <- 1 to 3; from = 4 - i; j <- from to 3) print (i + j)
```
	
The content of a `for` loop can be expressed in braces, replacing semicolons with newlines:

```scala
for { i <- 1 to 3
	from = 4 - i
	j <- from to 3
}
```

`for` loop with counters (like Go): Scala collections offer a `zipWithIndex` method that create a new collection with `(elem, counter)` as elements.

### `for` Comprehensions
When the body of the `for` loop starts with `yield` keyword, then the `for` loop constructs a collection of values. For example,

```scala
for {
	i <- 1 to 3
	square = i * i
} yield square
```

constructs a collection. Note variables inside `for` loop don't need a `var` or `val` quantifier. If the statement after `yield` requires multiple lines, enclose them in `{}`. In most cases, the `for/yield` loop returns the same type of collections as input.

For comprehension is essentially the same as `map` method available on `Iterable`.

### Patterns in `for` expressions
`for ((k, v) <- map) println(k + v)` traverses the maps and output keys and values bound to `k` and `v`. Match failures are silently ignored.

Guard can be applied, but it goes after the `<-` symbol.

### How Scala Compiler works with `for` loops
1. A simple `for` loop that iterates over a collection is translated to a `foreach` method call on the collection.
2. A `for` loop with a `yield` expression is translated to a `map` call on the collection.
3. The *guard* is translated to one or more `withFilter` calls on the collection prior to `foreach` or `map` call.

Thus it is more straightforward to avoid explicit for loop but use `map`, `foreach`, and `filter` directly.

## `break` and `continue`
There are no `break` and `continue` keywords, but similar functionality can be implemented with `scala.util.control.Breaks`. It offers two methods, `break` and `breakable`. `break` will throw a `BreakControl` exception and `breakable` will catch it.

### Break
```scala
breakable {
    for (x <- xs) {
        if (cond)
            break
    }
}
```

### Continue
```scala
for (x <- xs) {
    breakable {
        if (cond)
            break
    }
}
```

### Labeled Break
`Breaks` object can be instantiated, and `breakable` will only catch exception thown by `break` in the same object.

### Enumeration
Unlike Java, Scala does not have enumeration. Standard library provides an `Enumeration` helper class.

Define enumeration as follow:

```scala
object TrafficLightColor extends Enumeration {
	val Red, Yellow, Green = Value
}
	
```

Each gets an ID and name: ID defaults to one more than the previous one, starting from zero; name is the variable name.

So the enumeration can be accessed by `TrafficLightColor.Red`, etc.

You can look up an enumeration value by its ID or name:

```scala
TrafficLightColor(0)
TrafficLightColor.withName("Red")
```

## Exception
Scala exception works the same way as in Java. But Scala has no **"checked"** exceptions, i.e. you never have to declare a function of method might throw and exception. However, if you want, you can still do so, and it is required if the method will be called from Java code:

```scala
@throws(classOf[NumberFormatException])
def toInt(s: String) = s.toInt
```

A `throw` expression has the special type `Nothing`. If one branch in `if/else` is `Nothing`, the type of the `if/else` is the type of the other branch.

The syntax of catch exceptions is modeled after the pattern matching syntax.

`finally` clause is the same as in Java.