---
layout: "post"
title: "Coursera Programming Languages, Part C"
date: "2020-08-07 14:44"
tags:
  - Computer Science
---

This is the course notes I took when studying [Programming Languages (Part C)](https://www.coursera.org/learn/programming-languages-part-c), offered by Coursera.

This course is based on [Part A]({{ site.baseurl }}{% link _posts/Learning/2020-04-07-coursera-programming-languages-part-a.md %}) and [Part B]({{ site.baseurl }}{% link _posts/Learning/2020-08-02-coursera-programming-languages-part-b.md %}).

In Part A, we have learned
1. Basics, functions, recursion, scope, variables, tuples, lists.
2. Datatypes, pattern-matching, tail-recursion.
3. First-class functions, closures.
4. Type inference, modules, equivalence.

In Part B, we have learned
1. Dynamic types, parentheses, delayed evaluation, streams, macros.
2. Structs, interpreters, closures.
3. Static checking, static vs. dynamic.

Both parts focus on functional programming. In part C we will learn
1. Dynamically-typed object-oriented programming
2. OOP vs. functional decomposition and advanced OOP topics
3. Subtyping, generics vs. subtyping.

* toc
{:toc}

# Section 8
We will study Ruby in this part. Our focus will be on object-oriented programming, dynamic typing, blocks (which are almost closures), and mixins.

## OOP In Ruby
Everything in Ruby is described in terms of object-oriented programming, which we abbreviate OOP, as follows:
1. All values (as usual, the result of evaluating expressions) are references to *objects*.
2. Given an object, code “communicates with it” by calling its *methods*. A synonym for calling a method is *sending a message*. (In processing such a message, an object is likely to send other messages to other objects, leading to arbitrarily sophisticated computations.)
3. Each object has its own private *state*. Only an object’s methods can directly access or update this state.
4. Every object is an instance of a *class*.
5. An object’s class determines the object’s *behavior.* The class contains method definitions that dictate how an object handles method calls it receives.

The method call `e0.m(e1, ..., en)` evaluates `e0`, `e1`, ..., `en` to objects. It then calls the method `m` in the result of `e0` (as determined by the class of the result of `e0`), passing the results of `e1, ..., en` as arguments. As for syntax, the parentheses are optional. In OOP, another common name for a method call is a *message send*. So we can say `e0.m e1` sends the result of `e0` the message `m` with the argument that is the result of `e1`. This terminology is “more object-oriented” — as a client, we do not care how the receiver (of the message) is *implemented* (e.g., with a method named `m`) as long as it can handle the message. As general terminology, in the call `e0.m args`, we call the result of evaluating `e0` the receiver (the object receiving the message). In the method body, `self` is used to refer to the current object.

Most expressions in Ruby are actually method calls. Even `e1 + e2` is just syntactic sugar for `e1.+ e2`. `puts` is a method in class `Object` from which every class inherits.

To construct and object, use `Foo.new (...)`. The call to `Foo.new` will create a new instance of `Foo` and then, before `Foo.new` returns, call the new object’s `initialize` method with all the arguments passed to `Foo.new`. All instance variables start with an `@`, e.g., `@foo`, to distinguish them from variables local to a method. The normal approach is for initialize always to create the same instance variables and for no other methods in the class to create instance variables, such that all methods have the same instance variables. Because Ruby does not require a variable to be declared before using it, you could create instance variables in other methods.

Instance variables are private to an object: only method calls with that object as the receiver can read or write the fields. Methods can have different *visibilities*. To make the contents of an instance variable available and/or mutable, we can easily define getter and setter methods, which by convention we can give the same name as the instance variable: for a variable `@foo`, getter has the name `foo` and setter has the name `foo=`. As a syntactic sugar, you can write `e.foo = bar` instead of `e.foo= bar`. Because getter and setter methods are so common, there is shorter syntax for defining them. `attr_reader :y, :z` defines getters for `@y` and `@z` and `attr_accessor :x` defines both getter and setter for `@x`.

Everything is an object, including numbers, booleans, and `nil`. There are many methods to support `reflection`, such as `class` and `methods`. The class of an object is an object of `Class`.

## Dynamic Class Definition
A Ruby program can change class definitions while a Ruby program is running. Naturally this affects all users of the class. Perhaps surprisingly, it even affects instances of the class that have already been created.

The syntax to add or change methods is particularly simple: Just give a class definition including method definitions for a class that is already defined. The method definitions either replace definitions for methods previously defined (with the same name method name) or are added to the class (if no method with the name previously existed).

## Duck Typing
Duck typing refers to the expression, “If it walks like a duck and quacks like a duck, then it’s a duck” though a better conclusion might be, “then there is no reason to concern yourself with the possibility that it might not be a duck.” In Ruby, this refers to the idea that the class of an object (e.g., “Duck”) passed to a method is not important so long as the object can respond to all the messages it is expected to (e.g., “walk to x” or “quack now”).

Duck typing can make code more reusable, allowing clients to make “fake ducks” and still use your code. In Ruby, duck typing basically “comes for free” as long you do not explicitly check that arguments are instances of particular classes using methods like `instance_of?` or `is_a?`. Duck typing has disadvantages. The most lenient specification of how to use a method ends up describing the whole implementation of a method, in particular what messages it sends to what objects.

## Arrays, Hashes and Ranges
Ruby array is can hold objects of different classes, can be dynamically sized, and support negative indices (which are interpreted from the end of the array). You can insert objects into any position of an array, with gaps filled with `nil`. Ruby array has a lot of methods defined in the standard library, so it could be used as tuples, stacks, or queues.

You really can think of arrays as mapping from numeric indices to objects. A hash is like an array except the mapping is from (any) objects to objects. It is also common (and more efficient) to use Ruby’s symbols for hash keys as in:


```ruby
{:sml => 7, :racket => 12, :ruby => 42}
```

A range represents a contiguous sequence of numbers (or other things, but we will focus on numbers). For example `1..100` represents the integers 1, 2, 3, ..., 100. Although there are often better iterators available, a method call like `(0..n).each {|i| e}` is a lot like a `for`-loop from `0` to `n` in other programming languages. It is worth emphasizing that duck typing lets us use ranges in many places where we might naturally expect arrays.

## Blocks and the `Proc` Class
Many classes have methods that take *blocks*. For example, integers have a `times` method that takes a block and executes it the number of times you would imagine: `x.times { puts "hi" }`. Blocks can have arguments and methods can have any other "regular" arguments. For example, the inject method is like the fold function we studied in ML and we can pass it an initial accumulator as a regular argument: `sum = [4,6,8].inject(0) { |acc,elt| acc + elt }`. In addition to the braces syntax shown here, you can write a block using `do` instead of `{` and `end` instead of `}`. This is generally considered better style for blocks more than one line long.

You can pass a block to *any* method. The method body calls the block using the `yield` keyword. To pass arguments to a block, you put the arguments after the `yield`, e.g., `yield 7` or `yield(8, "str")`. Using this approach, the fact that a method may expect a block is implicit; it is just that its body might use `yield`.

These blocks are *almost* closures. Blocks are closures in the sense that they can refer to variables in scope where the block is defined. Blocks, surprisingly, are *not* objects. You cannot pass them as “regular” arguments to a method. Rather, any method can be passed either 0 or 1 blocks, separate from the other arguments. There is no direct way to pass the caller’s block as the callee’s block argument, so you cannot assign blocks to variables and pass blocks as arguments (aka not "first-class values"), like what you would do for function closures.

However, Ruby has “real” closures too: The class `Proc` has instances that are closures. The method `call` in `Proc` is how you apply the closure to arguments, for example `x.call` (for no arguments) or `x.call(3, 4)`. To make a `Proc` out of a block, you can write `lambda { ... }` where `{ ... }` is any block. Interestingly, `lambda` is not a keyword. It is just a method in class `Object` (and every class is a subclass of `Object`, so `lambda` is available everywhere) that creates a `Proc` out of a block it is passed.

Ruby’s design is an interesting contrast from ML and Racket, which just provide full closures as the natural choice. In Ruby, blocks are more convenient to use than `Proc` objects and suffice in most uses, but programmers still have `Proc` objects when needed.

## Subclassing and Inheritance
