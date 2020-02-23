---
layout: "post"
title: "Coursera Programming Languages Course"
date: "2020-02-17 19:45"
---

This is the course notes I took when studying Programming Languages, offered by Coursera.

* toc
{:toc}

This course is to learn the fundamental concepts that appear in one form or another in almost every programming language. Different languages are used to see how they can take complementary approaches to representing concepts taught in this course.

# Section 1
## Variable Bindings and Expressions
We will learn ML in a way that teaches core programming-languages concepts. An ML program is a sequence of *bindings*. Each binding gets **type-checked** and then (assuming it type-checks) **evaluated**. There are several kinds of bindings, but for now let’s consider only a *variable binding*, which in ML has this syntax:

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

## Variables are Immutable
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
Function call is a new kind of expression.

The syntax is `e0 (e1,...,en)` with the parentheses optional if there is exactly one argument.

The typing rules require that `e0` has a function type that looks like `t1*...*tn->t` and for 1 ≤ `i` ≤ n, `ei` has type `ti`.

For the evaluation rules, we use the environment at the point of the call to evaluate `e0` to `v0`, `e1` to `v1`, ..., `en` to `vn`. Then `v0` must be a function (it will be assuming the call type-checked) and we evaluate the function’s body in an environment extended such that the function arguments map to `v1`, ..., `vn`.

Exactly which environment is it we extend with the arguments? The environment that “was current” when the function was **defined**, not the one where it is being *called*.

# Tuples and Lists
A tuple is finite ordered sequence of elements. The syntax is `(e1, e2, ..., en)` which evaluates `e1` to `v1`, ..., `en` to `vn`. The type of a tuple is `t1 * t2 * ... tn` where for 1 ≤ `i` ≤ n, `ei` has type `ti`. Tuples can be nested as deep as we want (i.e. `ei` can be tuples), but its type defines how many parts it has.

A list can have flexible numbers of elements of the **same** type. A non-empty list with n values is written [`v1,v2,...,vn]` where `vi` has the same type. You can make a list with `[e1,...,en]` where each expression is evaluated to a value. The type of non-empty list is `t list` where `t` is the type of the element. The empty list, with syntax `[]`, has 0 elements. It is a value of type `'a list` (pronounced "alpha list") where `'a` is a type placeholder.

It is more common to make a list with `e1 :: e2`, pronounced "e1 consed onto e2." Here `e1` evaluates to an "item of type `t`" and `e2` evaluates to a "list of t" and the result is a new list that starts with the result of `e1` and then is all the elements in `e2`.

Functions over lists are usually recursive in order to get to all the elements. Two questions to ask yourself when writing the recursion:
1. What should the answer be for the empty list;
2. What should the answer be for a non-empty list in terms of the answer for the tail of the list?

Similarly, functions that produce lists will recursively create lists out of smaller lists.
