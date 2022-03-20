---
layout: post
title: Kotlin Coroutines
tags:
 - Kotlin
---

Kotlin provides coroutines as a library `kotlinx.coroutines`. The core language provides the concept of *suspending function*, a safer and less error-prone abstraction for asynchronous operations than futures and promises. The rest is provided in the coroutine library.

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

## Structured Concurrency
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

All functions that create coroutines should be defined as extensions on `CoroutineScope`, so that we can rely on structured concurrency to make sure that we don't have lingering global coroutines in our application.

## Coroutine Context
Coroutine context stores additional technical information used to run a given coroutine, like the coroutine custom name, or the dispatcher specifying the threads the coroutine should be scheduled on. Coroutine builder accepts an optional `CoroutineContext` that can be used to explicitly specify the dispatcher for the new coroutine and other context elements. If not specified, the launched coroutine inherits parent's context.

The `CoroutineDispatcher` determines what thread or threads the corresponding coroutine uses for its execution. Unlike Go where the Go runtime takes complete control of scheduling goroutine execution on the OS threads, `CoroutineDispatcher` allows developers to define context switching if needed.

You can combine multiple elements for a coroutine context using the `+` operator, like `Dispatchers.Default + CoroutineName("test")`.

## Channels
A `Channel` is conceptually very similar to `BlockingQueue`. One key difference is that instead of a blocking `put` operation it has a suspending `send`, and instead of a blocking `take` operation it has a suspending `receive`.

Unlike a queue, a channel can be closed to indicate that no more elements are coming. Conceptually, a `clos`e is like sending a special close token to the channel, and there is a guarantee that all previously sent elements before the close are received.

Channel is often used to bridge *producer-consumer* pattern. A channel can have multiple producers (fan-in) or multiple consumers (fan-out). But channels are *fair* with respect to the order of invocations of `send` and `receive`, and they're served FIFO. For example, first `receive` gets the value from first `send`.

The default Channel has no buffer and thus transfers elements when sender and receiver meets (aka rendezvous). Channel can have buffers via the optional `capacity` parameter, and it will suspend `send` when buffer is full.

### Producer
There is a coroutine builder `produce` to write the producer code:

```kotlin
fun CoroutineScope.produceNumbers(): ReceiveChannel<Int> = produce<Int> {
    var x = 1
    while (true) send(x++) // infinite stream of integers starting from 1
}
```

You can also use `produce` to build pipelines that takes a channel and returns another channel:

```kotlin
fun CoroutineScope.square(numbers: ReceiveChannel<Int>): ReceiveChannel<Int> = produce {
    for (x in numbers) send(x * x)
}
```

Note that cancelling a producer coroutine closes its channel, thus eventually terminating all consumers.

### Consumer
It is convenient to use a regular `for msg in channel` loop to receive elements from the channel on the consumer side.

## Kotlin Coroutine vs. Java ExecutionService
Java ExecutionService allows you to run `Runnable` on a thread pool, and Coroutines allows you to run suspending function on a dispatcher, the default of which, `Dispatchers.Default`, is also a thread pool. You would think - what is the difference between the two?

If your coroutines do not truly suspend, then the difference is not much. However, truly suspendable operations such as async IO or `delay` (instead of `Thread.sleep`) allows coroutines to preempt from a thread, while `Runnable` does not support that. A `Runnable` blocks the exeucting thread while `sleep` or waiting for IO to return. Therefore, large number of `Runnable` can easily exhaust the thread pool in the ExecutionService while coroutines are less likely, if implemented correctly.

## Actor
I love the [Actor model](https://en.wikipedia.org/wiki/Actor_model). The nice thing about actors is it does not have shared mutable state and thus no synchronization or locking problems, and interaction between different components are always asynchronous via message passing. I have written quite a few complex software with [Akka Actor](https://doc.akka.io/docs/akka/current/typed/actors.html).

There is an `actor` coroutine builder that conveniently combines actor's mailbox channel into its scope to receive messages from and combines the send channel into the resulting `Job` object, so that the reference to the actor can be carried around as its handle.

Just like Scala's `sealed trait`, Kotlin's sealed class is best for defining the type of messages an actor receives. When a message requires a reply, either supplies a `CompletableDeferred` response or a `SendChannel` sender as part of the message. For example, an actor with a counter can be implemented like this:

```kotlin
// Message types for counterActor
sealed class CounterMsg
object IncCounter : CounterMsg() // one-way message to increment counter
class GetCounter(val response: CompletableDeferred<Int>) : CounterMsg() // a request with reply

// This function launches a new counter actor
fun CoroutineScope.counterActor() = actor<CounterMsg> {
    var counter = 0 // actor state
    for (msg in channel) { // iterate over incoming messages
        when (msg) {
            is IncCounter -> counter++
            is GetCounter -> msg.response.complete(counter)
        }
    }
}

// the main function
fun main() = runBlocking<Unit> {
    val counter = counterActor() // create the actor
    withContext(Dispatchers.Default) {
        massiveRun { // run it many, many times
            counter.send(IncCounter)
        }
    }
    // send a message to get a counter value from an actor
    val response = CompletableDeferred<Int>()
    counter.send(GetCounter(response))
    println("Counter = ${response.await()}")
    counter.close() // shutdown the actor
}
```
