---
layout: "post"
title: "GoLang: Data Structures"
date: "2018-02-17 20:05"
tags: Go
---

# Array
Go arrays are fixed. The type `[n]T` is an array of `n` values of type `T`.

```go
var a [10]int
primes := [6]int{2, 3, 5, 7, 11, 13} // array literal
```
An array's length is part of its type (i.e. The types `[10]int` and `[20]int` are distinct), so arrays cannot be resized. Arrays are values. Assigning one array to another copies all the elements. In particular, if you pass an array to a function, it will receive **a copy of the array**, not a pointer to it. You could pass a pointer to an array as the parameter, but this style isn't idiomatic in Go. Use slices instead.

We can use `len` to get the length of the array. `range` can be used to iterate over it:

```go
for index, value := range scores {

}
```

In Go arrays rarely used directly.

# Slices
A slice is a dynamically-sized, flexible view into the elements of an array. The type `[]T` is a slice with elements of type `T`.

A slice has both a length and a capacity.

* The length of a slice is the number of elements it contains.
* The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.

The length and capacity of a slice `s` can be obtained using the expressions `len(s)` and `cap(s)`.

You can extend a slice's length by re-slicing it, provided it has sufficient capacity.

## Creating a slice literal
```go
scores := []int{1,4,293,4,9}
```
It creates an arrya of length 3, and then builds a slice that references it.

## Using `make`

```go
scores := make([]int, 10)
```

We use `make` instead of `new` because there's more to creating a slice than just allocating the memory (which is what `new` does). Specifically, we have to allocate the memory for the underlying array and also initialize the slice.  In the above, we initialize a slice with a length of 10 and a capacity of 10. The length is the size of the slice, the capacity is the size of the underlying array. Using `make` we can specify the two separately.

```go
scores := make([]int, 0, 10)
```

## Creating from array
`array[start:end]` produces a slice from `start` (inclusive) to `end` (exclusive) of `array`, same as Python. The high or low bounds can be omitted, and low and high bounds default to 0 and length of the array, same as Python.

```go
var s []int = primes[1:4]
```

A slice does not store any data, it just describes a section of an underlying array. Changing the elements of a slice modifies the corresponding elements of its underlying array.

## Nil slices
The zero value of a slice is `nil`. A `nil` slice has a length and capacity of 0 (`len` and `cap` are legal!) and has no underlying array. `var names []string` creates a nil slice.

## Append
```go
func append(s []T, vs ...T) []T
```
where `T` is a placeholder for any given type. The first parameter `s` of `append` is a slice of type `T`, and the rest are `T` values to append to the slice.

The resulting value of `append` is a slice containing all the elements of the original slice plus the provided values. Note it will append a value beyond the length of the original slice, even the original slice contains only default value.

If the backing array of `s` is too small to fit all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array. Therefore, remember to reassign the return value to a variable!

## Copy
Copy copies values from one slice to the other. `func copy(dst, src []Type) int` returns the number of elements copied, which will be the minimum of len(src) and len(dst). Thus, it is not necessary to have `dst` and `src` of the same length!

### Multi-dimension slice
Slices can contain any type, including other slices.

```go
// Create a tic-tac-toe board.
board := [][]string {
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
}
```

Because slices are variable-length, it is possible to have each inner slice be a different length.

Sometimes it's necessary to allocate a 2D slice, a situation that can arise when processing scan lines of pixels, for instance. There are two ways to achieve this. One is to allocate each slice independently; the other is to allocate a single array and point the individual slices into it. Which to use depends on your application. If the slices might grow or shrink, they should be allocated independently to avoid overwriting the next line; if not, it can be more efficient to construct the object with a single allocation. For reference, here are sketches of the two methods. First, a line at a time:

```go
// Allocate the top-level slice.
picture := make([][]uint8, YSize) // One row per unit of y.
// Loop over the rows, allocating the slice for each row.
for i := range picture {
	picture[i] = make([]uint8, XSize)
}
```
And now as one allocation, sliced into lines:

```go
// Allocate the top-level slice, the same as before.
picture := make([][]uint8, YSize) // One row per unit of y.
// Allocate one large slice to hold all the pixels.
pixels := make([]uint8, XSize*YSize) // Has type []uint8 even though picture is [][]uint8.
// Loop over the rows, slicing each row from the front of the remaining pixels slice.
for i := range picture {
	picture[i], pixels = pixels[:XSize], pixels[XSize:]
}
```

## Map
Maps are a convenient and powerful built-in data structure that associate values of one type (the key) with values of another type (the element or value). The key can be of any type for which **the equality operator is defined**, such as integers, floating point and complex numbers, strings, pointers, interfaces (as long as the dynamic type supports equality), structs and arrays. Slices cannot be used as map keys, because equality is not defined on them. Like slices, maps hold references to an underlying data structure. If you pass a map to a function that changes the contents of the map, the changes will be visible in the caller.

Map can be created using `make` function or literals:

```go
var m map[string]int
m := make(map[string]int)
var m = map[string]int {
	"Bell Labs": 1,
	"Google": 2,
}
```

Maps grow dynamically. However, we can supply a second argument to make to set an initial size.

The zero value of a map is `nil`. A `nil` map has no keys, nor can keys be added.

The `make` function returns a map of the given type, initialized and ready for use.

* Insert or update an element in map m: `m[key] = elem`
* Retrieve an element: `elem = m[key]`.
* Delete an element: `delete(m, key)`. It's safe to do this even if the key is already absent from the map.
* Test that a key is present with a two-value assignment: `elem, ok = m[key]`. If `key` is in `m`, `ok` is `true`. If not, `ok` is `false`. If key is not in the map, then `elem` is the zero value for the map's element type.

To iterate over a map, use a `for` loop with the `range` keyword:

```go
for key, value := range lookup {
  ...
}
```
Iteration is not ordered, and each iteration will result in random order.

An attempt to fetch a map value with a key that is not present in the map will return the zero value for the type of the entries in the map. Sometimes you need to distinguish a missing entry from a zero value. You can discriminate with a form of multiple assignment.

```go
var seconds int
var ok bool
seconds, ok = timeZone[tz]
```
For obvious reasons this is called the “comma ok” idiom. In this example, if `tz` is present, `seconds` will be set appropriately and `ok` will be true; if not, `seconds` will be set to zero and `ok` will be false.
