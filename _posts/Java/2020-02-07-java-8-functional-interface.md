---
layout: "post"
title: "Java 8 Functional Interface"
date: "2020-02-07 22:02"
tags: Java
---
Java is a strongly object-oriented language. Only object and primitive types can be part of function input and output. In other languages, function is just another type, allowing the creation of higher-order functions that take functions as input or return a function as output. Being object oriented is not bad, but it brings a lot of verbosity to the program. For example, let's say we have to create an instance of `Runnable`. Usually we do it using anonymous classes like below.

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
    	System.out.println("My Runnable");
    }
};
```
The important part of the program is the code inside the `run()` method. The rest of the code is because Java is a strongly object-oriented language and the anonymous definition that extends `Runnable` is required.

Java 8 Functional Interfaces and Lambda Expressions help us in writing smaller and cleaner code by removing a lot of boilerplate code.

* toc
{:toc}

## Java 8 Functional Interface

An interface with a SAM (Single Abstract Method) is a Functional Interface, and its implementation may be treated as lambda expressions. The `@FunctionalInterface` annotation is added so that we can mark an interface as functional interface. It is not mandatory to use the `@FunctionalInterface` annotation, but it's the best practice to use it with functional interfaces to avoid adding extra methods accidentally. If the interface is annotated with `@FunctionalInterface` annotation and we try to have more than one abstract method, it throws compile-time error.

`java.lang.Runnable` is a great example of functional interface with single abstract method `run()`.

The major benefit of Java 8 functional interfaces is that we can use lambda expressions to instantiate them and avoid using bulky anonymous class implementation.

Java 8 Collections API has been rewritten and new Stream APIs are introduced with the usage of functional interfaces. Java 8 has defined a lot of functional interfaces in `java.util.function` package. Some of the useful java 8 functional interfaces are `Consumer`, `Supplier`, `Function` and `Predicate`.


### Functions
The most simple and general case of a lambda is a functional interface with a method that receives one value and returns another. This function of a single argument is represented by the `Function` interface which is parameterized by the types of its argument and a return value:

```java
public interface Function<T, R> { … }
```

One of the usages of the Function type in the standard library is the `Map.computeIfAbsent` method that returns a value from a map by key but calculates a value if a key is not already present in a map. To calculate a value, it uses the passed Function implementation:

```java
Map<String, Integer> nameMap = new HashMap<>();
Integer value = nameMap.computeIfAbsent("John", s -> s.length());
```

A value, in this case, will be calculated by applying a function to a key, put inside a map and also returned from a method call. By the way, we may replace the lambda with a *method reference* that matches passed and returned value types:

```java
Integer value = nameMap.computeIfAbsent("John", String::length);
```

The `Function` interface has also a default `compose` method that allows to combine several functions into one and execute them sequentially:

```java
Function<Integer, String> intToString = Object::toString;
Function<String, String> quote = s -> String.format("'%s'", s);

Function<Integer, String> quoteIntToString = quote.compose(intToString);

