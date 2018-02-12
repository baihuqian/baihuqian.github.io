---
layout: "post"
title: "GoLang: Data Structures"
date: "2018-02-17 20:05"
tags: Go
---


# Data Structures
## Array
The type `[n]T` is an array of `n` values of type `T`.

```go
var a [10]int
primes := [6]int{2, 3, 5, 7, 11, 13} // array literal
```
An array's length is part of its type, so arrays cannot be resized.

### Slices
A slice is a dynamically-sized, flexible view into the elements of an array. In practice, slices are much more common than arrays. The type `[]T` is a slice with elements of type `T`. `array[start:end]` produces a slice from `start` (inclusive) to `end` (exclusive) of `array`, same as Python. The high or low bounds can be omitted, and low and high bounds default to 0 and length of the array, same as Python.

```go
var s []int = primes[1:4]
```

A slice does not store any data, it just describes a section of an underlying array. Changing the elements of a slice modifies the corresponding elements of its underlying array.

`[]bool{true, true, false}` creates an array of length 3, and then builds a slice that reference it.

A slice has both a length and a capacity.

* The length of a slice is the number of elements it contains.
* The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.

The length and capacity of a slice s can be obtained using the expressions `len(s)` and `cap(s)`.

You can extend a slice's length by re-slicing it, provided it has sufficient capacity.

#### Nil slices
The zero value of a slice is `nil`. A `nil` slice has a length and capacity of 0 and has no underlying array.

#### Creating a slice with `make`

Slices can be created with the built-in `make` function; this is how you create dynamically-sized arrays. The `make` function allocates a zeroed array and returns a slice that refers to that array:

```go
a := make([]int, 5)  // len(a)=5
```

To specify a capacity, pass a third argument to `make`:

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

#### Multi-dimension slice
Slices can contain any type, including other slices.

```go
// Create a tic-tac-toe board.
board := [][]string{
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
}
```

#### Apprend
```go
func append(s []T, vs ...T) []T
```

The first parameter `s` of `append` is a slice of type `T`, and the rest are `T` values to append to the slice.

The resulting value of append is a slice containing all the elements of the original slice plus the provided values.

If the backing array of `s` is too small to fit all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array.

## Map
```go
var m map[string]int
m := make(map[string]int)
var m = map[string]int {
	"Bell Labs": 1,
	"Google": 2,
}
```

The zero value of a map is `nil`. A `nil` map has no keys, nor can keys be added.

The `make` function returns a map of the given type, initialized and ready for use.

* Insert or update an element in map m: `m[key] = elem`
* Retrieve an element: `elem = m[key]`
* Delete an element: `delete(m, key)`
* Test that a key is present with a two-value assignment: `elem, ok = m[key]`. If `key` is in `m`, `ok` is `true`. If not, `ok` is `false`. If key is not in the map, then elem is the zero value for the map's element type.
