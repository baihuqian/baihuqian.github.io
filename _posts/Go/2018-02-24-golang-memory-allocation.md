---
layout: "post"
title: "GoLang: Memory Allocation"
date: "2018-02-24 16:00"
tags: Go
---

Go has two allocation primitives, the built-in functions `new` and `make`. Composite literal is a special way of allocating new instances every time it is evaluated.

# `new`
Let's talk about `new` first. It's a built-in function that allocates memory. But, it does not **initialize** the memory, it only **zeros** it. That is, `new(T)` allocates zeroed storage for a new item of type `T` and returns its address, a value of type `*T`. In Go terminology, it returns *a pointer to a newly allocated zero value of type `T`*.

Since the memory returned by new is zeroed, it's helpful to arrange when designing your data structures that *the zero value of each type can be used without further initialization*. This means a user of the data structure can create one with new and get right to work. For example, the documentation for `bytes.Buffer` states that "the zero value for `Buffer` is an empty buffer ready to use." Similarly, `sync.Mutex` does not have an explicit constructor or `Init` method. Instead, the zero value for a `sync.Mutex` is defined to be an unlocked mutex.

The zero-value-is-useful property works transitively. Consider this type declaration.

```go
type SyncedBuffer struct {
    lock    sync.Mutex
    buffer  bytes.Buffer
}
```
Values of type SyncedBuffer are also ready to use immediately upon allocation or just declaration:

```go
p := new(SyncedBuffer)  // type *SyncedBuffer
var v SyncedBuffer      // type  SyncedBuffer
```

# `make`
The built-in function `make(T, args)` serves a purpose different from `new(T)`. It creates slices, maps, and channels only, and it returns an initialized (not zeroed) value of type `T` (not `*T`). The reason for the distinction is that these three types represent, under the covers, references to data structures that must be initialized before use. A slice, for example, is a three-item descriptor containing a pointer to the data (inside an array), the length, and the capacity, and until those items are initialized, the slice is `nil`.  In contrast, `new([]int)` returns a pointer to a newly allocated, zeroed slice structure, that is, a pointer to a `nil` slice value. For slices, maps, and channels, `make` initializes the internal data structure and prepares the value for use.

These examples illustrate the difference between new and make.

```go
var p *[]int = new([]int)       // allocates slice structure; *p == nil; rarely useful
var v  []int = make([]int, 100) // the slice v now refers to a new array of 100 ints

// Unnecessarily complex:
var p *[]int = new([]int)
*p = make([]int, 100, 100)

// Idiomatic:
v := make([]int, 100)
```

# Composite Literals
Composite literals construct values for structs, arrays, slices, and maps and **create a new value each time they are evaluated**. They consist of the type of the literal followed by a brace-bound list of elements. Each element may optionally be preceded by a corresponding key.

```go
// Arrays
elements := []int{1, 2, 3, 4}

// Struct definition
type Thing struct {
    name       string
    generation int
    model      string
}

// Positional fields
thing1 := Thing{"Raspberry Pi", 2, "B"}
// Explicit field names as key
thing2 := Thing{name: "Raspberry Pi", generation: 2, model: "B"}
```

Key must be either literal constant or constant expression, and is interpreted as follows:

* struct: as field name. If at least one element has keys, then all of them need to have keys.
* array or slice: as index
* map: as map key

Duplicate keys are forbidden. Keys and values from composite literals must be assignable to respective keys, elements or struct fields.

Omitted fields will be set to zero value of field’s type. Only keyed fields can be used when some fields are omitted.

## Arrays and Slices
Elements of slice or array are indexed so key from literal must be constant integer expression. For elements without key, the index of previous element plus one is used. Key (index) of first element from literal is zero if not specified. It’s valid to specify less element that length of an array.

There is an convenient notation using three dots `...` as the length of array. It’s computed by compiler by adding one to maximum index:

```go
// length is 5
elements := [...]string{2: "foo", 4: "bar"}
```

Slice literal specifies the whole underlaying array.

## Constructors and Composite Literal
Composite literal is commonly used in constructors. For example, the following constructor can be shortened with composite literal:

```go
// Without composite literal
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := new(File)
    f.fd = fd
    f.name = name
    f.dirinfo = nil
    f.nepipe = 0
    return f
}

// With composite literal
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := File{fd, name, nil, 0}
    return &f
}
```

Note that, unlike in C, it's perfectly OK to return the address of a local variable; the storage associated with the variable survives after the function returns. In fact, taking the address of a composite literal allocates a fresh instance each time it is evaluated, so we can combine these last two lines.

```go
return &File{fd, name, nil, 0}
```

By using keyed elements, the initializers can appear in any order, with the missing ones left as their respective zero values. Thus we could say

```go
return &File{fd: fd, name: name}
```

As a limiting case, if a composite literal contains no fields at all, it creates a zero value for the type. The expressions `new(File)` and `&File{}` are equivalent.