assertEquals("'5'", quoteIntToString.apply(5));
```

All Function interfaces have an `apply` method that applies the function to the given input parameter(s).

### Functions for Primitive Types
Since a primitive type can’t be a generic type argument, there are versions of the `Function` interface for most used primitive types `double`, `int`, `long`, and their combinations in argument and return types:

* `IntFunction`, `LongFunction`, `DoubleFunction`: arguments are of specified type, return type is parameterized
* `ToIntFunction`, `ToLongFunction`, `ToDoubleFunction`: return type is of specified type, arguments are parameterized
* `DoubleToIntFunction`, `DoubleToLongFunction`, `IntToDoubleFunction`, `IntToLongFunction`, `LongToIntFunction`, `LongToDoubleFunction` — having both argument and return type defined as primitive types, as specified by their names

You can write similar Function Interfaces for other primitive types, say `ShortToByteFunction`:

```java
@FunctionalInterface
public interface ShortToByteFunction {
    byte applyAsByte(short s);
}
```

### Bi-Functions
Function interfaces with the word "Bi" in them are for functions with two arguments: `BiFunction` (both input parameters and return values generalized), `ToDoubleBiFunction`, `ToIntBiFunction`, and `ToLongBiFunction` (both input parameters generalized and return primitive).

One of the typical examples of using this interface in the standard API is in the `Map.replaceAll` method, which allows replacing all values in a map with some computed value.

### Suppliers
The `Supplier` functional interface is a Function Interface that does not take any arguments. It is typically used for lazy generation of values that takes a considerable amount of time.

Another use case for the `Supplier` is defining a logic for sequence generation. To demonstrate it, let’s use the static `Stream.generate` method to create a `Stream` of Fibonacci numbers:

```java
int[] fibs = {0, 1};
Stream<Integer> fibonacci = Stream.generate(() -> {
    int result = fibs[1];
    int fib3 = fibs[0] + fibs[1];
    fibs[0] = fibs[1];
    fibs[1] = fib3;
    return result;
});
```

The function that is passed to the `Stream.generate` method implements the `Supplier` functional interface. Notice that to be useful as a generator, the `Supplier` usually needs some sort of external state (implemented as an array). Note, **all external variables used inside the lambda have to be effectively final**.

Other specializations of `Supplier` functional interface include `BooleanSupplier`, `DoubleSupplier`, `LongSupplier` and `IntSupplier`, whose return types are corresponding primitives.

### Consumers
As opposed to the `Supplier`, the `Consumer` accepts a generic argument and returns nothing. It is a function that is representing side effects, such as `System.out.println`.

The `BiConsumer` interface takes two generic parameters. One of its use cases is to iterate through the entries of a map:

```java
map.forEach((key, value) ->
    System.out.println(String.format("Key: %s, Value: %s", key, value))
);
```

There are special `Consumer` and `BiConsumer` types for primitive types

### Predicates
In mathematical logic, a predicate is a function that receives a value and returns a boolean value.

The `Predicate` functional interface is a specialization of a Function that receives a generic value and returns a boolean. A typical use case of the `Predicate` lambda is to filter a collection of values:

```java
List<String> namesWithA = names.stream()
  .filter(name -> name.startsWith("A"))
  .collect(Collectors.toList());
```

As in all other function types, there are `IntPredicate`, `DoublePredicate` and `LongPredicate` versions of this function that receive primitive values.

### Operators
`Operator` interfaces are special cases of a function that receive and return the same value type. The `UnaryOperator` interface receives a single argument, and a `BinaryOperator` takes two arguments. One use case of the `UnaryOperator` is to replace all values in a collection, and one use case of the `BinaryOperator` is a reduction operation:

```java
// UnaryOperator, can be replaced by method reference
names.replaceAll(name -> name.toUpperCase());

// BinaryOperator
int sum = values.stream().reduce(0, (i1, i2) -> i1 + i2);
```

Of course, there are also specializations of UnaryOperator and BinaryOperator that can be used with primitive values.

### Legacy SAMs
Not all functional interfaces are introduced in Java 8. SAMs in older Java versions, such as `Runnable` and `Comparator`, are also marked with `@FunctionalInterface` to use with Lambdas.

## Lambda Expression

Lambda Expression produces functions. Since there is only one abstract function in the functional interfaces, there is no confusion in applying the lambda expression to the method. Lambda Expressions syntax is `(argument) -> (body)`. We can write above anonymous `Runnable` using lambda expression:

```java
Runnable r1 = () -> System.out.println("My Runnable");
```

### Lambda Expression Examples

Below I am providing some code snippets for lambda expressions with small comments explaining them.

```java
() -> {}                     // No parameters; void result

() -> 42                     // No parameters, expression body
() -> null                   // No parameters, expression body
() -> { return 42; }         // No parameters, block body with return
() -> { System.gc(); }       // No parameters, void block body

// Complex block body with multiple returns
() -> {
  if (true) return 10;
  else {
    int result = 15;
    for (int i = 1; i < 10; i++)
      result *= i;
    return result;
  }
}

(int x) -> x+1             // Single declared-type argument
(int x) -> { return x+1; } // same as above
(x) -> x+1                 // Single inferred-type argument, same as below
x -> x+1                   // Parenthesis optional for single inferred-type case

(String s) -> s.length()   // Single declared-type argument
(Thread t) -> { t.start(); } // Single declared-type argument
s -> s.length()              // Single inferred-type argument
t -> { t.start(); }          // Single inferred-type argument

