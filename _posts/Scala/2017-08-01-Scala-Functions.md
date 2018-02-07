---
title: Learning Scala - Functions
tags: Scala
---

* TOC
{:toc}

Scala support native functions (outside an object) as C++ do, while in Java you have to use static methods.

Definition:

```scala
def abs(x: Double) = if (x > 0) x else -x

def fac(n: Int) = {
	var r = 1
	for (i <- 1 to n) r = r * i
	r
}
```

Types of all parameters must be specified, but no need to specify the return type when the function is not recursive. With a recursive function, you must specify the return type.

```scala
def fac(n: Int): Int = if (n <= 0) 1 else n * fac(n - 1)
```

There are no `return` keywords. The return value is the value of the block statement after the assignment sign.

There are no `throws` keywords to declare the exceptions that can be thrown, as it is not necessary to state a function throws an exception. However, `@throws(classOf[ExceptionClass])` does this, and multiple annotations can be applied to one function.

Scala is also a Functional Programming launguage, a function is a "first-class citizen", so you can assign it to a `val` or `var`:
	
```scala
// The underscore indicates you really mean the function itself, and you didn't just forget to supply the parameters.
val fun = scala.math.ceil _
val double = (i: Int) => { i * 2 }
```

Under the covers, a function variable is an instance of the `Function1` trait, which defines a function that takes one argument. The `scala` package defines similar traits with 0 up to 22 arguments.

## The Type of Functions
Its type is written as `(parameterType) => resultType`.

## Parameters
It is possible to provide default values. When the number of parameters provided is less than the actual number, default values are applied from the end (right). Parameters with default values must be after those without to take advantage of the default values. This means `def connect(timeout: Int = 6000, protocol: String)` will compile, but you cannot call it without specifying value for timeout.

Named arguments are supported. Mixture of named and unnamed parameters are supported.

Parameter checking is enforced using `require()` method. It takes a `Boolean` parameter, and if it evaluates to `false`, it throws and `IllegalArgumentException`.

### Variable number of parameters 
```scala
def sum(args: Int*) = {
	var result = 0
	for (arg <- args) result += arg
	result
}
```

`args` is a *varargs* field of type `Seq`. In this case, if the function is called with one argument, it needs to be a single Integer. The `_*` method adapts a sequence to a varargs field:

```scala
val s = sum(1 to 5: _*)
```

Attempting to define another parameter *after* a varargs field is not allowed. As an implication of this, a method can have only one varargs field.

### 0-args v.s. Parentheses-less
You can define a function without parentheses. You can call a **parameterless** methods with parentheses or without parentheses, but you **cannot call a parenthesesless method with empty parentheses**. A good style is:
* Mutator methods (that changes the object state) use parentheses.
* Accessor methods don't use parentheses.

## Return value
Scala lets you return multiple values using [tuples](#tuple).

```scala
def func() = {
    ...
    (a, b, c)
}
val (a, b, c) = func()
```

## Higher-order Functions
You can pass a function into another function by specifying the function type:

```scala
def fun(f: (Double) => Double) = f(0.25)
```
	
Then function `fun` is a **higher-order function**.

A function can also returns a function:

```scala
def mulBy(factor : Double) = (x : Double) => factor * x
```

Then `val triple = mulBy(3)` forms a **closure**: it consists of code about `mulBy` and the nonlocal parameter 3.

## Partial Function
A partial function of type `PartialFunction[A, B]` is a unary function where the domain does not necessarily include all values of type A. The function isDefinedAt allows you to test dynamically if a value is in the domain of the function.

Partial functions are often written using case statements:
```scala
val divide2: PartialFunction[ Int, Int] = {
    case d: Int if d != 0 => 42 / d
}
```
Partial function's `isDefinedAt` method tests if the given input is within the define domain.

Partial function can be chained sequentially with `andThen` method, which applies another partial function on the output of this partial function, or expand the input domain with `orElse` method which applies another partial function when input is not defined for this partial function.

One example of where you’ll run into partial functions is with the `collect` method on collections’ classes. The `collect` method takes a partial function as input, and as its Scaladoc describes, `collect` “Builds a new collection by applying a partial function to all elements of this list on which the function is defined.” Therefore, it will not throw exceptions like `map` when the input is not defined by the given operation.


## Procedures
Procedure is special function that returns no value, or return `Unit` to be precise. It is a function without the `=` between definition and body:

```scala
def doSomething(s: String) { // no =
	print(s)
}
```
To be cautious, avoid using it and explicitly returns `Unit` in the function definition.

## Anonymous Functions
`(x: Double) => 3 * x` is an anonymous function, or function literal. You can enclose it in braces, especially it is part of infix notations:

```scala
Array(3, 2, 1) map { (x: Double) => 3 * x }
```

### Parameter Inference
When you pass an anonymous function to another function or method, since the input type is given, Scala helps you deduce the type. For example, suppose `def fun(f: (Double) => Double) = f(0.25)`, then
```scala
fun((x: Double) => 3 * x)
```
can be written as 
```scala
fun((x) => 3 * x)
```
In this case, you can omit the parentheses around the only parameter:
```scala
fun(x => 3 * x)
```
If the parameter occurs only once in the right hand side, you can replace it with underscore:
```scala
fun(3 * _)
```
The reduced notation can be used in definition of anonymous function as well:
```scala
val fun = 3 * (_: Double) // specifies type
val fun: (Double) => Double = 3 * _ // also specifies type
```

If it consists of one statement that takes a single argument, you need not explicitly name and specify the argument. e.g.

```scala
x.foreach(println)
```

## Tail Recursion
To transform a `while` loop that involves using `var`s to more functional style code with only `val`s, recursion is used. Scala compiler is able to automatically apply optimization to tail recursion by transforming it into a `while` loop.

To explicitly apply tail recursion optimization, supply `@tailrec` annotation.

## Partially applied function
In functional programming languages, when you call a function that has parameters, you are said to be applying the function to the parameters. When all the parameters are passed to the function — something you always do in Java — you have fully applied the function to all of the parameters. But when you give only a subset of the parameters to the function, the result of the expression is a partially applied function.

```scala
val c = scala.math.cos _
```
`c` is called a partially applied function. It’s partially applied because the cos method requires one argument, which you have not yet supplied.

Simmilary, you can apply parameters to some arguments, leaving others free:
```scala
val sum = (a: Int, b: Int) => a + b
val f = sum(a, _: Int)
```
`f` is a partial function with one parameter.

## Closure
There are many definitions of closure. Wikipedia defines a closure like this:
>“In computer science, a closure (also lexical closure or function closure) is a function together with a referencing environment for the non-local variables of that function. A closure allows a function to access variables outside its immediate lexical scope.”

Paul Cantrell states,
>“a closure is a block of code which meets three criteria:
1. The block of code can be passed around as a value, and
2. It can be executed on demand by anyone who has that value, at which time
3. It can refer to variables from the context in which it was created (i.e., it is closed with respect to variable access, in the mathematical sense of the word “closed”).

For a function that exercises closure, its references to variables in the enclosing scope become free variable along with arguments of this function. Thus the values of closure are passed to function implicitly.