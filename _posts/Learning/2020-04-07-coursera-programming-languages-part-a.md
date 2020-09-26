---
layout: post
title: Coursera Programming Languages, Part A
date: '2020-04-07 21:53'
tags:
  - Computer Science
full-width: true
---

This is the course notes I took when studying [Programming Languages (Part A)](https://www.coursera.org/learn/programming-languages), offered by Coursera. Subsequently I studied [Part B]({{ site.baseurl }}{% link _posts/Learning/2020-08-02-coursera-programming-languages-part-b.md %}) and [Part C]({{ site.baseurl }}{% link _posts/Learning/2020-08-14-coursera-programming-languages-part-c.md %}).

This course series studiy the fundamental concepts that appear in one form or another in almost every programming language. Different languages are used to see how they can take complementary approaches to representing concepts taught in this course. We mainly study two axes: statically-typed languages vs. dynamically-typed languages, and functional vs. object-oriented programming.

Part A studies the basics and statically-typed functional programming language:
1. Basics, functions, recursion, scope, variables, tuples, lists.
2. Datatypes, pattern-matching, tail-recursion.
3. First-class functions, closures.
4. Type inference, modules, equivalence.

Part B applied functional programming to a dynamically-typed language:
1. Dynamic types, parentheses, delayed evaluation, streams, macros.
2. Structs, interpreters, closures.
3. Static checking, static vs. dynamic.

Part C studies object-oriented programming:
1. Dynamically-typed object-oriented programming
2. OOP vs. functional decomposition and advanced OOP topics
3. Subtyping, generics vs. subtyping.

* toc
{:toc}

# Section 1
## Variable Bindings and Expressions
We will learn ML in a way that teaches core programming-languages concepts. An ML program is a sequence of *bindings*. Each binding gets **type-checked** and then (assuming it type-checks) **evaluated**. There are several kinds of bindings.

A *variable binding* in ML has this syntax:

```sml
val x = e;
```
Here, `val` is a keyword, `x` can be any variable, and `e` can be any *expression*. `=` and `;` are keywords. Syntax is *how to write it*, but we still need to know its *semantics* (how it type-checks and evaluates). What type (if any) a binding has depends on a *static environment* (or *context*), which is roughly the types of the preceding bindings in the file. How a binding is evaluated depends on a *dynamic environment* (or just *environment*), which
is roughly the values of the preceding bindings in the file. To type-check a variable binding `e`, we use the “current static environment” (the types of preceding bindings) to type-check `e` and produce a “new static environment”: `x` having type `t` where `t` is the type of `e`. To evaluate a variable
binding, we use the “current dynamic environment” (the values of preceding bindings) to evaluate `e` and produce a “new dynamic environment”: `x` having the value `v` (value is an expression that "has no more computation to do") where `v` is the result of evaluating `e`. For example,

```sml
(* This is a comment *)
val x = 34;
(* static environment: x-->int *)
(* dynamic environment: x-->34 *)
```

Thus, there are three rules to think about for an expression: syntax, type-checking, and evaluation. Because an expression can be built upon sub-expressions, these rules can be specified based on other expressions. For example, the conditional expression:

- Syntax is `if e1 then e2 else e3` where `e1`, `e2`, and `e3` are expressions
- Type-checking: using the current static environment, a conditional type-checks only if
 - `e1` has type `bool`
 - `e2` and `e3` have the same type.
 - The type of the whole expression is the type of `e2` and `e3`.
- Evaluation: under the current dynamic environment, evaluate e`1`. If the result is `true`, the result of evaluating `e2` under the current dynamic environment is the overall result. If the result is `false`, the result of evaluating `e3` under the current dynamic environment is the overall result.

Bindings are *immutable*. Given `val x = 8+9;` we produce a dynamic environment where `x` maps to `17`. In this environment, `x` will always map to `17`. Also, the expression in binding is evaluated eagerly, i.e., it is evaluated before the binding is added to the dynamic environment. There is no **assignment statement** in ML to change what `x` maps to. However, you can have another binding of `x` later, but that just creates a *different environment* where the later binding for `x` **shadows** the earlier one.

## Function Bindings
Syntax:
```
fun x0 (x1 : t1, ..., xn : tn) = e
```
This is a binding for a function named `x0`. It takes `n` arguments `x1`, ... `xn` of types `t1`, ..., `tn` and has an expression `e` for its body.

Type-checking:

Function `x0` has a type `(t1 * t2 * ... * tn) -> t`. To type-check a function binding, we type-check the body `e` in a static environment that includes
1. all the earlier bindings;
2. argument bindings `x1` to `t1`, ..., `xn` to `tn`. Note that the arguments are not added to the top-level environment and thus cannot be used outside the function body.
3. the function `x0` itself of type `(t1 * t2 * ... * tn) -> t`. Because `x0` is in the environment, we can make *recursive* calls (a function definition can use itself).

For the function binding to type-check, the body `e` must have the type `t`, i.e., the result type of `x0`. The return type `t` is not specified, but *inferred* by the language. This feature is called *type inference*.

Evaluation:

The evaluation rule for a function binding is trivial: A function is a *value* - we simply add `x0` to the environment as a function that can be called later.

## Function Calls
Function call is a new kind of *expression*.

The syntax is `e0 (e1,...,en)` with the parentheses optional if there is exactly one argument.

The typing rules require that `e0` has a function type that looks like `t1*...*tn->t` and for 1 ≤ `i` ≤ n, `ei` has type `ti`.

For the evaluation rules, we use the environment at the point of the call to evaluate `e0` to `v0`, `e1` to `v1`, ..., `en` to `vn`. Then `v0` must be a function (it will be assuming the call type-checked) and we evaluate the function's body in an environment extended such that the function arguments map to `v1`, ..., `vn`.

Exactly which environment is it we extend with the arguments? The environment that “was current” when the function was **defined**, not the one where it is being *called*.

## Tuples and Lists
A tuple is finite ordered sequence of elements. The syntax is `(e1, e2, ..., en)` which evaluates `e1` to `v1`, ..., `en` to `vn`. The type of a tuple is `t1 * t2 * ... tn` where for 1 ≤ `i` ≤ n, `ei` has type `ti`. Tuples can be nested as deep as we want (i.e. `ei` can be tuples), but its type defines how many parts it has.

A list can have flexible numbers of elements of the **same** type. A non-empty list with n values is written `[v1,v2,...,vn]` where `vi` has the same type. You can make a list with `[e1,...,en]` where each expression is evaluated to a value. The type of non-empty list is `t list` where `t` is the type of the element. The empty list, with syntax `[]`, has 0 elements. It is a value of type `'a list` (pronounced "alpha list") where `'a` is a type placeholder.

It is more common to make a list with `e1 :: e2`, pronounced "e1 consed onto e2." Here `e1` evaluates to an "item of type `t`" and `e2` evaluates to a "list of t" and the result is a new list that starts with the result of `e1` and then is all the elements in `e2`.

Functions over lists are usually recursive in order to get to all the elements. Two questions to ask yourself when writing the recursion:
1. What should the answer be for the empty list;
2. What should the answer be for a non-empty list in terms of the answer for the tail of the list?

Similarly, functions that produce lists will recursively create lists out of smaller lists.

## Let Expressions and Scope
Let expressions define local *bindings* (i.e. variables and functions). It is crucial for style and for efficiency.

Syntax: `let b1 b2 ... bn in e end` where each `bi` is a binding and `e` is an expression.

The type-checking and semantics of a let-expression are much like the semantics of the top-level bindings in our ML program. We evaluate each binding in turn, creating a larger environment for the subsequent bindings. So we can use all the earlier bindings for the later ones, and we can use them all for `e`. We call the scope of a binding "where it can be used," so the scope of a binding in a let-expression is the later bindings in that let-expression and the “body” of the let-expression (the `e`). The value `e` evaluates to is the value for the entire let-expression, and, unsurprisingly, the type of `e` is the type for the entire let-expression.

For example, this expression evaluates to 7; notice how one inner binding for `x` *shadows* an outer one.

```sml
let val x = 1
in
	(let val x = 2 in x+1 end) + (let val y = x+2 in y+1 end)
end
```

Let-expressions can be used to define helper functions used in only one other function. Because the helper functions defined in let-expressions use the scope of the outer function, they can access the outer function's arguments:

```sml
(* Note the use of x in count *)
fun countup_from1_better (x:int) =
	let fun count (from:int) =
		if from=x
		then x::[]
		else from :: count(from+1)
	in
		count 1
	end
```

This technique - define a local function that uses other variables in scope - is a hugely common and convenient thing to do in functional programming. It can also improve program efficiency, especially in recursions where the result from the recursion calls can be stored in scope. If recursion is called multiple times, it will result in **exponential** computation time. Using let-expressions to store the result of recursions can cut down computation time.

## Options
An option value has either 0 or 1 thing: `NONE` is an option value “carrying nothing” whereas `SOME e` evaluates `e` to a value `v` and becomes the option carrying the one value `v`. The type of `NONE` is `'a option` and the type of `SOME e` is `t option` if `e` has type `t`.

* `isSome` evaluates to `false` if its argument is `NONE`.
* `valOf` to get the value carried by `SOME` (raising an exception for `NONE`).

## Boolean Operations
* And: `e1 andalso e2`.
* Or: `e1 orelse e2`.
* Not: `not e1`. Note `not` is a function `bool -> bool` while `andalso` and `orelse` are not.

## Lack of Mutation and Benefits Thereof
Because there are no assignment statements, in ML there is no way to *change* the contents of a binding, a tuple, or a list. Having immutable data is probably the most important “non-feature” a language can have, and it is one of the main contributions of functional programming. Immutable data makes sharing and aliasing irrelevant.

Java, on the contrary, does require programmers to be *obsessed* about aliasing (creating a new reference to the same object) and object identity, because changing a field in an object will impact all aliased object references.

## The Pieces of a Programming Language
we can list the essential “pieces” necessary for defining and learning any programming language:
* Syntax: How do you write the various parts of the language?
* Semantics: What do the various language features mean? For example, how are expressions evaluated?
* Idioms: What are the common approaches to using the language features to express computations?
* Libraries: What has already been written for you? How do you do things you could not do without library support (like access files)?
* Tools: What is available for manipulating programs in the language (compilers, read-eval-print loops, debuggers, ...)

Programming Languages course focuses on semantics and idioms. Syntax is a fact to learn. Libraries and tools are almost always learned on the job.

# Section 2
## Conceptual Ways to Build New Types
Programming languages have *base types*, like `int`, `bool`, and `unit` and *compound type*s, which are types that contain other types in their definition. There are really only three types of building blocks to construct compound types:
* "Each-of" (product types): A compound type `t` describes values that contain *each of* values of type `t1`, `t2`, ..., ***and*** `tn`. Tuples are "each-of" types. In OOP languages, a class defines an "each-of" type.
* "One-of" (sum types): A compound type `t` describes values that contain a value of *one of* the types `t1`, `t2`, ..., ***or*** `tn`. Options and Scala `Either` are "one-of" types. In OOP languages, "one-of" is achieved by subclassing.
* "Self-reference" (recursive types): A compound type `t` may refer to itself in its definition in order to describe recursive data structures like lists and trees.

## Records
Record types are “each-of” types where each component is a *named field*. The syntax for a record expression is `{f1 = e1, ..., fn = en}` where, as always, each `ei` can be any expression. Here each `fi` can be any field name (though each must be different). A field name is basically any sequence of letters or numbers.

Type-check each expression to get some type `ti` and then build the record type that has all the right fields with the right types. A *record expression* builds a *record value*. A record value has type `{f1 : t1, ..., fn : tn}`. Note that the order of field names never matters.

The evaluation rules for record expressions are analogous: Evaluate each expression to a value and create the corresponding record value.

## By name vs. By Position
Records and tuples are very similar. They are both "each-of" constructs that allow any number of components. The only real difference is that records are defined and accessed "by name" (and thus position doesn't matter) and tuples are defined and accessed "by position" (and thus have no name).

By name versus by position is a classic decision when designing a language construct or choosing which one to use, with each being more convenient in certain situations. As a rough guide, by position is simpler for a small number of components, but for larger compound types it becomes too difficult to remember which position is which.

Java method arguments (and ML function arguments as we have described them so far) actually take a hybrid approach: The method body uses variable *names* to refer to the different arguments, but the caller passes arguments by *position*. There are other languages where callers pass arguments by name.

## Tuple as Syntactic Sugar
A tuple *is* a record. A tuple expression `(e1,...,en)` is a record expression `{1=e1,...,n=en}` and the type `t1 * ... * tn` is another way of writing `{1:t1, ..., n:tn}`.

This is an example of ***syntactic sugar***. It is *syntactic* because we can describe everything about tuples in terms of equivalent record syntax. It is *sugar* because it makes the language sweeter. Syntactic sugar is a great way to keep the key ideas in a programming language small (making it easier to implement without additional *semantics*) while giving programmers convenient ways to write things.

## Datatype Bindings and Case Expressions
Datatype bindings is a kind of binding that defines "one-of" types. The syntax is

```sml
datatype t = C1 of t1 | C2 of t2 | ... | Cn of tn
```

introduces a new type `t` and each constructor `Ci` is a function of type `ti -> t`. Constructors can omit the `of ti` part and become a *value* of type `t`. Datatype bindings adds the following to the environment:

* a new type `t`;
* Constructors of `Ci` that is either a function of type `ti -> t` or a value of type `t`.

A case expression can "get at the pieces" of a datatype `t`. The syntax is

```sml
case e of p1 => e1 | p2 => e2 | ... | pn => en
```
A case expression evaluates `e` to a value `v`, finds the *first* pattern `pi` that matches `v`, and evaluates `ei` to produce the result for the whole case expression. Patterns look like `Ci(x1,...,xn)` where `Ci` is a constructor of `t` (or just Ci if Ci is a value of `t`). Such a pattern matches a value of the form `Ci(v1,...,vn)` and binds each `xi` to `vi` for evaluating the corresponding `ei`. Thus, patterns look like expressions but not expressions.

In one sense, a case-expression is like a more powerful if-then-else expression because it finds the matching pattern in the order of the case expression. But it is powerful in the sense that
* We can never “mess up” and try to extract something from the wrong variant. That is, we will not get exceptions like we get with `hd []`.
* If a case expression forgets a variant, then the type-checker will give a warning message. This indicates that evaluating the case-expression could find no matching branch, in which case it will raise an exception. If you have no such warnings, then you know this does not occur.
* If a case expression uses a variant twice, then the type-checker will give an error message since one of the branches could never possibly be used.
* If you still want functions like `null` and `hd`, you can easily write them yourself. But it is bad style.

Datatype bindings and case expressions can be recursive. For example, the following example defines a syntax tree and the evaluation rule of it:

```sml
datatype exp = Constant of int
             | Negate of exp
             | Add of exp * exp
             | Multiply of exp * exp

fun eval e =
    case e of
             Constant i => i
             | Negate e2 => ~ (eval e2)
             | Add(e1,e2) => (eval e1) + (eval e2)
             | Multiply(e1,e2) => (eval e1) * (eval e2)

(* This expression evaluates to 19 + (-4) = 15 *)
eval (Add (Constant 19, Negate (Constant 4)))
```

Thus, list and option are datatypes. `NONE` and `SOME` are constructors for option, and `[]` and `::` are constructors for lists. `::` is unusual because it is an infix operator (placed between its two operators). Thus, instead of `null`, `hd`, `tl`, `isSome`, and `valOf`, we can use pattern matching:

```sml
fun inc_or_zero intoption =
    case intoption of
        NONE => 0
        | SOME i => i + 1

fun size =
    case xs of
        [] => 0
        | x :: xs' => 1 + size xs'
```

Note that option and list are *polymorphic* - they can be used for carrying values of any type. You can define polymorphic datatypes. In fact, this is *exactly* how options are defined:

```sml
datatype 'a option = NONE | SOME of 'a
```

Such a binding does not introduce a *type* called option. Rather, it makes it so that if `t` is a type, then `t option` is a type. You can also define polymorphic datatypes that take multiple types. For example, here is a binary tree where internal nodes hold values of type `'a` and leaves hold values of type `'b`:

```sml
datatype ('a,'b) tree = Node of 'a * ('a,'b) tree * ('a,'b) tree
                      | Leaf of 'b
```

## Type Synonyms
A type synonym simply creates another name for an existing type that is entirely interchangeable with the existing type. The syntax is `type foo = int`. It is useful to simplify types of tuples and records.

In contrast, datatype bindings introduce a type that is not the same as any existing type. It creates constructors that produces values of this new type.

## Pattern-Matching for Each-Of Types: the Truth of Variable Bindings
Pattern-matching works for each-of types, such as tuples and records. Given a record value `{f1=v1,...,fn=vn}`, the pattern `{f1=x1,...,fn=xn}` matches and binds `xi` to `vi`. Due to the fact that tuple is syntactic sugar, given a tuple `(v1,...,vn)`, the pattern `(x1,...,xn)` matches and binds `xi` to `vi`.

However, using case expressions to bind each part to a variable is poor style because the purpose of case expressions are to distinguish *cases*. In fact, we can use pattern matching in variable bindings, such as `val (x, y, z) = triple`. Moreover, we can use a pattern when defining a function binding and the pattern will be used to introduce bindings by matching against the value passed when the function is called. Therefore, we can rewrite `fun sum triple = e` into `fun sum (x, y, z) = e`! The type of the function `sum` is also confusing: `int * int * int -> int` means a function of three `int` parameters or of a single triple parameter of `int`?

Then we realize there is no difference between passing in a triple to a function or passing in three parameters! **Every function in ML takes exactly one argument!** Every time we write a multi-argument function, we are really writing a one-argument function that takes a tuple as an argument and uses pattern-matching to extract the pieces.

This flexibility is sometimes useful. In languages like C and Java, you cannot have one function/method compute the results that are immediately passed to another multi-argument function/method. But with one-argument functions that are tuples, this works fine. More generally, you can compute tuples and then pass them to functions even if the writer of that function was thinking in terms of multiple arguments.

What about zero-argument functions? They do not exist either. The binding `fun f () = e` is using the unit-pattern `()` to match against calls that pass the unit value `()`, which is the only value of type `unit`. The type `unit` is just a datatype with only one constructor, which takes no arguments and uses the unusual syntax `()`. Basically, `datatype unit = ()` comes pre-defined.

## Type Inference, Polymorphic Types and Equality Types
By using patterns to access values of tuples and records rather than `#foo`, you will find it is no longer necessary to write types on your function arguments. In fact, it is conventional in ML to leave them off. The reason we needed them before is that `#foo` does not give enough information to type-check the function because the type-checker does not know what other fields the record is supposed to have, but the record/tuple patterns introduced above provide this information. In ML, every variable and function has a type (or your program fails to type-check) - type inference only means you do not need to write down the type. Type inference sometimes reveals that functions are more *general* than you might have thought.

If you can take a type containing `'a`, `'b`, `'c`, etc. and replace each of these type variables consistently to get the type you “want,” then you have a more ***general*** type than the one you want.

In addition, you may also see type variables with two leading apostrophes, like `''a`. These are called *equality types*. The `=` operator in ML (for comparing things) works for many types, not just `int`, but its two operands must have the same type. But you can only substitute `''a` with types that `=` operator supports.

## Nested Patterns
It turns out the definition of patterns is recursive: anywhere we have been putting a variable in our patterns, we can instead put another pattern.

In general, pattern-matching is about taking a *value* and a *pattern* and
1. deciding if the pattern matches the value and
2. if so, binding variables to the right parts of the value.

Here are some key parts to the elegant recursive definition of pattern matching:
* A variable pattern `x` matches any value `v` and introduces one binding (from `x` to `v`).
* The *wildcard* pattern `_` matches anything and introduces no bindings.
* The constant pattern `v` matches the value `v` (say integer 37 matches 37) and introduces no bindings.
* The pattern `C` matches the value `C`, if `C` is a constructor that carries no data.
* The pattern `C p` where `C` is a constructor and `p` is a pattern matches a value of the form `C v` (notice the constructors are the same) if `p` matches `v` (i.e., the nested pattern matches the carried value). It introduces the bindings that `p` matching `v` introduces.
* The pattern `(p1,p2,...,pn)` matches a tuple value `(v1,v2,...,vn)` if `p1` matches `v1` and `p2` matches `v2`, ..., and `pn` matches `vn`. It introduces all the bindings that the recursive matches introduce. Note that you cannot use the *same* variable name in the nested pattern matching and compiler will throw a compile-time error.
* (A similar case for record patterns of the form `{f1=p1,...,fn=pn}` ...)

The recursive definition of pattern matching means that we can use nested patterns instead of nested case expressions when we want to match only values that have a certain "shape."

The following example shows nested pattern matching:

```sml
exception BadTriple

fun zip3 list_triple = case list_triple of
    ([],[],[]) => []
  | (hd1::tl1,hd2::tl2,hd3::tl3) => (hd1,hd2,hd3)::zip3(tl1,tl2,tl3)
  | _ => raise BadTriple

fun unzip3 lst =
    case lst of
        [] => ([],[],[])
      | (a,b,c)::tl => let val (l1,l2,l3) = unzip3 tl
                       in
                           (a::l1,b::l2,c::l3)
                       end
```

## Multiple Cases in a Function Binding
We have seen pattern matching in function bindings for "each-of" types. ML has special syntax for matching one-of types in function definitions.

```sml
fun f p1 = e1
  | f p2 = e2
  ...
  | f pn = en
```
is just *syntactic sugar* for:

```sml
fun f x =
   case x of
     p1 => e1
   | p2 => e2
   ...
   | pn => en
```

For example,

```sml
fun append e =
    case e of
       ([],ys) => ys
     | (x::xs',ys) => x :: append(xs',ys)
```

## Exceptions
ML has a built-in notion of exception. You can *raise* (also known as *throw*) an exception with the `raise` primitive. You can create your own kinds of exceptions with an exception binding. Exceptions can optionally carry values with them, which let the code raising the exception provide more information:

```sml
exception MyUndesirableCondition
exception MyOtherException of int * int
```

Kinds of exceptions are a lot like constructors of a datatype binding. Indeed, they are functions (if they carry values) or values (if they don't) that create values of type `exn` rather than the type of a datatype.

The other feature related to exceptions is *handling* (also known as *catching*) them. For this, ML has `handle` expressions, which look like `e1 handle p => e2` where `e1` and `e2` are expressions and `p` is a pattern that matches an exception. The semantics is to evaluate `e1` and have the result be the answer. But if an exception matching `p` is raised by `e1`, then `e2` is evaluated and that is the answer for the whole expression. If `e1` raises an exception that does not match `p`, then the entire handle-expression also raises that exception. Similarly, if `e2` raises an exception, then the whole expression also raises an exception.

As with case-expressions, handle-expression can also have multiple branches each with a pattern and expression, syntactically separated by `|`.

## Tail recursion and Accumulators
Function calls are implemented, conceptually, with a *call stack*, which is a stack (the data structure with push and pop operations) with one element for each function call that has been started but has not yet completed. Each element, called stack-frame, stores things like the value of local variables and what part of the function has not been evaluated yet.

For recursive calls, the call stack grows with recursion, unless *there is nothing more for the caller to do after the callee returns except return the callee's result*. This situation is called a *tail call* and functional languages like ML typically promise an essential optimization: When a call is a tail call, the caller's stack-frame is popped *before* the call - the callee's stack-frame just replaces the caller's. By doing so, recursion can sometimes be as efficient as a while-loop, which also does not make the call stack bigger.

Using an accumulator is a common way to turn a recursive function into a "tail-recursive function". The accumulator holds the result of the recursion. It is initialized at the *bottom* (i.e., the beginning of the recursion) of the call stack, and holds the final result when reaching the *top* of the call stack (i.e., end condition of the recursion). In general, converting a non-tail-recursive function to a tail-recursive function usually needs associativity, but many functions are associative.

Then with calls are tail calls? We can be more precise by defining *tail position* recursively and saying a call is a tail call if it is in a tail position. The definition has one part for each kind of expression; here are several parts:
* In `fun f(x) = e`, e is in tail position.
* If an expression is not in tail position, then none of its subexpressions are in tail position.
* If `if e1 then e2 else e3` is in tail position, then `e2` and `e3` are in tail position (but not `e1`). (Case-expressions are similar.)
* If `let b1 ... bn in e end` is in tail position, then `e` is in tail position (but no expressions in the bindings are).
* Function-call arguments are not in tail position.
* ...

# Section 3
*First-class* functions can be used (i.e. computed, passed, stored, etc.) wherever values can. Function closures refers to functions that use variables defined outside of them. *Higher-order function* is a function that takes or returns other functions.

The term *functional programming* is used to describe many concepts, the most important and the most common of which are:
* Not using mutable data in most or all cases: We have avoided mutation throughout the course so far and will mostly continue to do so.
* Using functions as values (i.e. provide first class functions).

A functional language is one where writing in a functional style is more convenient, more natural, and more common than programming in other styles (such as OOP). At a minimum, you need good support for immutable data, first-class functions, and function closures.

## Functions as Arguments and Polymorphic Types
The most common use of first-class functions is passing them as arguments to other functions. For example,

```sml
fun n_times (f,n,x) = if n=0
    then x
    else f (n_times(f,n-1,x))

fun double x = x+x
val x1 = n_times(double,4,7)
```
It lets us *abstract* the common parts of multiple computations so we can reuse some code in different ways by passing in different arguments.

The type of `n_times` is `('a -> 'a) * int * 'a -> 'a`. This is called *parametric polymorphism*, or perhaps more commonly *generic types*. It lets functions take arguments of any type. This makes our higher-order functions more useful.

Note that it is not related to first-class functions: there are higher-order functions that are not polymorphic and polymorphic functions that do not take functions.

## Anonymous Functions
If a function is defined only to pass into a higher-order function, then it does not need to be defined at top level. It can be defined locally in let-expressions:

```sml
fun triple_n_times (n,x) =
    let fun triple x = 3*x
    in
        n_times(triple,n,x)
    end
```
We could give the `triple` function an even smaller scope: we need it only as the first argument to `n_times`:

```sml
fun triple_n_times (n,x) = n_times((let fun triple y = 3*y in triple end), n, x)
```

However, this is poor style as there is no need to introduce a function *binding* and pass it into a higher-order function. We can use anonymous function, an *expression* that evaluates to a function:

```sml
fun triple_n_times (n,x) = n_times((fn y => 3*y), n, x)
```

The `fn` (not `fun`) is a keyword and `=>` (not `=`) is the syntax.

The only thing you cannot do with an anonymous function is recursion, exactly because you have no name to use for the recursive call. For non-recursive functions, you could use anonymous functions with `val` bindings instead of a `fun` binding. For example, these two bindings are exactly the same thing:

```sml
fun increment x = x + 1
val increment = fn x => x+1
```

## Lexical Scope
Functions can use any bindings in scope. Doing so in combination with higher-order functions is very powerful, but understanding what is in scope is crucial. *The body of a function is evaluated in the environment where the function is **defined**, not the environment where the function is **called**.* This semantics is called ***lexical scope***. The alternate, inferior semantics where you use the current environment is called *dynamic scope*.

We have said that functions are values, but we have not been precise about what that value exactly is. We now explain that a function value has *two parts*, the *code* for the function and the *environment that was current when we created the function*. Call to a function uses both parts because it evaluates the *code* part using the *environment* part. The two parts combined is called a ***function closure*** or just *closure*. The reason is that while the code itself can have *free variables* (variables that are not *bound* inside the code so they need to be bound by some outer environment), the closure carries with it an environment that provides all these bindings.

Lexical scope is the preferred way, because
1. Function meaning does not depend on variable names used, as changing them does not have any effect except the environment in the closure.
2. Functions can be type-checked and reasoned about where defined.
3. Closures can easily store the data they need.

But exception handling is more like dynamic scope than lexical scope. When an exception is raised, evaluation has to "look up" which handle expression should be evaluated. This "look up" is done using the dynamic call stack, with no regard for the lexical structure of the program.

## Famous Higher-Order Functions
Map, filter, and fold are most common higher-order functions.

```sml
(* val map = fn : ('a -> 'b) * 'a list -> 'b list *)
fun map (f,xs) =
    case xs of
	[] => []
      | x::xs' => (f x)::(map(f,xs'))

(* val filter = fn : ('a -> bool) * 'a list -> 'a list *)
fun filter (f,xs) =
    case xs of
      	[] => []
      | x::xs' => if f x
         then x::(filter (f,xs'))
         else filter (f,xs')

(* val fold = fn : ('a * 'b -> 'a) * 'a * 'b list -> 'a *)
fun fold (f,acc,xs) =
    case xs of
        [] => acc
      | x::xs' => fold (f,f(acc,x),xs')
```

This pattern of splitting the recursive traversal (`fold` or `map`) from the data-processing done on the elements (the closures passed in) is fundamental. More generally, we may have a very complicated set of data structures to traverse or we may have very involved data processing to do. It is good to *separate these concerns* so that the programming problems can be solved separately.

## Function Composition
It is useful to create new functions that are just combinations of other functions. It is called function composition in mathematics.

```sml
(* val compose : fn ('a -> 'b) * ('c -> 'a) -> 'c -> 'b *)
fun compose (f,g) = fn x => f (g x)
```
It takes two functions `f` and `g` and returns a function that applies its argument to `g` and makes that the argument to `f`.

The ML library defines the infix operator `o` as function composition, just like in math, so

```sml
fun sqrt_of_abs i = Math.sqrt(Real.fromInt (abs i))

(* Use o infix operator for function composition *)
fun sqrt_of_abs i = (Math.sqrt o Real.fromInt o abs) i

(* Without unnecessary function wrapping *)
val sqrt_of_abs = Math.sqrt o Real.fromInt o abs
```
However, you need to read the example from right to left, which is not intuitive. We can define convenient syntax to do this left-to-right (i.e., the function is on the right of the argument):

```sml
infix |> (* tells the parser |> is a function that appears between its two arguments *)
fun x |> f = f x

fun sqrt_of_abs i = i |> abs |> Real.fromInt |> Math.sqrt
```
This operator `|>`, commonly called the pipeline operator, is very popular in F# programming. F# is a dialect of ML that runs on .Net and interacts well with libraries written in other .Net languages.

## Currying and Partial Functions
Every ML function takes exactly one argument, so previously tuples are used to pass in multiple parameters. More clever and often more convenient way is to have a function take the first conceptual argument and return another function that takes the second conceptual argument and so on. Lexical scope is essential to this technique working correctly. This technique is called ***currying** after a logician named Haskell Curry.

```sml
(* old way to get the effect of multiple arguments *)
fun sorted3_tupled (x,y,z) = z >= y andalso y >= x

val t1 = sorted3_tupled (7,9,11)

(* new way: currying *)
val sorted3 = fn x => fn y => fn z => z >= y andalso y >= x

val t2 = ((sorted3 7) 9) 11

(* syntactic sugar for calling curried functions: optional parentheses *)
val t3 = sorted3 7 9 11

(* syntactic sugar for defining curried functions: space between arguments *)
fun sorted3_nicer x y z = z >= y andalso y >= x

val t4 = sorted3_nicer 7 9 11
```

There are two syntactic sugar that makes currying much more elegant:
1. Optional parentheses when calling curried functions. In general, the syntax `e1 e2 e3 e4` is implicitly the nested function calls `(((e1 e2) e3) e4)` and this choice was made because it makes using a curried function so pleasant.
2. `fun f p1 p2 p3 ... = e` means `fun f p1 = fn p2 => fn p3 => ... => e`.

Currying returns a function with a closure in the intermediate steps, calling a curried function with fewer arguments can return a ***partial function*** that can be called with the remaining arguments later but using the closure defined earlier. For example, we can use the curried form of `fold` to generate partial functions:

```sml
fun fold f acc xs = (* means fun fold f = fn acc => fn xs => *)
  case xs of
    []     => acc
  | x::xs' => fold f (f(acc,x)) xs'

val sum = fold (fn (x,y) => x+y) 0

(* Unnecessary function wrapping *)
fun sum_inferior xs = fold (fn (x,y) => x+y) 0 xs
```

A lot of iterator functions are written using currying for partial application later. For example,

```sml
fun exists predicate xs =
    case xs of
      [] => false
    | x::xs' => predicate x orelse exists predicate xs'

val hasZero = exists (fn x => x=0)
```

One strange warning in partial function when the result function is polymorphic is "value restriction". To get around that, add the unnecessary function wrapping or explicitly specifying the function type:

```sml
(* Doesn't work. val pairWithOne : fn = 'a list -> ('a * int) list *)
val pairWithOne = List.map (fn x => (x,1))

(* workarounds: *)
fun pairWithOne xs = List.map (fn x => (x,1)) xs

val pairWithOne : string list -> (string * int) list = List.map (fn x => (x,1))
```

Note that caller and callee must use either tuples or currying together; you cannot mix and match. But it is easy to convert between the two forms:

```sml
fun curry f x y = f (x,y)
fun uncurry f (x,y) = f x y
```

## References
Mutation is okay in some settings. A key approach in functional programming is to use it only when "updating the state of something so all users of that state can see a change has occurred" is the natural way to model your computation. Moreover, we want to keep features for mutation separate so that we know when mutation is not being used.

*Reference* is a container whose contents can be changed. Create a reference with `ref e` (of type `t ref`), get the content of the reference using `!r`, and update reference's content using `r := e` where `e` has type `t`.

## Callbacks
Callback is a common idiom that allows clients to register functions to be executed when an event occurs. Library implementer has no idea what data callback functions need, so closure is ideal for this. Of course, without mutation (i.e., references), callbacks are meaningless as they cannot return values. Thus, callbacks often mutate data in their closures.

## Abstract Data Types using Closures
A data type or simply type is an attribute of data which tells the compiler or interpreter how the programmer intends to use the data. An **abstract data type** (ADT) is a mathematical model for data types, where a data type is defined by its behavior (semantics) from the point of view of a *user* of the data, specifically in terms of possible values, possible operations on data of this type, and the behavior of these operations. This contrasts with data structures, which are concrete representations of data, and are the point of view of an *implementer*, not a user.

The keyword "Abstract" is used as we can use these datatypes, we can perform different operations. But how those operations are working that is totally hidden from the user.

In an object-oriented language, you might implement an ADT by defining a class with all private fields (inaccessible to clients) and some public methods (the interface with clients). We can do the same thing in ML with a record of closures that share the same environment; the variables that the closures use from the environment correspond to the private fields.

```sml
(* Use a record of functions as definition of ADT *)
(* datatype and S constructor is needed so the definition can be recursive *)
datatype set = S of { insert : int -> set, member : int -> bool, size : unit -> int }

(* implementation of sets: this is the fancy stuff, but clients using
   this abstraction do not need to understand it *)
val empty_set =
    let
        fun make_set xs = (* xs is a "private field" in result *)
            let (* contains a "private method" in result *)
                fun contains i = List.exists (fn j => i=j) xs
            in
                S { insert = fn i => if contains i
                                     then make_set xs
                                     else make_set (i::xs),
                    member = contains,
                    size   = fn () => length xs
                  }
            end
    in
        make_set []
    end
```

## Implementing Closures in Non-Functional Languages
In OOP languages like Java, closures can be implemented using Single Abstract Method (SAM) interfaces. These interfaces act like functions in ML, and private fields in the concrete classes of these interfaces are the environment in closures. In fact, this is the idea of Java 8's functional interface.

In procedural languages like C, the environment of closures can be passed in as an additional parameter to functions to simulate closure. Generally speaking, a field of type `void *` is passed to functions as the environment, and function pointers can be used as higher-order functions.

# Section 4
## Type Inference
*Statically typed languages* define the type of every binding at compile time, i.e., before any part of the program is run. *Dynamically typed languages* do not determine types ahead of time but rely on run-time checks.

ML is *implicitly typed*, meaning programmers rarely need to write down the types of bindings. The type-checker must *infer* what *type annotations* would have been applied to each binding. In principle, type inference and type checking could be separate steps, but in practice they are often merged into “the type-checker.” Note that a correct type-inferencer must find a solution to what all the types should be whenever such a solution exists, else it must reject the program.

ML type inference ends up intertwined with parametric polymorphism, but type inference and polymorphism are entirely separate concepts: a language could have one or the other.

Here is an overview of how ML type inference works:
* It determines the types of bindings in order, using the types of earlier bindings to infer the types of later ones. This is why you cannot use later bindings in a file.
* For each `val` or `fun` binding, it analyzes the binding to determine necessary facts about its type. For example, if we see the expression `x+1`, we conclude that `x` must have type `int`. We gather similar facts for function calls, pattern-matches, etc.
* Afterward, use type variables (e.g., `'a`) for any unconstrained types in function arguments or results.
* Enforce the value restriction.

However, this system is unsound. For example,

```sml
val r = ref NONE (* 'a option ref *)
val _ = r := SOME "hi" (* instantiate 'a with string *)
val i = 1 + valOf(!r) (* instantiate 'a with int *)
```

The problem results from a combination of polymorphic types and mutable references, and the fix is a special restriction to the type system called the *value restriction*: a variable in a val-binding can be assigned a polymorphic type only if the expression in the val-binding is a value or a variable. Otherwise, a dummy type is assigned so the type-checking will fail. In our example, `ref NONE` is a call to the function `ref`. Function calls are not variables or values. So we get a warning and `r` is given a type `?X1 option ref` where `?X1` is a "dummy type," not a type variable.

Type inference in ML would be more difficult if
* ML *did not* have polymorphism as more type restrictions must be figured out by the type-inferencer. Some types must be assigned for `length` or `compose`.
* ML had subtyping (e.g., if every triple could also be a pair). This could be done, but type inference is more difficult and the results are more difficult to understand.

## Mutual Recursion
Because bindings are evaluated in order, it requires special language feature to allow mutual recursion, i.e., two functions `f` and `g` to call each other in their bodies. Similarly, we can have mutually recursive datatype bindings.

Mutual recursive functions can be defined by replacing the keyword `fun` for all functions except the first with `and`. The type-checker will type-check all the functions (two in the example above) together, allowing calls among them regardless of order. The syntax is the same for datatype bindings.

```sml
(* f1, f2, ..., fn are type-checked together. *)
fun f1 p1 = e1
and f2 p2 = e2
...
and fn pn = en

datatype t1 = C1 of t1 | C2 of t2
and t2 = C3 of t1 | C4 of t2
```

## Modules
In ML, we can use **structures** to define **modules** that contain a collection of bindings. At its simplest, you can write `structure Name = struct bindings end` where `Name` is the name of your structure (you can pick anything; capitalization is a convention) and `bindings` is any list of bindings, containing values, functions, exceptions, datatypes, and type synonyms. Modules can be used to separate bindings into different **namespaces**.

Inside the structure you can use earlier bindings just like we have been doing "at top-level" (i.e., outside of any module). Outside the structure, you refer to a binding `b` in Name by writing `Name.b`.

To avoid repetition of writing `Name.b` many times, you can create a val-binding. Or you can port *all* bindings into your environment with `open Name`. This is potentially dangerous as bindings inside the "opened" module can shadow any existing bindings you already have.

Structure *signatures* are types for modules. This provides strict *interfaces* to code outside the module.

```sml
(* Signature names are capitalized *)
signature MATHLIB =
sig
val fact : int -> int (* Use val bindings for function types *)
val half_pi : real
val doubler : int -> int
end

(* :> MATHLIB requires MyMathLib to provide everything defined in signature *)
structure MyMathLib :> MATHLIB =
struct
fun fact x =
    if x=0
    then 1
    else x * fact (x - 1)

val half_pi = Math.pi / 2.0

fun doubler y = y + y
end
```
Signatures can also contain datatype, exception, and type bindings. In addition, signatures can hide bindings inside modules to the outside. Bindings not defined in the signature are not in scope to the outside code. We can implement the module however we like and only bindings that are explicitly listed in the signature can be called directly by clients.

Signature can contain *abstract types*. This is useful when signatures contain functions that take or return a datatype but the implementation of the datatype should be hidden. Abstract type can be defined by `type rational` in the signature definition.

As a twist, certain *constructors* of the abstract type can be *exported* in the signature: just define the binding of the constructor without changing the implementation! For example:

```sml
signature RATIONAL =
sig
type rational (* type still abstract *)
val Whole : int -> rational (* client knows only that Whole is a function *)
end

structure Rational >: RATIONAL =
struct
datatype rational = Frac of int * int | Whole of int
end
```
The constructor `Whole` of the `rational` datatype is visible to the outside.

If a structure does not match a signature assigned to it, then the module does not type-check. A structure `Name` matches a signature `BLAH` if:
* For every val-binding in `BLAH`, Name must have a binding with that type or a more general type (e.g., the implementation can be polymorphic even if the signature says it is not). This binding could be provided via a val-binding, a fun-binding, or a datatype-binding.
* For every non-abstract type-binding in `BLAH`, `Name` must have the same type binding.
* For every abstract type-binding in `BLAH`, `Name` must have some binding that creates that type (either a datatype binding or a type synonym).

Multiple modules with the same signature define different types. For example, if module `Rational1` and `Rational2` both use `RATIONAL` signature, `Rational1.rational` and `Rational2.rational` are different types and they cannot be used interchangeably.

## Equivalence
The idea that one piece of code is "equivalent" to another piece of code is fundamental to programming and computer science.
* Code maintenance: Can you simplify, clean up, or reorganize code without changing how the rest of the program behaves?
* Backward compatibility: Can you add new features without changing how any of the existing features work?
* Optimization: Can you replace code with a faster or more space-efficient implementation?
* Abstraction: Can an external client tell if I make this change to my code?

The intuition behind our definition is as follows:
* A function `f` is equivalent to a function `g` (or similarly for other pieces of code) if they produce the same answer and have the same side-effects no matter where they are called in any program with any arguments.
* Equivalence does not require the same running time, the same use of internal data structures, the same helper functions, etc. All these things are considered "unobservable", i.e., implementation details that do not affect equivalence.

One easy way to make sure two functions have the same side effects (mutating references, doing input/output, etc.) is to have no side effects at all. This is exactly what functional languages like ML encourage.

There are several guaranteed equivalence:
1. Syntactic sugar.
2. Variable renaming. Note, such renaming should not shadow other bindings.
3. Helper functions.
4. Unnecessary function wrapping.
5. `let val p = e1 in e2 end` can be sugar for `(fn p => e2) e1`.