(int x, int y) -> x+y      // Multiple declared-type parameters
(x,y) -> x+y               // Multiple inferred-type parameters
(x, final y) -> x+y        // Illegal: can't modify inferred-type parameters
(x, int y) -> x+y          // Illegal: can't mix inferred and declared types
```

### Method and Constructor References

A method reference is used to refer to a method without invoking it. A constructor reference is similarly used to refer to a constructor without creating a new instance of the named class or array type.

Examples of method and constructor references:

```
System::getProperty
System.out::println
"abc"::length
ArrayList::new
int[]::new
```

## Best Practices
### Prefer Standard Functional Interfaces
Functional interfaces, which are gathered in the java.util.function package, satisfy most developers' needs in providing target types for lambda expressions and method references. Each of these interfaces is general and abstract, making them easy to adapt to almost any lambda expression. Developers should explore this package before creating new functional interfaces. Refer to [Java 8 Functional Interface](#java-8-functional-interface) section.

### Use the `@FunctionalInterface` Annotation
Annotate your functional interfaces with `@FunctionalInterface`. At first, this annotation seems to be useless. Even without it, your interface will be treated as functional as long as it has just one abstract method.

But the `@FunctionalInterface` annotation forces the compiler to trigger an error in response to any attempt to break the predefined structure of a functional interface.

### Instantiate Functional Interfaces with Lambda Expressions
The compiler will allow you to use an inner class to instantiate a functional interface. However, this can lead to very verbose code. The lambda expression approach can be used for any suitable interface from old libraries. It is usable for interfaces like `Runnable`, `Comparator`, and so on.

### Avoid Overloading Methods with Functional Interfaces as Parameters
To explain this, let's look at an example:

```java
public interface Processor {
    String process(Callable<String> c) throws Exception;
    String process(Supplier<String> s);
}

public class ProcessorImpl implements Processor {
    @Override
    public String process(Callable<String> c) throws Exception {
        // implementation details
    }

    @Override
    public String process(Supplier<String> s) {
        // implementation details
    }
}
```
At first glance, this seems reasonable. But any attempt to execute either of the ProcessorImpl‘s methods with lambda:

```java
String result = processor.process(() -> "abc");
```

ends with an error with the following message:

```
reference to process is ambiguous
both method process(java.util.concurrent.Callable<java.lang.String>)
in com.baeldung.java8.lambda.tips.ProcessorImpl
and method process(java.util.function.Supplier<java.lang.String>)
in com.baeldung.java8.lambda.tips.ProcessorImpl match
```

We should use methods with different names.

### Don’t Treat Lambda Expressions as Inner Classes
While lambda expression looks similar to generate an anonymous inner class for the function interface, lambda expression and inner class are different in an important way: scope.

When you use an inner class, it creates a new scope. You can hide local variables from the enclosing scope by instantiating new local variables with the same names. You can also use the keyword `this` inside your inner class as a reference to its instance.

However, lambda expressions work within the enclosing scope. You can’t hide variables from the enclosing scope inside the lambda’s body. In this case, the keyword `this` is a reference to an enclosing instance.

### Keep Lambda Expressions Short And Self-explanatory
If possible, use one line constructions instead of a large block of code. Remember **lambdas should be an expression, not a narrative**. Despite its concise syntax, lambdas should precisely express the functionality they provide.

This is mainly stylistic advice, as performance will not change drastically. Here are the general rules of thumb:
1. Avoid blocks of code in Lambda's body. Make it short and succinct, one-liner is the best.
2. Avoid specifying parameter types. A compiler in most cases is able to resolve the type of lambda parameters with the help of type inference. Therefore, adding a type to the parameters is optional and can be omitted.
3. Avoid parentheses around a single parameter. Lambda syntax requires parentheses only around more than one parameter or when there is no parameter at all.
4. Avoid return statement and braces in one-liner. Braces and return statements are optional in one-line lambda bodies.
5. Use Method References if possible.
6. Use “effectively final” variables. Accessing a non-final variable inside lambda expressions will cause the compile-time error. But it doesn’t mean that you should mark every target variable as `final` because a compiler treats every variable as final, as long as it is assigned only once. It is safe to use such variables inside lambdas because the compiler will control their state and trigger a compile-time error immediately after any attempt to change them. This approach should simplify the process of making lambda execution thread-safe.


## References
1. [https://www.baeldung.com/java-8-lambda-expressions-tips](https://www.baeldung.com/java-8-lambda-expressions-tips)
2. [https://www.baeldung.com/java-8-functional-interfaces](https://www.baeldung.com/java-8-functional-interfaces)
