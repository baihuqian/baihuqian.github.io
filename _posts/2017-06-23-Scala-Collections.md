---
title: Scala Collections
tags: Scala
---

Scala’s collection classes begin with the `Traversable` and `Iterable` traits, and extend into the three main categories of sequences (Seq), sets (Set), and maps (Map). Sequences further branch off into indexed and linear sequences.

The `Traversable` trait lets you traverse an entire collection, and its Scaladoc states that it “implements the behavior common to all collections in terms of a `foreach` method,” which lets you traverse the collection *repeatedly*.

The `Iterable` trait yields an `Iterator` that can be used as:
```scala
val coll = ... // some collection
val iter = coll.iter
while(iter.hasNext)
	doSomething(iter.next())
```
When using an iterator, the collection can be traversed *only once*, because each element is consumed during the iteration process.

* `Seq` is an ordered sequence of values:
 * `IndexSeq` indicates taht reandom access of elements is efficient through index. By default, `IndexSeq` creates a `Vector`.
 * `LinearSeq` implis taht the collection cadn be efficiently split into head and tail. By default, `LinearSeq` creates a `List`, which is singly linked list.
* `Set` is an unordered collection of values.
* `Map` is a set of (key, value) pairs.

Scala prefers immutable collections: companion objects in `scala.collection` package produce immutable collections. And `Predef` object (which is always imported) has alias to immutable `List`, `Set` and `Map`.

Best practice: to use both immutable and mutable collection, `import scala.collection.mutable` and you can get `Map` and immutable map and `mutable.Map` as mutable.

Work with immutable collections: you can create new collections out of old ones.

`coll ++ coll2` and `coll ++: coll2` returns a collection of the same type with all elements combined.

For mutable collections:

* `coll += elem`, `coll -= elem`: add or remove `elem` to `coll`.
* `coll += (e1, e2, ...)`, `coll -= (e1, e2, ...)`: add or remove elements to `coll`.
* `coll ++= coll2`, `coll --= coll2`: add or remove `coll2` to `coll`.

## Strict and lazy collections
A *transformer* method is a method that constructs a new collection from an existing collection. This includes methods like `map`, `filter`, `reverse`, etc. — any method that transforms the input collection to a new output collection. Given that definition, collections can also be thought of in terms of being strict or lazy.

In a strict collection, memory for the elements is allocated immediately, and all of its elements are immediately evaluated when a transformer method is invoked.

In a lazy collection, memory for the elements is not allocated immediately, and transformer methods do not construct new elements until they are demanded. All of the collection classes except Stream are strict, but the other collection classes can be converted to a lazy collection by creating a view on the collection.

## Common Methods
* `map`: applies the one-to-one function to all elements of a collection, and return a new collection.
* `take` and `takeRight`: select first or last `n` elements.
* `drop` and `dropRight`: select all elements except first or last `n`.
* `flatMap`: if an element of a collection is a collection, treat its elements as in the outer collection and apply the function on to them directly. For example, `Option` is a collection with 0 or 1 element, so flapMap will only apply to those match to `Some()`.
* `foreach`: similar to `map` but it doesn't return a value.
* `filter`: yields all elements of a collection taht matches a condition.
* `reduceLeft`: takes a *binary* function, and applies to all elements of a sequence from left to right. For example,
```scala
(1 to 100).reduceLeft(_ + _)
```
sums all numbers from 1 to 100. In `_ + _` each underscore represents a separate parameter.
* `sortWith`: takes a binary comparison function for sorting.
* `/:`: Apply a binary operator to a start value and all elements of this traversable or iterator, from left to right. `:\` does the same from right to left. Note the order of parameters for this binary operator.

```scala
def removeAll(collection: TraversableOnce[A], items: Iterable[A]) = (collection /: items) { (coll, item) => coll.remove(item) }

