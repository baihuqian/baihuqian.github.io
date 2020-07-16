---
layout: "post"
title: "Coursera Programming Languages, Part B"
date: "2020-07-03 15:07"
---

This is the course notes I took when studying [Programming Languages (Part B)](https://www.coursera.org/learn/programming-languages-part-b), offered by Coursera.

This course is based on [Part A]({{ site.baseurl }}{% link _posts/Learning/2020-04-07-coursera-programming-languages-part-a.md %}). In Part B, we re-apply functional programming concepts to a dynamically-typed language, and We discuss new concepts like delay evaluation, macros, implementing programming languages, and static vs. dynamic typing.

# Section 5
We will use the Racket programming language (instead of ML) and the DrRacket programming environment.

Racket is derived from Scheme, a well-known programming language that has evolved since 1975. (Scheme in turn is derived from LISP, which has evolved since 1958 or so.)

## Introduction to Racket
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
Racket provides four ways of defining local bindings, compared to the single `let` expression in ML. The variety is in the scope and environment that the bindings are added.

First, there is the expression of the form
```
(let ([x1 e1]
      [x2 e2]
      ...
      [xn en])
  e)
```
This creates local variables `x1`, `x2`, ... `xn`, bound to the results of evaluating `e1`, `e2`, ..., `en`, and then the body `e` can use these variables (i.e., they are in the environment) and the result of `e` is the overall result. Syntactically, notice the “extra” parentheses around the collection of bindings and the common style of where we use square parentheses.

The key question is what environment do we use to evaluate `e1`, `e2`, ..., `en`? It uses the environment from “**before**” the `let`-expression. That is, later variables in the same `let`-expression do not have earlier ones in their environment. If `e3` uses `x1` or `x2`, that would either be an error or would mean some outer variable of the same name. This is **not** how ML `let`-expressions work.

The ML-equivalent one, which evaluates each binding’s expression in the environment *produced from the previous ones*, is `let*`. Neither `let` nor `let`* allows recursion since the `e1`, `e2`, ..., `en` cannot refer to the binding being defined or any later ones. To do so, we have a third variant `letrec`. One typically uses `letrec` to define one or more (mutually) recursive functions. For example:

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

## `cons` Cells
The truth is `cons` just makes a *pair*, which is also called `cons` cell, where you get the first part with `car` and the second part with `cdr`.

A *list* is, by convention and according to the `list?` predefined function, either `null` or a pair where the `cdr` (i.e., second component) is a list. A `cons` cell that is not a list is often called an *improper list*, especially if it has nested `cons` cells in the second position.

Most list functions like `length` will give a run-time error if passed an improper list. On the other hand, the built-in `pair?` primitive returns true for anything built with `cons`, i.e., any improper or proper list except the empty list.

Pairs are a generally useful way to build an *each-of* type, i.e., something with multiple pieces. And in a dynamically typed language, all you need for lists are pairs and some way to recognize the end of the list, which by convention Racket uses the `null` constant (which prints as `’()`) for. As a matter of style, you should use proper lists and not improper lists for collections that could have any number of elements.

## `Cons` cells are immutable, but there is `mcons`
Cons cells are immutable: When you create a cons cell, its two fields are initialized and will never change. The Racket implementation can be clever enough to make `list?` a constant-time operation since it can store with every cons cell whether or not it is a proper list when the cons cell is created.

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
