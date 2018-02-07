---
title: Learning Scala - Scala Collection Basics
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
 * `IndexSeq` indicates that reandom access of elements is efficient through index. By default, `IndexSeq` creates a `Vector`.
 * `LinearSeq` implis that the collection can be efficiently split into head and tail. By default, `LinearSeq` creates a `List`, which is a singly linked list.
* `Set` is an unordered collection of values.
* `Map` is a set of (key, value) pairs.

Scala prefers immutable collections: companion objects in `scala.collection` package produce immutable collections. And `Predef` object (which is always imported) has alias to immutable `List`, `Set` and `Map`.

**Best practice**: to use both immutable and mutable collection, `import scala.collection.mutable` and you can get `Map` and immutable map and `mutable.Map` as mutable.

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