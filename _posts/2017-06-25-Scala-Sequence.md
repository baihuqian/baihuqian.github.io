---
title: Learning Scala - Scala Sequences
tags: Scala
---

Scala's `Seq` is an ordered sequence of values. It further breaks into two categories:
* `IndexSeq` indicates that reandom access of elements is efficient through index. By default, `IndexSeq` creates a `Vector`.
* `LinearSeq` implis that the collection can be efficiently split into head and tail. By default, `LinearSeq` creates a `List`, which is a singly linked list.

* `coll :+ elem` append
* `elem +: coll` prepend

## Vector
A `Vector` is the **immutable** equivalent of `ArrayBuffer`. It is implemented as trees, and each node has up to 32 children.

## Range
It represents an **immutable** integer sequence. It only stores start, end, and imcrement. It is constructed using `to` and `until` methods.

## Stack, Queue, PriorityQueue
Just as normal data structures.

## Immutable Lists
A `List` in Scala is either `Nil` (empty) or an object with a `head` element and a `tail` (that is a `List`). So a `List` is like a node in a linked list.

The `::` operator makes a new list from given head and tail: 

```scala
9 :: List (4, 2)
```

It is right-associative. It can also destructure the list into head and tail.

It is natural to use recursion to traverse a list. 

## Mutable Lists
`LinkedList`: you can modify the head (`lst.elem`) and tail (`lst.next`).
`DoubleLinkedList`: has a mutable `prev` reference.

* `elem :: lst`, `elem +: lst` : a new list with `elem` prepended to `lst`.
* `lst2 ::: lst`, `lst2 ++: lst`: a new list with `lst2` prepended to `lst`.
