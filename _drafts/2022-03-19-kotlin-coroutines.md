---
layout: post
published: false
title: Kotlin Coroutines
---
Unlike other languages like Golang, Kotlin provides coroutines as a library `kotlinx.coroutines`. The core language provides the concept of *suspending function*, a safer and less error-prone abstraction for asynchronous operations than futures and promises. The rest is provided in the coroutine library.

A *coroutine* is an instance of suspendable computation. It consists of suspending function and *coroutine builder*. Coroutine builder launches a coroutine, a few common ones are:
* `runBlocking`: it runs a new coroutine and **block**s the current thread until its completion. It is designed to bridge regular blocking code to libraries that are written in suspending style, to be used in `main` functions and in tests.
* `launch`: it launches a new coroutine without blocking the current thread and returns a reference to the coroutine as a `Job`.
* `async`: it creates a coroutine and returns its future result as an implementation of `Deferred`.

Coroutines are lightweight and can be run on any threads. A coroutine may run on different threads between suspensions.

## Cancellation and Timeouts
`Job` refers to a coroutine. `job.cancel` can be used to cancel a coroutine.

Coroutine cancellation is *cooperative*. A coroutine code has to cooperate to be cancellable. It means the coroutine must check for cancellation and throw `CancellationException` when cancelled. All the suspending functions in `kotlinx.coroutines` are cancellable.

There are two approaches to making computation code cancellable. 
* The first one is to periodically invoke a suspending function that checks for cancellation. There is a `yield` function that is a good choice for that purpose.
* The other one is to explicitly check the cancellation status. `isActive` is an extension property available inside the coroutine via the `CoroutineScope` object.

Cancellable suspending functions throw `CancellationException` on cancellation which can be handled in the usual way with `finally`. This can release any open resources.

The most obvious practical reason to cancel execution of a coroutine is because its execution time has exceeded some timeout. `withTimeout` function launches another coroutine and cancel the tracked `Job` after delay.

## Writing Suspending Functions
Invoking two suspending functions normally means sequential invocation. For example,

```kotlin
runBlocking {
  suspendFun1()
  suspendFun2()
}
```
will run them sequentially.

`async` is used for concurrent invocation. Optionally, `async` can be made lazy by setting its `start` parameter to `CoroutineStart.LAZY`. Lazy `async`only starts the coroutine when its result is required by await, or if its Job's start function is invoked. For example,

```kotlin
runBlocking {
  val one = suspendFun1()
  val two = suspendFun2()
  println("$one.await()")
}
```
means `two` never runs.

## Structured Concurrency and Coroutine Context
Coroutines follow a principle of **structured concurrency** which means that new coroutines can be only launched in a specific `CoroutineScope` which delimits the lifetime of the coroutine. 

Coroutine scope is responsible for the structure and parent-child relationships between different coroutines. Coroutine builder automatically creates the corresponding scope for each coroutine. It is possible to create a new scope without starting a new coroutine, using the `coroutineScope` function. `coroutineScope` is usually usedinside any suspending function to perform multiple concurrent operations. For example,

```kotlin
suspend fun concurrentSum(): Int = coroutineScope {
    val one = async { suspendFun1() }
    val two = async { suspendFun2() }
    one.await() + two.await()
}
```

It's also possible to start a new coroutine from the global scope using `GlobalScope.async` or `GlobalScope.launch`. This will create a top-level "independent" coroutine. Structured concurrency has the following benefits over global scopes:
* The scope is generally responsible for child coroutines, and their lifetime is attached to the lifetime of the scope.
* The scope can automatically cancel child coroutines if something goes wrong or if a user simply changes their mind and decides to revoke the operation.
* The scope automatically waits for completion of all the child coroutines. Therefore, if the scope corresponds to a coroutine, then the parent coroutine does not complete until all the coroutines launched in its scope are complete.

Coroutine context stores additional technical information used to run a given coroutine, like the coroutine custom name, or the dispatcher specifying the threads the coroutine should be scheduled on. Coroutine builder accepts an optional `CoroutineContext` that can be used to explicitly specify the dispatcher for the new coroutine and other context elements. If not specified, the launched coroutine inherits parent's context.

The `CoroutineDispatcher` determines what thread or threads the corresponding coroutine uses for its execution.

You can combine multiple elements for a coroutine context using the `+` operator, like `Dispatchers.Default + CoroutineName("test")`.