---
layout: post
title: 'Coursera Programming Languages, Part C'
date: '2020-08-14 21:10'
tags:
  - Computer Science
full-width: true
---

This is the course notes I took when studying [Programming Languages (Part C)](https://www.coursera.org/learn/programming-languages-part-c), offered by Coursera.

This course is based on [Part A]({{ site.baseurl }}{% link _posts/Learning/2020-04-07-coursera-programming-languages-part-a.md %}) and [Part B]({{ site.baseurl }}{% link _posts/Learning/2020-08-02-coursera-programming-languages-part-b.md %}).

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
Subclassing is an essential feature of class-based OOP. If class `C` is a subclass of `D`, then every instance of `C` is also an instance of `D`. The definition of `C` inherits the methods of `D`. Ruby fields are not inherited but Java fields are inherited from the superclass.

Every class in Ruby except `Object` has one superclass. The classes form a tree where each node is a class and the parent is its superclass, and the `Object` class is the root of the tree. In class-based languages, this is called the *class hierarchy*. Ruby's `Class` class defines a method `superclass` that returns the superclass.

Every object also has methods `is_a?` and `instance_of?`. The method `is_a?` takes a class (e.g., `x.is_a? Integer`) and returns `true` if the receiver is an instance of `Integer` or any (transitive) subclass of `Integer`, i.e., if it is below `Integer` in the class hierarchy. The method `instance_of?` is similar but returns true only if the receiver is an instance of the class **exactly**, not a subclass. (Note that in Java the primitive `instanceof` is analogous to Ruby’s `is_a?`.)

Inheritance improves code reuse. However, there are other ways to achieve code reuse:
1. Ruby allows run-time extension and modification of classes;
2. Composition.

## Overriding and Dynamic Dispatch
Overriding means redefining methods in inheritance. Suppose Class `D` has two methods `m` and `n`, and `n` calls `m`. Class `C` extends from `D` and overrides `m`. Now, When `C.n` is called, it will use the overridden version of `m` in `C` even though `C` does not redefine method `n`. This semantics goes by many names, including *dynamic dispatch*, *late binding*, and *virtual method calls*. This behavior is due the way `self` is treated in the environment.

Dynamic dispatch answers the question that, given a call `e0.m(e1,e2,...en)`, what are the rules for “looking up” what method definition `m` we call. Let us notice that in general such questions about how we “look up” something are often essential to the semantics of a programming language. For example, in ML and Racket, the rules for looking up variables led to lexical scope and the proper treatment of function closures. And in Racket, we had three different forms of let-expressions exactly because they have different semantics for how to look up variables in certain subexpressions.

In Ruby, the variable-lookup rules for local variables in methods and blocks are not too different from in ML and Racket despite some strangeness from variables not being declared before they are used. But we also have to consider how to “look up” instance variables, class variables, and methods. In all cases, the answer depends on the object bound to `self` — and `self` is treated specially.

In class-based object-oriented languages like Ruby,
* To look up an instance variable `@x`, we use the object bound to `self`;
* To look up a class variable `@@x`, we use the object bound to `self.class`.
* To look up a method `m` in `e0.m(e1,...,en)`:
 * Evaluate `e0`, `e1`, ..., `en` to values, i.e., objects `obj0`, `obj1`, ..., `objn`.
 * Get the class of `obj0`. Every object “knows its class” at run-time. Think of the class as part of the state of `obj0`.
 * Suppose `obj0` is class `A`. If `m` is defined in `A`, call that method. Otherwise recur with the superclass of `A` to see if it defines `m`. Raise a “method missing” error if neither `A` nor any of its superclasses define `m`. (Actually, in Ruby the rule is actually to  call a method called `method_missing`, which any class can define, so we again start looking in `A` and then its superclass. But most classes do not define `method_missing` and the definition of it in `Object` raises the error we expect.)
 * We have now found the method to call. If the method has formal arguments (i.e., argument names or parameters) `x1`, `x2`, ..., `xn`, then the environment for evaluating the body will map `x1` to `obj1`, `x2` to `obj2`, etc. But there is one more thing that is the essence of object-oriented programming and has no real analogue in functional programming: We always have `self` in the environment. **While evaluating the method body, `self` is bound to `obj0`, the object that is the “receiver” of the message**.

The binding of `self` in the callee as described above is what is meant by the synonyms “late-binding,” This semantics is quite a bit more complicated than ML/Racket function calls as we have to treat the notion of `self` differently from everything else in the language.

*Static overloading* in Java and C# makes method-lookup rules more complicated. So we need to not just find *some* method with the right name, but we have to find one that *matches* the types of the arguments at the call. Moreover, multiple methods might match and the language specifications have a long list of complicated rules for finding the *best* match (or giving a type error if there is no best match).

Dynamic dispatch is different from closures. Lexical scope in closures defines the lookup rules when the function is **defined**, and thus a later shadowing cannot change the previous definition. Dynamic dispatch defines the lookup rules at run-time, and thus an overriding can change a previous definition.

## Implementing Dynamic Dispatch in Racket
We want to demonstrate that one language's *semantics* can typically be coded up as an *idiom*. Using racket instead of ML removes the type limitation.

We define objects as a list of fields and methods.

```
(struct obj (fields methods))
```

We will have `fields` hold an *immutable* list of *mutable* pairs where each element pair is a symbol (the field name) and a value (the current field contents). We have to define our own `assoc` function because the Racket’s `assoc` expects an immutable list of immutable pairs.

```
(define (assoc-m v xs)
  (cond [(null? xs) #f]
        [(equal? v (mcar (car xs))) (car xs)]
        [#t (assoc-m v (cdr xs))]))

(define (get obj fld)
  (let ([pr (assoc-m fld (obj-fields obj))])
    (if pr
        (mcdr pr)
        (error "field not found"))))

(define (set obj fld v)
  (let ([pr (assoc-m fld (obj-fields obj))])
    (if pr
        (set-mcdr! pr v)
        (error "field not found"))))
```

The `methods` field will also be an association list mapping method names to functions. The key to getting dynamic dispatch to work is that these functions will all take an extra *explicit* argument that is *implicit* in languages with built-in support for dynamic dispatch.

```
(define (send obj msg . args) ; convenience: multi-argument functions (2+ arguments)
  (let ([pr (assoc msg (obj-methods obj))])
    (if pr
        ((cdr pr) obj args) ; do the call
        (error "method not found" msg))))
```

Notice how the function we use for the method gets passed the “whole” object `obj`, which will be used for any sends to the object bound to `self`. To make a 2-D point:

```

(define (make-point _x _y)
  (obj
   (list (mcons 'x _x)
         (mcons 'y _y))
   (list (cons 'get-x (lambda (self args) (get self 'x)))
         (cons 'get-y (lambda (self args) (get self 'y)))
         (cons 'set-x (lambda (self args) (set self 'x (car args))))
         (cons 'set-y (lambda (self args) (set self 'y (car args))))
         (cons 'distToOrigin
               (lambda (self args)
                 (let ([a (send self 'get-x)]
                       [b (send self 'get-y)])
                   (sqrt (+ (* a a) (* b b)))))))))
```

Subclassing means appending fields and methods to the superclass. Overriding works by placing the overridden methods earlier in the methods list (`assoc` returns the first matching pair).

```
(define (make-color-point _x _y _c)
  (let ([pt (make-point _x _y)])
    (obj
     (cons (mcons 'color _c)
           (obj-fields pt))
     (append (list
              (cons 'get-color (lambda (self args) (get self 'color)))
              (cons 'set-color (lambda (self args) (set self 'color (car args)))))
           (obj-methods pt)))))
```

Then dynamic dispatch works because the function lookup in `send` uses the list specific to the object itself.

# Section 9
## Code Decomposition: OOP vs. Functional
To implement some programs, we will build up data and operations on them. In functional (and procedural) programming, programs are broken down into *functions that perform some operation*. We define "one-of" data types to hold the data. In object-oriented programming, programs are broken down into *classes that give behavior to some kind of data*.

If we put the functionality of a program in a matrix, with types of data in one axis and operations in the other axis, then these two forms of decompositions are exactly opposite:
* Functional programming breaks down the program along the operations axis;
* Object-oriented programming breaks down the program along the data axis.

Which way is better is purely a personal preference. Understanding this symmetry is invaluable in conceptualizing software or deciding how to decompose a problem. Moreover, various software tools and IDEs can help you view a program in a different way than the source code is decomposed.

The choice between “rows” and “columns” becomes less subjective if we later extend our program by adding new data or new operations. It is easy to add new operations in functional programming, and it's easy to add new data in object-oriented programming. Here "easy" means not to change existing code. The visiter pattern makes adding operations more easily in OOP, and having a generic "other" type in functional programming helps with adding new data.

More generally, making software that is both robust and extensible is valuable but difficult. Extensibility can make the original code more work to develop, harder to reason about locally, and harder to change (without breaking extensions). In fact, languages often provide constructs exactly to *prevent* extensibility, such as Java's `final` keyword.

## Binary Methods Decomposition
When we have operations that take two (binary) or more (n-ary) variants as arguments, we often have many more cases. A function that takes two arguments has $$n^2$$ cases, which can be shown as a 2-D matrix. With functional programming, all these cases are still covered together in a function. And this function uses pattern matching with all $$n^2$$ cases to branch out.

The OOP approach is more cumbersome. We expect each type/class implements a method to perform the operation with another object, but the implementation should depend on the type of the other object! So instead of defining branches by type using `is_a?`, we should "tell" the other object to do the operation for `self`'s type by calling a method corresponding to the type of `self`.

This technique is called *double dispatch*. For example, we want to define addition on type `Int`, `String`, and `Rational`, we will need to do the following:
1. Implement `add_values` that takes another value in each class, as obliged by the base class. The implementation calls one of the `add_` methods in 2 depending on the type of `self`.
2. Implement `add_int`, `add_string`, and `add_rational` for the second dispatch.

```ruby
class Int < Value
  # double-dispatch for adding values
  def add_values v # first dispatch
    v.addInt self
  end
  def addInt v # second dispatch: other is Int
    Int.new(v.i + i)
  end
  def addString v # second dispatch: other is MyString (notice order flipped)
    MyString.new(v.s + i.to_s)
  end
  def addRational v # second dispatch: other is MyRational
    MyRational.new(v.i+v.j*i,v.j)
  end
end
```

Essentially, we have one method for each of the $$n^2$$ cases, with each of the $$n$$ classes getting one for each type (a total of $$n$$). Then dynamic dispatch would pick the correct `add_values` based on the runtime type of the first argument, and then pick the right `add_` calls based on the type of the second argument.

It is *not* true that all OOP languages require the cumbersome double-dispatch pattern to implement binary operations in a full OOP style. Languages with *multimethods*, also known as *multiple dispatch*, provide more intuitive solutions. In such languages, each class defines $$n$$ versions of the operation by indicating the class it expects for its argument. It's still a total of $$n^2$$ methods, but one of them will be picked based on the type of both arguments, at run-time, instead of dispatching in two steps.

Multimethods is a powerful and *different* semantics and the method-lookup rules involve the run-time class of the receiver (the object whose method we are calling) and the run-time class of the argument(s).

Ruby does not support multimethods because you can only have one method with a particular name in each class. Java and C++ also do not have multimethods. Though they allow methods with the same name, it uses the *types* of the arguments determined at *compile-time* and *not* the runtime class of the result of evaluating the arguments. This semantics is called *static overloading*.

## Multiple Inheritance, Mixins, and Interfaces
With single inheritance, the *class hierarchy* — all the classes in a program and what extends what — forms a *tree*, where `A extends B` means `A` is a child of `B` in the tree. With multiple inheritance, the class hierarchy may not be a tree, but a directed acyclic graph (DAG). Hence it can have “diamonds” — four classes where one is a (not necessarily immediate) subclass of two others that have a common (not necessarily immediate) superclass. By “immediate” we mean directly extends (child-parent relationship) whereas we could say “transitive” for the more general ancestor-descendant relationship.

With multiple inheritance, we may have conflicts for the fields / methods inherited from the different classes, or ambiguity of a common ancestor in diamonds. At the very least we need expressions using `super` to indicate which superclass is intended. These issues are some of the reasons language with multiple inheritance (most well-known is C++) need fairly complicated rules for how subclassing, method lookup, and field access work. For example, C++ has (at least) two different forms of creating a subclass. One always makes copies of all fields from all superclasses. The other makes only one copy of fields that were initially declared by the same common ancestor.

Ruby has *mixins*, which are somewhere between multiple inheritance and interfaces. Ruby mixins is the same idea as Scala *traits*. They provide actual code to classes that *include* them, but they are not classes themselves, so you cannot create instances of them.

To define a Ruby mixin, we use the keyword `module` instead of `class`. A class definition can include these methods by using the `include` keyword and the name of the mixin. Mixins can access `self` and methods on `self` that are not defined by the mixin. Instead the mixin *assumes* that all classes that include the mixin define this method. The most commonly-used mixins are `Enumerable` and `Comparable`. `Comparable` assumes classes that includes it implement a `<=>` method, which is the same as Java's `compare` method, and provide methods `=`, `!=`, `>`, `>=`, `<`, and `<=`. The `Enumerable` module is where many of the useful block-taking methods that iterate over some data structure are defined, by assuming the class has the method `each` defined.

Mixins are not as powerful as multiple inheritance because we have to decide upfront what to make a class and what to make a mixin.

In Java or C#, a class can have only one immediate superclass but it can implement any number of *interfaces*. An interface is just a list of methods signatures. A class type-checks only if it actually provides (directly or via inheritance) all the methods of all the interfaces it claims to implement. An interface is a type (every class is also a type), so if a class `C` implements interface `I`, then we can pass an instance of `C` to a method expecting an argument of type `I`. Because interfaces do not actually define methods, none of the problems discussed above about multiple inheritance arise. But it makes the type systems more flexible.

Interfaces are closer to the idea of “duck typing” than just using classes as types, but a class has some interface type only if the class definition explicitly says it implements the interface.

Dynamic dispatch allows subclasses to provide code to superclasses. Abstract methods allow superclasses to define the signatures of such code-providing methods. Type-checking also ensures the object’s class has implemented all the abstract methods, so it is safe to call these methods in the superclass. Languages with abstract methods and multiple inheritance (e.g., C++) do not need interfaces, because a class with only pure virtual methods (abstract) is effectively an interface, but multiple inheritance is more flexible than interface.

There is an interesting parallel between abstract methods and higher-order functions. In both cases, the language supports a programming pattern where some code is passed other code in a flexible and reusable way. In OOP, different subclasses can implement an abstract method in different ways and code in the superclass, via dynamic dispatch, can then uses these different implementations. With higher-order functions, if a function takes another function as an argument, different callers can provide different implementations that are then used in the function body.

# Section 10
A key source of expressiveness in ML’s type system is *parametric polymorphism* (like `'a list`), also known as *generics*. While statically-typed object-oriented languages such as Java or C# have generics these days, the source of type-system expressiveness most fundamental to object-oriented style is *subtype polymorphism*, also known as *subtyping*.

The idea of subtyping is to allow functions to work on arguments with *more* fields. In other words, If some expression has a record type `{f1:t1, ..., fn:tn}`, then let the expression also have a type where some of the fields are removed. It seems backwards that *subtype* has *more* information.

We need to define the subtyping rules for a type system. A common misconception is that if we are defining our own language, then we can make the typing and subtyping rules whatever we want. For subtyping, the key guiding principle is *substitutability*: If we allow `t1 <: t2`, then any value of type `t1` must be able to be used in every way a `t2` can be.

Some good subtyping rules are:
* “Width” subtyping: A supertype can have a subset of fields with the same types, i.e., a subtype can have “extra” fields;
* “Permutation” subtyping: A supertype can have the same set of fields with the same types in a different order.
* Transitivity: If `t1 <: t2` and `t2 <: t3`, then `t1 <: t3`.
* Reflexivity: Every type is a subtype of itself: `t <: t`.

## Depth Subtyping
One bad subtyping example is depth subtyping with mutation: “Depth” subtyping: If `ta <: tb`, then `{f1:t1,...,f:ta,...,fn:tn} <: {f1:t1,...,f:tb,...,fn:tn}`. It doesn't work because the supertype can set `f` to a value of type `ta`, not knowing it is the type `tb`. Thus, depth subtyping is unsound with mutation. But if the field is immutable, then the depth subtyping rule is sound.

Java array allows depth subtyping: if `t1 <: t2`, then `t1[] <: t2[]`. Java array is also mutable, leading to unsoundness. Java throws a runtime `ArrayStoreException` when attempting to set an array element with an object of the supertype, by having runtime checks. Better solutions would be to use generics in combination with subtyping (e.g., bounded polymorphism).

## `null` in Java
`null` is a value with no fields or methods, its type should naturally reflect that it cannot be used as the receiver for a method or for getting/setting a field. Instead, Java allows `null` to have *any object type*, as though it defines every method and has every field. From a static checking perspective, this is exactly backwards. As a result, the language definition has to indicate that *every* field access and method call includes a run-time check for `null`, leading to the `NullPointerException` errors that Java programmers regularly encounter.

While it is convenient to have `null`, such as initializing a field of type `Foo` before you can create a `Foo` instance, it is also very common to have fields and variables that should never hold `null` and you would like to have help from the type-checker to maintain this invariant. In ML, the `option` type is exactly for that: The types `t option` and `t` are not the same type.

## Function Subtyping
We need to understand the subtyping rule for function for high-order functions. For a function `t1 -> t2`, the subtyping rule works as follows:
* Return types are **covariant** for function subtyping: if `ta <: tb`, then `t -> ta <: t -> tb`, i.e., the subtype can have a return type that is a subtype of the supertype’s return type. You can return *more* than what you need to.
* Argument types are **contravariant** for function subtyping: if `tb <: ta`, then `ta -> t <: tb -> t`. You can pass in *more* than what you need to.

Put it together, the general rule for function subtyping is: If `t3 <: t1` and `t2 <: t4`, then `t1->t2 <: t3->t4`. This rule, combined with reflexivity (every type is a subtype of itself) lets us use contravariant arguments, covariant results, or both. Argument contravariance is the least intuitive concept in the course.

## Subtyping for OOP
Classes and types are different things! Java and C# purposely confuse them as a matter of convenience, but you should keep the concepts separate. A class defines an object’s *behavior*. Subclassing inherits behavior, modifying behavior via extension and override. A type describes *what fields an object has and what messages it can respond to*. Subtyping is a question of substitutability and what we want to flag as a type error.

An object is basically a record holding mutable fields and immutable methods. Sound subtyping for objects follows from sound subtyping for records and functions:
* A subtype can have extra fields.
* Because fields are mutable, a subtype cannot have a different type for a field.
* A subtype can have extra methods.
* Because methods are immutable, a subtype can have a subtype for a method, which means the method in the subtype can have contravariant argument types and a covariant result type.

OOP languages like Java or C# uses the class names as types, and they don't allow method overriding with contravariant argument types.

As a final subtle detail and advanced point, Java’s `this` (i.e., Ruby’s `self`) is treated specially from a type-checking perspective. While arguments are contravariant, `this` is *covariant*: methods are always called with a `this` argument that is a subtype of the type the method expects.

## Generics vs. Subtyping
Now we can compare subtype polymorphism (subtyping) and parametric polymorphism (generic types). Subtyping is a bad substitute for generics because we can only use `Object` as the type and later *downcast* to the actual type, which requires dynamic checking and results in possibility of failure. Generics are a bad substitute for subtyping because we cannot abstract away common code without using combersome higher-order functions.

## Bounded Polymorphism
You can combine generics and subtyping with *bounded generic types*, where instead of just saying “a subtype of `T`” or “for all types `’a`,” we can say, “for all types `’a` that are a subtype of `T`.” Like with generics, we can then use `’a` multiple times to indicate where two things must have the same type. Like with subtyping, we can treat `’a` as a subtype of `T`, accessing whatever fields and methods we know a `T` has.