def addAll(collection: TraversableOnce[A], items: Iterable[A]) = (items :\ collection) { (item, coll) => coll.add(item) }
```

## Array
Array indices are zero-based. To index in an array, use `()` instead of `[]` in Java:

```scala
val arr = Array(1, 2, 3)
val one = arr(0)
```

### Fixed-length Arrays
`Array`:
```scala
val nums = Array[Int](10)
```
It is implemented as a Java array, so the memory is initialized to zero or `null`.

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

### Array Traversal
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

### Array Transformation
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

### Common methods
`sum`, `min`, `max`

`sorted`: sort results into a new collection without modifying the original one. `sortWith()` works with a comparison function.

`quickSort(a)` sort an array in place.

`a.mkString()` joins elements in array and ArrayBuffer with provided string. `toString()` on array is useless.

### Multidimensional Arrays
`val matrix = Array.ofDim[Double](3, 5)` creates a 3-row, 5-column matrix. `ofDim` can create up to 5 dimensional arrays.

To access an element, use multiple pairs of parentheses:
`matrix(row)(col) = 42`.

## Sequences
* `coll :+ elem` append
* `elem +: coll` prepend

### Vector
A `Vector` is the **immutable** equivalent of `ArrayBuffer`. It is implemented as trees, and each node has up to 32 children.

### Range
It represents an **immutable** integer sequence. It only stores start, end, and imcrement. It is constructed using `to` and `until` methods.

### Stack, Queue, PriorityQueue
Just as normal data structures.

### Lists
#### Immutable
A `List` in Scala is either `Nil` (empty) or an object with a `head` element and a `tail` (that is a `List`). So a `List` is like a node in a linked list.

The `::` operator makes a new list from given head and tail: 

```scala
9 :: List (4, 2)
```

It is right-associative. It can also destructure the list into head and tail.

It is natural to use recursion to traverse a list. 

#### Mutable
`LinkedList`: you can modify the head (`lst.elem`) and tail (`lst.next`).
`DoubleLinkedList`: has a mutable `prev` reference.

#### Operations
* `elem :: lst`, `elem +: lst` : a new list with `elem` prepended to `lst`.
* `lst2 ::: lst`, `lst2 ++: lst`: a new list with `lst2` prepended to `lst`.

## Sets
A collection of distinct elements. By default sets are implemented as *hash sets* with `hashCode` method. It's faster to find elements this way.

`LinkedHashSet` remembers the order in which elements are inserted with a linked list.

`SortedSet` can be iterated in sorted order. It is implemented as a red-black tree.

`BitSet` is an implementation of a set of nonnegative integers as a sequence of bits. The ith bit is 1 if i is present in the set. It's efficient as long as the maximum number os not too large.

`contains` method checks whether a set contains a given value. `subsetOf` checks whether a set is a subset of another set.

`union`: can be written as `|` and `++`.

`intersect`: can be written as `&`.

`diff`: can be written as `&~` or `--`.

## Map
Immutable map: `val scores = Map("Alice" -> 10, "Bob" -> 3)`.

Mutable map: `val scores = scala.collection.mutable.Map("Alice" -> 10, "Bob" -> 3)`.

Maps are implemented as hash tables.

`SortedMap` is an immutable tree map.

Map is a collection of pairs. A pair is a group of two values. `->` operator makes a pair so the value of `"Alice" -> 10` is `("Alice", 10)`. So map can be defined as `val scores = Map(("Alice", 10), ("Bob", 3))`, too.

### Get value from map
`val bobScore = scores("Bob")`. It will throw exception if key `"bob"` does not exist.

`val bobScore = scores.get("Bob")` returns an `Option` that can be `None` if key `"bob"` does not exist.

`scores.contains("Bob")` checks if `Bob` is in the map key.

`scores.getOrElse("Bob", 0)` returns value of `Bob` if found, 0 otherwise.

### Update Map
`scores("Fred") = 7`. Only works on a mutable map.

Add multiple associations or update: `scores += ("Bob" -> 10, "Fred" -> 7)`.

Remove an association: `scores -= "Alice"`. Only remove the key.

You can create a new Map by modifying and immutable map using `+` (add or update) and `-`. Or make `scores` a `var` so that the new Map can be assigned to `scores`. Then `+=` and `-=` can be used. It is **NOT inefficient** to use immutable map this way.

`map -- map2` remove occurrences in `map2` from `map`.

### Map Iteration
```scala
for ((k, v) <- scores) process k and v
```
	
`scores.keySet` returns a Set of keys.

`scores.keys` and `scores.values` returns an Iterable that can be used in for loop.

## Tuple
Tuple is a set of values, up to 22, enclosed in parentheses. Underlying they are instances of Tuple1 to Tuple22 classes. To access its components, using `_1`, `_2`, etc. Note it starts from 1.

`(a, b)` and `(a -> b)` both produce a `Tuple2`. However, latter form is easier to read.

Tuples are usually for multiple assignments: `val (first, second, third) = t`. It can be useful when a function or method returns multiple values. Unnecessary components are marked out with `_` placeholder.

### zip method
* `zip` bundles elements in two collections into a sequence of tuples. If one collection is longer than the other, the remaining is ignored.
* `zipAll` fills the shorter collection with default value.
* `zipped`, available on 2- and 3-tuple of lists, does 2- or 3-way zip of multiple lists.
* `zipWithIndex` converts a list into a list of `(elem, index)` pairs. Note the index is the second element in the tuple.

## Option
A generic type to hold some optional information. It can be matched to `Some(a)` or `None`. It's better than `null` check. You can think of it as an iterable that holds either zero or one values, so you can use `foreach` to perform an operation if it is not empty.

## Either
`Either[A, B]` is more general than `Option[A]`'s `Some()`/`None` pair. To get the concrete type, match against `Left(A)` and `Right(B)`.