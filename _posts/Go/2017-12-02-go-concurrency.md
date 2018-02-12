---
layout: "post"
title: "GoLang: Concurrency"
date: "2018-02-17 20:05"
tags: Go
---


# Concurrency
## Goroutines
A goroutine is a lightweight thread managed by the Go runtime.

```go
go f(x, y, z)
```

starts a new goroutine running

```go
f(x, y, z)
```

The evaluation of f, x, y, and z happens in the current goroutine and the execution of f happens in the new goroutine.

Goroutines run in the same address space, so access to shared memory must be synchronized.

## Channels

Channels are a typed conduit through which you can send and receive values with the channel operator, <-. The data flows in the direction of the arrow.

```go
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
```   


Like maps and slices, channels must be created before use:

```go
ch := make(chan int)
```

By default, sends and receives block until the other side is ready. This allows goroutines to synchronize without explicit locks or condition variables.

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

Sends to a buffered channel block only when the buffer is full. Receives block when the buffer is empty. Thus writing to a full channel before anyone can read from it is a deadlock.

### Select

The select statement lets a goroutine wait on multiple communication operations.

A select blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple are ready. **It runs only once.**

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

## Mutex
Go's standard library provides mutual exclusion with `sync.Mutex` and its two methods: `Lock` and `Unlock`.

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
