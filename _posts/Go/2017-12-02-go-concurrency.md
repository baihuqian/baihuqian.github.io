---
layout: "post"
title: "GoLang: Concurrency"
date: "2018-02-17 20:05"
tags: Go
---

Go is often described as a concurrent-friendly language. The reason for this is that it provides a simple syntax over two powerful mechanisms: goroutines and channels.

# Goroutines
A goroutine is a lightweight thread managed by the Go runtime, not the OS. To start a goroutine, simply use the `go` keyword followed by the function we want to execute.

```go
go f(x, y, z)
```

The evaluation of f, x, y, and z happens in the current goroutine and the execution of f happens in the new goroutine. If we just want to run a bit of code, we can use an anonymous function.

```go
go func() {
  fmt.Println("processing")
}()
```

Goroutines run in the same address space, so access to shared memory must be synchronized.

Goroutines are easy to create and have little overhead. Multiple goroutines will end up running on the same underlying OS thread. This is often called an M:N threading model because we have M application threads (goroutines) running on N OS threads. The result is that a goroutine has a fraction of overhead (a few KB) than OS threads. On modern hardware, it's possible to have millions of goroutines.

## Channels
Channels help make concurrent programming saner by taking shared data out of the picture. A channel is a communication pipe between goroutines which is used to pass data. In other words, a goroutine that has data can pass it to another goroutine via a channel. The result is that, at any point in time, only one goroutine has access to the data.

Channels are a typed conduit through which you can send and receive values. To create a channel:
```go
c := make(chan int)
```

The type of this channel is `chan int`. Therefore, to pass this channel to a function, our signature looks like:

```go
func worker(c chan int) { ... }
```

Channels support two operations: receiving and sending. We send to a channel by doing:

```
CHANNEL <- DATA
```

and receive from one by doing

```
VAR := <-CHANNEL
```

The data flows in the direction of the arrow.

By default, sends and receives block until the other side is ready. That is, when we receive from a channel, execution of the goroutine won't continue until data is available. Similarly, when we send to a channel, execution won't continue until the data is received. This allows goroutines to synchronize without explicit locks or condition variables.

If there are multiple receivers to the same channel, Go guarantees that the data will only be received by a single receiver.

A sender can close a channel to indicate that no more values will be sent. Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression:

```go
v, ok := <-ch
```

`ok` is false if there are no more values to receive and the channel is closed.

The loop for `i := range c` receives values from the channel repeatedly until it is closed.

`close(ch)` closes a channel. Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic. Channels aren't like files; you don't usually need to close them. Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a range loop.

### Buffered Channels

Channels can be buffered. Provide the buffer length as the second argument to `make` to initialize a buffered channel:

```go
ch := make(chan int, 100)
```

The channel's `len` provides a view into the usage of a buffer.

Sends to a buffered channel block only when the buffer is full. Receives block when the buffer is empty. Thus writing to a full channel before anyone can read from it is a deadlock.

### Select

The `select` statement allows:
* receivers to wait on multiple channels.
* senders to send to multiple channels.

A select blocks until one of its cases can run, then it executes that case. If non-blocking default is provided, it will run the default if all cases are blocking. It chooses one at random if multiple are ready. **It runs only once.**

```go
select {
case msg1 := <-c1:
	fmt.Println("received", msg1)
case msg2 := <-c2:
	fmt.Println("received", msg2)
default:
	// Non-blocking operation
}
```

The `default` case in a select is run if no other case is ready. Use a default case to try a send or receive without blocking.

## Timeout
To block for a maximum amount of time, we can use the `time.After` function. Let's look at it then try to peek beyond the magic. To use this, our sender becomes:

```go
for {
  select {
  case c <- rand.Int():
  case <-time.After(time.Millisecond * 100):
    fmt.Println("timed out")
  }
  time.Sleep(time.Millisecond * 50)
}
```

`time.After` returns a channel, so we can `select` from it. The channel is written to after the specified time expires. Notice that we're sending to `c` but receiving from `time.After`. `select` works the same regardless of whether we're receiving from, sending to, or any combination of channels:

* The first available channel is chosen.
* If multiple channels are available, one is randomly picked.
* If no channel is available, the default case is executed.
* If there's no default, select blocks.


## Mutex
The only concurrent thing you can safely do to a variable is to read from it. You can have as many readers as you want, but writes need to be synchronized. There are various ways to do this, including using some truly atomic operations that rely on special CPU instructions. However, the most common approach is to use a mutex. Go's standard library provides mutual exclusion with `sync.Mutex` and its two methods: `Lock` and `Unlock`.

We can define a block of code to be executed in mutual exclusion by surrounding it with a call to `Lock` and `Unlock`. We can ensure `Unlock` is called properly using `defer`.

```go
func SafeInc(mux sync.Mutex, counter *int) {
	mux.Lock()
	*counter++
	mux.Unlock()
}

func SafeInc2(mux sync.Mutex, counter *int) {
	mux.Lock()
	defer mux.Unlock()
	*counter++
	}
```

There's another common mutex called a read-write mutex. This exposes two locking functions: one to lock for reading and one to lock for writing. This distinction allows multiple simultaneous readers while ensuring that writing is exclusive. In Go, `sync.RWMutex` is such a lock. In addition to the `Lock` and `Unlock` methods of a `sync.Mutex`, it also exposes `RLock` and `RUnlock` methods; where `R` stands for *Read*.
