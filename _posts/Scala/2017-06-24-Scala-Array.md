---
title: Learning Scala - Scala Arrays
tags: Scala
---

In Scala, array indices are zero-based. To index in an array, use `()` instead of `[]` in Java:

```scala
val arr = Array(1, 2, 3)
val one = arr(0)
```

## Fixed-length Arrays
`Array`: It is implemented as a Java array, so the memory is initialized to zero or `null`.

```scala
val nums = Array[Int](10)
```

### Variable-length Arrays
`ArrayBuffer`: Scala's equivalency of ArrayList: 
```scala
val b = ArrayBuffer[Int]()
```

`b += 1`, `b += (1, 2, 3, 4)` are used to append elements at the end. If multiple elements are added, enclose them in parentheses.

`b ++= Array(1, 2, 3)` are used to append collection to `b`.

`elem +=: b` prepend `b` with `elem`. `coll ++=: b` prepend `coll` to `b`.

`buff -- buff2` remove all elements in `buff2` from `buff`.

`b.toArray()` and `a.toBuffer()` can convert between `Array` and `ArrayBuffer`.

## Array Traversal
Scala offers more uniform way to traverse `Array` and `ArrayBuffer`. In a for loop:

```scala
for (i <- 0 until a.length)
	println(a(i))
```

`until` constructs a Range up to the upper bound (exclusive). You can also use `to`, which is inclusive.

To visit every other element, use `0 until (a.length, 2)`. To visit elements in reverse order, use `(0 until a.length).reverse`.

If you don't need to access the index, visit the elements directly:

```scala
for (elem <- a)
	println(elem)
```

## Array Transformation
Apply a certain transformation to all elements or some elements in array: use `for ... yield`:

```scala
	val result = for (elem <- a if elem % 3 == 0) yield 2 * elem
```

It creates a new collection of the same type with results, and the original collection is not affected. Use *guard* if necessary. It is the same as

```scala
	a.filter(_ % 3 == 0).map(_ * 2)
	a filter {_ % 3 == 0} map {_ * 2}
```

It's just a matter of style.

## Common methods
* `sum`, `min`, `max`
* `sorted`: sort results into a new collection without modifying the original one. `sortWith()` works with a comparison function.
* `quickSort(a)` sort an array in place.
* `a.mkString()` joins elements in array and ArrayBuffer with provided string. `toString()` on array is useless.

## Multidimensional Arrays
`val matrix = Array.ofDim[Double](3, 5)` creates a 3-row, 5-column matrix. `ofDim` can create up to 5 dimensional arrays.

To access an element, use multiple pairs of parentheses:
`matrix(row)(col) = 42`.
