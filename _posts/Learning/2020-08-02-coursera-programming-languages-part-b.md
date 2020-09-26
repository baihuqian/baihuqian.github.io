---
layout: post
title: 'Coursera Programming Languages, Part B'
date: '2020-08-02 17:24'
tags:
  - Computer Science
full-width: true
---

This is the course notes I took when studying [Programming Languages (Part B)](https://www.coursera.org/learn/programming-languages-part-b), offered by Coursera.

This course is based on [Part A]({{ site.baseurl }}{% link _posts/Learning/2020-04-07-coursera-programming-languages-part-a.md %}) and subsequently I studied [Part C]({{ site.baseurl }}{% link _posts/Learning/2020-08-14-coursera-programming-languages-part-c.md %}).

* toc
{:toc}

# Section 5
We will use the Racket programming language in this part. Racket is derived from Scheme, a well-known programming language that has evolved since 1975. (Scheme in turn is derived from LISP, which has evolved since 1958 or so.)

The first line of a Racket file (which is also a Racket module) should be

```
#lang racket
```
This tells DrRacket environment to interpret the file as Racket code (DrRacket can interpret other languages or language definitions of your own).

Racket's single-line comment starts with `;` and goes to the end of line. It is legal to use hyphen `-` to separate words in variable and function names, and it is the convention.

## Definition
`(define x e)` where `x` is a variable and `e` is an expression, we evaluate `e` to a value and change the environment so that `x` is bound to that value. Racket uses parentheses extensively. In Racket, *everything* is prefix, such as `(+ 2 2)` (`+` is a function) is an expression evaluated to `4`.

An anonymous function that takes one argument is written `(lambda (x) e)` where the argument is the variable `x` and the body is the expression `e`. A function binding can be defined by binding the anonymous function to a variable. In Racket, different functions really take different numbers of arguments and it is a run-time error to call a function with the wrong number. A three argument function would look like `(lambda (x y z) e)`.

Many functions can take any number of arguments. The multiplication function, `*`, is one of them, so we can write a cubic function like this:

```
(define cube2
  (lambda (x)
    (* x x x)))
```

There is a very common form of syntactic sugar you should use for defining functions. It does not use the word lambda explicitly:

```
(define (cube3 x)
  (* x x x))
```

## Condition
An `if`-expression has the general syntax `(if e1 e2 e3)`. It evaluates `e1`. If the result is `#f` (Racket’s constant for false), it evaluates `e3` for the result. If the result is *anything else*, including `#t` (Racket’s constant for true), it evaluates `e2` for the result. Notice how this is much more flexible type-wise than anything in ML.

The `cond` expression is better style for nested conditionals than multiple `if`-expressions. A `cond` just has any number of parenthesized pairs of expressions, `[e1 e2]`. The `e1` is a test; if it evaluates to `#f` we skip to the next branch. Otherwise we evaluate `e2` and that is the answer. As a matter of style, your last branch should have the test `#t`, so you do not “fall off the bottom” in which case the result is some sort of “void object” that you do not want to deal with. Note that it is by convention to use `[]` for each pair of expressions.

As with `if`, the result of a test does not have to be `#t` or `#f`. Anything other than `#f` is interpreted as true for the purpose of the test. It is sometimes bad style to exploit this feature, but it can be useful.

## Recursion
Unlike ML, you can use recursion with anonymous functions because the definition itself is in scope in the function body. We can define the `pow` function like this:

```
(define (pow1 x y)
  (if (= y 0)
      1
      (* x (pow1 x (- y 1)))))
```

We can use currying in Racket. After all, Racket’s first-class functions are closures like in ML and currying is just a programming idiom.
```
(define pow2
  (lambda (x)
    (lambda (y)
      (pow1 x y))))

(define three-to-the (pow2 3))
(define eightyone (three-to-the 4))
(define sixteen ((pow2 2) 4))
```
Because Racket’s multi-argument functions really are multi-argument functions (not sugar for something else), currying is not as common. There is no syntactic sugar for calling a curried function: we have to write `((pow2 2) 4)` because `(pow 2 4)` calls the one-argument function bound to `pow` with two arguments, which is a run-time error. Racket has added sugar for defining a curried function: `(define ((pow x) y) e)`.

## Lists
Racket has built-in lists, much like ML, and Racket programs probably use lists even more often in practice than ML programs.

Primitive | Description                                       | Example
----------|---------------------------------------------------|-------------------------
`null`    | The empty list                                    |
`cons`    | Construct a list                                  | `(cons 2 (cons 3 null))`
`car`     | Get the head of a list                            | `(car a-list)`
`cdr`     | Get the tail of a list                            | `(cdr a-list)`
`null?`   | Return `#t` for the empty-list and `#f` otherwise | `(null? a-list)`
`list`    | Build a list from any number of elements          | `(list 2 3 4)`

A few list-processing functions can be written like this:

```
(define (sum xs)
  (if (null? xs)
      0
      (+ (car xs) (sum (cdr xs)))))

(define (my-append xs ys)
  (if (null? xs)
      ys
      (cons (car xs) (my-append (cdr xs) ys))))

(define (my-map f xs)
  (if (null? xs)
      null
      (cons (f (car xs)) (my-map f (cdr xs)))))
```

## `cons` Cells
The truth is `cons` just makes a *pair*, which is also called `cons` cell, where you get the first part with `car` and the second part with `cdr`.

A *list* is, by convention and according to the `list?` predefined function, either `null` or a pair where the `cdr` (i.e., second component) is a list. A `cons` cell that is not a list is often called an *improper list*, especially if it has nested `cons` cells in the second position.

Most list functions like `length` will give a run-time error if passed an improper list. On the other hand, the built-in `pair?` primitive returns true for anything built with `cons`, i.e., any improper or proper list except the empty list.

Pairs are a generally useful way to build an *each-of* type, i.e., something with multiple pieces. And in a dynamically typed language, all you need for lists are pairs and some way to recognize the end of the list, which by convention Racket uses the `null` constant (which prints as `’()`) for. As a matter of style, you should use proper lists and not improper lists for collections that could have any number of elements.

Cons cells are immutable: When you create a cons cell, its two fields are initialized and will never change. The Racket implementation can be clever enough to make `list?` a constant-time operation since it can store with every cons cell whether or not it is a proper list when the cons cell is created.

## Syntax and Parentheses
With a few exceptions, Racket has an amazingly simple syntax. Everything in the language is either:
* Some form of *atom*, such as `#t`, `#f`, `34`, `"hi"`, `null`, etc. A particularly important form of atom is an *identifier*, which can either be a variable (e.g., `x` or `something-like-this!`) or a special form such as `define`, `lambda`, `if`, and many more.
* A sequence of things in parentheses `(t1 t2 ... tn)`. The first thing in a sequence affects what the rest of the sequence means. If the first thing in a sequence is not a special form and the sequence is part of an expression, then we have a function call.

By “parenthesizing everything” Racket has a syntax that is **unambiguous**. There is no operator or binding priorities. It makes parsing, converting the program text into a tree representing the program structure, trivial. Everything inside a pair of parentheses form a sub-tree. This structure is very similar to HTML.

As a minor note, Racket also allows `[` and `]` in place of `(` and `)` anywhere, as long as they are matched accordingly (`(` must be matched by `)` and `[` by `]`).

From the standpoint of learning about programming languages and fundamental programming constructs, you should recognize a strong opinion about parentheses (either for or against) as a syntactic prejudice. *Parentheses change the meaning of your Racket program. You cannot add or remove them because you feel like it. They are never optional or meaningless.* For example, in expressions, `(e)` means evaluate `e` and then call the resulting function with 0 arguments.

## Dynamic Typing
Racket does not use a static type system to reject programs before they are run. Code with mismatched types only errors out when it gets executed. Because of dynamic typing, Racket list can hold arbitrary types. It also provides predicates for type testing, such as `null?` or `number?`.

## Local Bindings
Racket provides four ways of defining local bindings, compared to the single `let` expression in ML. The variety is in the **scope and environment** that the bindings are added.

First, there is the expression of the form
```
(let ([x1 e1]
      [x2 e2]
      ...
      [xn en])
  e)
```
This creates local variables `x1`, `x2`, ... `xn`, bound to the results of evaluating `e1`, `e2`, ..., `en`, and then the body `e` can use these variables (i.e., they are in the environment) and the result of `e` is the overall result. Syntactically, notice the **extra** parentheses around the collection of bindings and the common style of where we use square parentheses.

The key question is *what environment do we use to evaluate `e1`, `e2`, ..., `en`*? It uses the environment from **before** the `let`-expression. That is, later variables in the same `let`-expression do not have earlier ones in their environment. If `e3` uses `x1` or `x2`, that would either be an error or would mean some outer variable of the same name. This is **not** how ML `let`-expressions work.

The ML-equivalent one, which evaluates each binding’s expression in the environment *produced from the previous ones*, is `let*`.

Neither `let` nor `let*` allows recursion since the `e1`, `e2`, ..., `en` cannot refer to the binding being defined or any later ones. To do so, we have a third variant `letrec`. One typically uses `letrec` to define one or more (mutually) recursive functions. For example:

```
(define (mod2 x)
  (letrec
      ([even?(lambda (x) (if (zero? x) #t (odd? (- x 1))))]
       [odd? (lambda (x) (if (zero? x) #f (even? (- x 1))))])
    (if (even? x) 0 1)))
```

Alternately, you can get the same behavior as `letrec` by using local `define`s, which is very common in real Racket code and is in fact the preferred style over let-expressions.

```
(define (mod2_b x)
  (define even? (lambda(x)(if (zero? x) #t (odd? (- x 1)))))
  (define odd?  (lambda(x)(if (zero? x) #f (even? (- x 1)))))
  (if (even? x) 0 1))
```

We need to be careful with `letrec` and local definitions: They allow code to refer to variables that are initialized later, but the *expressions* for each binding are still evaluated in order. This is never a problem for mutually recursive functions as the function body is not *evaluated* as part of reference in the function definition. However, *variable bindings* cannot refer to other variable definitions in the later part of `letrec` and local definitions.

## Top-Level Definitions
A Racket file is a module with a sequence of definitions. Just as with `let`-expressions, it matters greatly to the semantics what environment is used for what definitions. In ML, a file was like an implicit `let*`. In Racket, it is basically like an implicit `letrec`.

On the other hand, there are a few limitations:
1. You *cannot* have two bindings use the same variable. With `letrec`-like semantics, we do not have one variable shadow another one if they are defined in the same collection of mutually-recursive bindings.
2. Like `letrec`, variable bindings cannot use expressions that refer to later variables.
3. One module can shadow a binding in another file, such as the files implicitly included from Racket’s standard library.

## Mutations
Racket does have assignment statements. If `x` is in your environment, then `(set! x 13)` will mutate the binding so that `x` now maps to the value `13`. Doing so affects all code that has this `x` in its environment, but not *expressions* evaluated prior to the `set!`.

Mutating top-level bindings is particularly worrisome because we may not know all the code that is using the definition. There is a general technique in software development to guard against this: *If something might get mutated and you need the old value, make a copy before the mutation can occur.*

## `Cons` cells are immutable, but there is `mcons`

If we want mutable pairs, though, Racket is happy to oblige with a different set of primitives:
* `mcons` makes a mutable pair
* `mcar` returns the first component of a mutable pair
* `mcdr` returns the second component of a mutable pair
* `mpair?` returns `#t` if given a mutable pair
* `set-mcar!` takes a mutable pair and an expression and changes the first component to be the result of the expression
* `set-mcdr!` takes a mutable pair and an expression and changes the second component to be the result of the expression

## Delayed Evaluation and Thunks
A key semantic issue for a language construct is *when are its subexpressions evaluated*. For example, in Racket (and similarly in ML and most but not all programming languages), function arguments are evaluated once before we execute the function body. However, we do not evaluate the body of a function until the function is called, or neither branches of an `if`-statement until the condition is evaluated.

```
(define (my-if-bad x y z) (if x y z))
```
This function breaks the evaluation order of `if` and requires both branches `y` and `z` to be evaluated before the function is executed.

The general idiom of using a zero-argument function to delay evaluation (do not evaluate the expression now, do it later when/if the zero-argument function is called) is very powerful. As convenient terminology/jargon, when we use a zero- argument function to delay evaluation we call the function a *thunk*. You can even say, “thunk the argument” to mean “use `(lambda () e)` instead of `e`”.

## Lazy Evaluation
While thunks delay evaluations, it is evaluated every time it is called. If we thunk, then we may repeat the large computation many times. But if we do not thunk, then we will perform the large computation even if we do not need to. To get the “best of both worlds,” we can use a programming idiom known by a few different (and perhaps technically slightly different) names: *lazy-evaluation*, *call-by-need*, *promises*. The idea is to use mutation to remember the result from the first time we use the thunk so that we do not need to use the thunk again.

```
(define (my-delay f)
    (mcons #f f))

(define (my-force th)
    (if (mcar th)
        (mcdr th)
        (begin (set-mcar! th #t)
               (set-mcdr! th ((mcdr th)))
               (mcdr th))))

```
We can use a mutable pair to indicate whether the thunk is executed: if the first element is `#t`, then the second element is the result of the thunk; otherwise, the second element is the thunk. Note that it is not easy to understand when the thunk is evaluated. Thus, it is important not to have side effects in the thunk.

Some languages, most notably Haskell, use this approach for all function calls, i.e., the semantics for function calls is different in these languages: If an argument is never used it is never evaluated, else it is evaluated only once. This is called *call-by-need* whereas all the languages we will use are *call-by-value* (arguments are fully evaluated before the call is made).

## Streams
A stream is an infinite sequence of values. You cannot generate the entire stream as it's infinite, but you can generate the *code* that produces the next value infinitely. There are many ways to code up streams; we will take the simple approach of representing a stream as a thunk that when called produces a *pair* of (1) the first element in the sequence and (2) a thunk that represents the stream for the second-through-infinity elements. Defining such thunks typically uses recursion. Here are three examples:

```
(define ones (lambda () (cons 1 ones)))

(define nats
  (letrec ([f (lambda (x) (cons x (lambda () (f (+ x 1)))))])
    (lambda () (f 1))))

(define powers-of-two
  (letrec ([f (lambda (x) (cons x (lambda () (f (* x 2)))))])
    (lambda () (f 2))))
```

Given this encoding of streams and a stream `s`, we would get the first element via `(car (s))`, the second element via `(car ((cdr (s))))`, the third element via `(car ((cdr ((cdr (s))))))`, etc. Remember parentheses matter: (`e)` calls the thunk `e`.

Note that all the streams above can produce their next element given at most their previous element. So we could use a higher-order function to abstract out the common aspects of these functions, which lets us put the stream-creation logic in one place and the details for the particular streams in another.

```
; fn takes two arguments and apply to the current value of the stream and arg
(define (stream-maker fn arg)
    (letrec ([f (lambda (x)
                (cons x (lambda () (f (fn x arg)))))])
    (lambda () (f arg))))

(define ones (stream-maker (lambda (x y) 1) 1))
(define nats (stream-maker + 1))
(define powers-of-two (stream-maker * 2))
```

A few callouts about using recursion to define streams:
1. The recursive call to the stream itself must be thunked. Racket is call-by-value and `(define ones (cons 1 ones))` does not work.
2. Stream must be a thunk that returns a pair with the second argument being a thunk. `(define ones (lambda () (cons 1 (ones))))` results in infinite recursion and thus never returns.

## Memoization
An idiom related to lazy evaluation that does not actually use thunks is memoization. If a function does not have side-effects, then if we call it multiple times with the same argument(s), we do not actually have to do the call more than once. Instead, we can look up what the answer was the first time we called the function with the argument(s).

Whether this is a good idea or not depends on trade-offs. Keeping old answers in a table takes space and table lookups do take some time, but compared to reperforming expensive computations, it can be a big win. Again, for this technique to even be correct requires that given the same arguments a function will always return the same result and have no side-effects. So being able to use this memo table (i.e., do memoization) is yet another advantage of avoiding mutation.

To implement memoization we do use mutation: Whenever the function is called with an argument we have not seen before, we compute the answer and then add the result to the table (via mutation). For example, to implement Fibonacci:

```
(define fibonacci
  (letrec([memo null] ; list of pairs (arg . result)
          [f (lambda (x)
                ; assoc returns the first pair if (= (car pr) x) is true
               (let ([ans (assoc x memo)])
                 (if ans
                     (cdr ans)
                     (let ([new-ans (if (or (= x 1) (= x 2))
                                        1
                                        (+ (f (- x 1))
                                           (f (- x 2))))])
                       (begin
                           ; store the result in memo
                         (set! memo (cons (cons x new-ans) memo))
                         new-ans)))))])
    f))

```

Note the similarity between memoization and dynamic programming. Both Memoization and Dynamic Programming solves individual subproblem only once. Memoization uses recursion and works top-down, whereas Dynamic programming moves in opposite direction solving the problem bottom-up.

## Macros
*Macros* add to the *syntax* of a language by letting programmers define their own syntactic sugar. A *macro definition* introduces some new syntax into the language. It describes how to transform the new syntax into different syntax in the language itself. A *macro system* is a language (or part of a larger languages) for defining macros. A *macro use* is just using one of the macros previously defined. The semantics of a macro use is to replace the macro use with the appropriate syntax as defined by the macro definition. This process is often called *macro expansion* because it is common but not required that the syntactic transformation produces a larger amount of code.

The key point is that macro expansion happens *before* anything else we have learned about: before type-checking, before compiling, before evaluation. Think of “expanding all the macros” as a pre-pass over your entire program before anything else occurs. So macros get expanded everywhere, such as in function bodies, both branches of conditionals, etc.

### Macro Definition
The definition of macros and macro expansion is more structured and subtle than simple find-and-replace.

First, macro works on *tokens* instead of string substitution. So the implementation of macros has to at least understand how a programming language’s text is broken into tokens (i.e., words). This notion of tokens is different in different languages.

Second, we can ask if macros do or do not understand parenthesization. For example, in C/C++, if you have a macro `#define ADD(x,y) x+y` then `ADD(1,2/3)*4` gets rewritten as `1 + 2 / 3 * 4`, which is not the same thing as `(1 + 2/3)*4`. So in such languages, macro writers generally include lots of explicit parentheses in their macro definitions. In Racket, macro expansion preserves the code structure so this issue is not a problem. A Racket macro use always looks like `(x ...)` where `x` is the name of a macro and the result of the expansion “stays in the same parentheses”. This is an advantage of Racket’s minimal and consistent syntax.

Third, we can ask if macro expansion happens even when creating variable bindings. If not, then local variables can shadow macros, which ensures variable binding does not change by macros and is probably what you want.

Here is a macro that lets users write `(my-if e1 then e2 else e3)` for any expressions `e1`, `e2`, and `e3` and have it mean exactly `(if e1 e2 e3)`:

```
(define-syntax my-if ; my-if the name if the macro
    (syntax-rules (then else) ; define the list of keywords in the macro
    ; a list of pairs, each is a way of using the macro
    [(my-if e1 then e2 else e3) ; macro form
     (if e1 e2 e3)])) ; macro expansion
```
Be careful of number of occurrences of each expression in the macro expansion. If the expression is expensive or has side effects, then the expansion will result in evaluating the expression multiple times. If an expression must appear multiple times, consider using local variables in macro definitions to control if/when expressions get evaluated is exactly what you should do, but in less powerful macro languages (again, C/C++ is an easy target for derision here), local variables in macros are typically avoided or have super-funny names. The reason has to do with scope and something that is called **hygiene**.

In Racket, the rule for macro expansion is more sophisticated to avoid this problem. Basically, every time a macro is used, all of its local variables are *rewritten* to be fresh new variable names that do not conflict with anything else in the program. This is “one half” of what by definition make Racket macros hygienic. The other half has to do with free variables in the *macro definition* and making sure they do not wrongly end up in the scope of some local variable where the macro is used. Free variables in a macro definition always refer to what was in the environment where the macro was *defined*, not where the macro was *used*. This makes it much easier to write macros that always work as expected.

# Section 6
## "Datatype Programming" in Racket
In ML, we used datatype-bindings to define our own one-of types. A datatype-binding introduces a new type into the static environment, along with constructors for creating data of the type and pattern-matching for using data of the type.

Racket is a dynamically-typed language so there is no type definition. However, we can implement *constructor*, *type checker*, and *extractor* (which is done by pattern-matching in ML) as Racket functions. We can implement them using own definition of data structures, such as lists, or use `struct`. A struct definition looks like:

```
(struct foo (bar baz quux) #:transparent)
```

It adds
* `foo` constructor that takes three arguments and returns a value that is a foo with a bar field holding the first argument, a baz field holding the second argument, and a quux field holding the third argument.
* `foo?` type checker that takes one argument and returns `#t` for values created by calling `foo` and `#f` for everything else.
* `foo-bar`, `foo-baz`, and `foo-quux` are functions that takes a `foo` and returns the contents of the `bar`, `baz`, and `quux` field, raising an error if passed anything other than a `foo`.

There are some useful *attributes* we can include in struct definitions to modify their behavior:
* `#:transparent` attribute makes the fields and accessor functions visible even outside the module that defines the struct. It's a questionable style in real code, but allows REPL to print struct values with their contents rather than just as an abstract value.
* `#:mutable` attribute makes all fields mutable by also providing mutator functions like `set-foo-bar!`, `set-foo-baz!`, and `set-foo-quux!`.

We can use `struct`s to define an expression and a function to evaluated, as shown in [Datatype Bindings and Case Expressions in Section 3]({{ site.baseurl }}{% link _posts/Learning/2020-04-07-coursera-programming-languages-part-a.md %}/#datatype-bindings-and-case-expressions):

```
(struct const (int) #:transparent)
(struct negate (e) #:transparent)
(struct add (e1 e2) #:transparent)
(struct multiply (e1 e2) #:transparent)

(define (eval-exp e)
  (cond [(const? e) e] ; note returning an exp, not a number
        [(negate? e) (const (- (const-int (eval-exp (negate-e e)))))]
        [(add? e) (let ([v1 (const-int (eval-exp (add-e1 e)))]
                        [v2 (const-int (eval-exp (add-e2 e)))])
                    (const (+ v1 v2)))]
        [(multiply? e) (let ([v1 (const-int (eval-exp (multiply-e1 e)))]
                             [v2 (const-int (eval-exp (multiply-e2 e)))])
                         (const (* v1 v2)))]
        [#t (error "eval-exp expected an exp")]))
```

Unlike datatype bindings in ML, there is no exhaustive tracking of all possible "subtypes" of an "expression" in Racket. Rather, we have to track ourselves.

Defining structs is *not* syntactic sugar for anything else, as it introduces multiple bindings. The key distinction is that a struct definition creates a *new type of value*, which is different from all other types, so that `number?`, `null?`, etc. will return `#f`. Similarly, it defines the *only* way of accessing data compared to approaches using composite data structure like lists, and this approach has built-in error-checking. So in addition to being more concise, our struct-based approach is superior because it catches errors *sooner*.

## Implementing a Programming Language
We can describe a typical workflow for a language implementation as follows.
1. Parsing: the parser take a *string* holding the *concrete syntax* of a program in the language and turn it into the *abstract-syntax tree* (AST) or error. The type checker checks the AST for errors.
2. The AST is then passed to the rest of the implementation. There are basically two approaches to this rest-of-the-implementation for implementing some programming language `B`.
   1. *Interpreter*, a program in another language `A` that takes programs in B and produces answers. Calling such a program in `A` an “evaluator for `B`” or an “executor for `B`” probably makes more sense.
   2. *Compiler*, a program in another language `A` that takes programs in `B` and produces equivalent programs in some other language `C`, and then uses some pre-existing implementation for `C`. For compilation, we call `B` the source language and `C` the target language. A better term than “compiler” would be “translator”

For either the interpreter approach or the compiler approach, we call A, the language in which we are writing the implementation of B, the *metalanguage*.

Modern systems often combine aspects of each and use multiple levels of interpretation and translation.

>Interpreter versus compiler is a feature of a particular programming language implementation, not a feature of the programming language.

Our `eval-exp` function above for arithmetic expressions is a perfect example of an interpreter for a small programming language. We define language constructs in Racket and write ASTs directly in Racket, thus skipping parsing and type-checking.

Interpreter can *assume* the input is a legal AST and fail arbitrarily otherwise, but *cannot assume* the recursive results are the right kind of *value* and should give good error message otherwise.

### Variables and Environments
Since expressions can contain variables, evaluating them requires an environment that maps variables to values. So an interpreter for a language with variables needs a recursive helper function that takes an expression and an environment and produces a value.

The representation of the environment is part of the interpreter’s implementation in the metalanguage, not part of the abstract syntax of the language. Many representations, such as maps, will suffice and fancy data structures that provide fast access for commonly used variables are appropriate. With Racket as our metalanguage, a simple association list holding pairs of strings (variable names) and values (what the variables are bound to) can suffice.

Given an environment, the interpreter uses it differently in different cases:
* To evaluate a variable expression, it looks up the variable’s name (i.e., the string) in the environment.
* To evaluate most subexpressions, such as the subexpressions of an addition operation, the interpreter passes to the recursive calls the *same* environment that was passed for evaluating the outer expression.
* To evaluate things like the body of a `let`-expression, the interpreter passes to the recursive call a slightly different environment, such as the environment it was passed with one more binding (i.e., pair of string and value) in it.

To evaluate an entire program, we just call our recursive helper function that takes an environment with the program and a suitable initial environment, such as the empty environment, which has no bindings in it.

### Closures
To implement a language with function closures and lexical scope, our interpreter needs to “remember” the environment when the function was *defined* so that it can use this environment instead of the environment when the function is *called*.

We can create a small data structure called a closure that includes the environment along with the function itself. It is *this pair (the closure) that is the result of interpreting a function*. In other words, *a function is not a value, a closure is*, so the evaluation of a function produces a closure that “remembers” the environment from when we evaluated the function.

To implement function calls, such as `(call e1 e2)`, we do the following:
* Evaluate `e1` in the current environment and produces a closure;
* Evaluate `e2` in the current environment and produces a value;
* Evaluate the body part of the closure **using the environment part of the closure** *extended* with
  - The name of the argument mapped to the value when it is called;
  - The name of the function mapped to the entire closure.

It may seem expensive that we store the “whole current environment” in every closure. First, the environment can be implemented efficiently. Second, in practice we can save space by storing only those parts of the environment that the function body might possibly use. We can look at the function body and see what *free variables* it has (variables used in the function body whose definitions are outside the function body) and the environment we store in the closure needs only these variables.

In addition, we don't look up free variables at runtime. Language implementations *precompute* the free variables of each function before beginning evaluation. They can store the result with each function so that this set of variables is quickly available when building a closure.

To compile closures into languages without closure, we change all the functions in the program to take an *extra argument (the environment)* and change all function calls to explicitly *pass in this extra argument*. Now when we have a closure, the code part will have an extra argument and the caller will pass in the environment part for this argument. The compiler then just needs to translate all uses of free variables to code that uses the extra argument to find the right value.

## Macros
When implementing an interpreter or compiler, it is essential to keep separate what is in **the language being implemented** and what is in **the language used for doing the implementation (the metalanguage)**. So an language expression should never include usage of the language interpreter or compiler, or any expressions in the metalanguage.

We can define Racket helper functions to create the program in language `B`. Doing so is basically defining macros for language `B` using Racket functions as the macro language. Such functions takes language-implemented syntax and produces language-implemented syntax and thus invisible to the interpreter or compiler. For example,

```
(define (double e)
    (multiply e (const 2)))
```

is a macro and `(negate (double (negate (const 4))))` produces `(negate (multiply (negate (const 4)) (const 2)))`.

We should call out that this approach does not handle issues related to variable shadowing as well as a real macro system that has hygienic macros.

# Section 7
This section discusses static typing.

## ML versus Racket
First, we will compare Racket with ML. The most widespread difference between the two languages is that ML has a static type system that Racket does not. ML's type system rejects lots of programs before running them by doing type-checking and reporting errors.

We can describe ML as roughly defining a *subset* of Racket: Programs that run produce similar answers, but ML rejects many more programs as illegal, i.e., not part of the language. But sometimes, ML rejects Racket-like programs that are not bugs, such as lists with different types of members.

On the other hand, Racket is just ML where *every expression is part of one big datatype*. Specifically, it is like Racket has a such datatype binding and all functions take and returns this datatype:

```sml
datatype theType = Int of int
                 | String of string
                 | Pair of theType * theType
                 | Fun of theType -> theType
                 | ... (* one constructor per built-in type *)

fun car v = case v of Pair(a,b) => a | _  => raise ... (* give some error *)
fun pair? v = case v of Pair _ => true | _ => false
```

Finally, Racket’s struct definitions do one thing you cannot quite do with ML datatype bindings: They dynamically add new constructors to a data type.

## What is Static Checking
What is usually meant by “static checking” is anything done to reject a program *after* it (successfully) parses but *before* it runs. Generally we think static checking is "compile-time checking" though it is irrelevant whether the language implementation will use a compiler or an interpreter after static checking succeeds.

What static checking is performed is part of the definition of a programming language. Given a language with a particular definition, you could also use other tools that do even more static checking, even though such tools are not part of the language definition.

The most common way to define a language’s static checking is via a *type system*. But this is the language’s *approach* to static checking (how it does it), which is different from the *purpose* of static checking (what it accomplishes). The purpose is to reject programs that “make no sense” or “may try to misuse a language feature.”

As ML and Racket demonstrate, the typical points at which to prevent a “bad thing” are “compile-time” and “run-time.” However, it is worth realizing that there is really a continuum of eagerness about when we declare something an error. For example, errors can be shown at
* keystroke-time as certain IDEs perform static analysis;
* compile-time;
* link-time;
* run-time;
* return specific value instead an error so the programmer should handle it.

## Correctness: Soundness, Completeness, Undecidability
A static checker is correct if it prevents what it claims to prevent — otherwise, either the language definition or the implementation of static checking needs to be fixed. But a more precise description of correctness can be defined with the terms **soundness** and **completeness**.

For a given thing *X* we wish to prevent. For example, *X* could be “a program looks up a variable that is not in the environment”:

* A type system is **sound** if it never accepts a program that, when run with some input, does *X*. In other words, it prevents *false negatives*.
* A type system is **complete** if it never rejects a program that, no matter what input it is run with, will not do *X*. In other words, it prevents *false positives*.

The terms soundness and completeness come from logic and are commonly used in the study of programming languages. A sound logic proves only true things. A complete logic proves all true things. Here, our type system is the logic and the thing we are trying to prove is “*X* cannot occur.”

In modern languages, type systems are sound (they prevent what they claim to) but not complete (they reject programs they need not reject). Type systems are not complete because for almost anything you might like to check statically, it is *impossible* to implement a static checker that given any program in your language (a) always terminates, (b) is sound, and (c) is complete. Soundness is important because it lets language users and language implementers rely on *X* never happening, and the type checker must terminate. Thus, we have to give up completeness.

The impossibility result is exactly the idea of **undecidability** at the heart of the study of the theory of computation. Knowing what it means that nontrivial properties of programs are undecidable is fundamental to being an educated computer scientist. The fact that undecidability directly implies the inherent incompleteness of static checking is probably the most important ramification of undecidability.

## Weak Typing
Weak typing is not related to static/dynamic typing, but is about what a program can do when *X* happens.

If a language has programs where a legal implementation is allowed to do *anything*, we call the language **weakly typed**. Languages where the behavior of buggy programs is more limited are called **strongly typed**. One big source of undefined behavior in a weakly-typed languages is array-bounds errors.

C and C++ are the well-known weakly typed languages. They are low-level programming languages and thus do not perform dynamic checks (apart from the static checks at compile time) because 1) dynamic checks require extra data (like tags on values) to do the checks, and 2) performance cost of dynamic checking.

An older perspective is to leave such checks to programmers. But humans are very error-prone so modern languages generally do not adhere to this perspective anymore.

## Static vs. Dynamic Typing
There is no definitive answer to which one is better between static and dynamic checking. A better question would be "what should be enforced statically."
