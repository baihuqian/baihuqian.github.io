---
layout: "post"
title: "Segment Tree"
date: "2018-08-08 23:25"
---

A segment tree is a binary tree where each node represents an interval. Generally a node would store one or more properties of an interval which can be queried later.
![](https://leetcode.com/media/original_images/segtree_intro_1.png)

Many problems require that we give results based on query over a range or segment of available data. This can be a tedious and slow process, especially if the number of queries is large and repetitive. A segment tree let's us process such queries efficiently in logarithmic order of time.

Segment Trees have applications in areas of computational geometry and geographic information systems. For example, we may have a large number of points in space at certain distances from a central reference/origin point. Suppose we have to lookup the points which are in a certain range of distances from our origin. An ordinary lookup table would require a linear scan over all the possible points or all possible distances (think hash-maps). Segment Trees lets us achieve this in logarithmic time with much less space cost. Such a problem is called Planar Range Searching. Solving such problems efficiently is critical, especially when dealing with dynamic data which changes fast and unpredictably (for example, a radar system for air traffic.)

# How do we make one?

Let our data be in an array `arr[]` of size `n`.

1. The root of our segment tree typically represents the entire interval of data we are interested in. This would be `arr[0:n-1]`.
1. Each leaf of the tree represents a range comprising of just a single element. Thus the leaves represent `arr[0]`, `arr[1]` and so on till `arr[n-1]`.
1. The internal nodes of the tree would represent the merged or union result of their children nodes.
1. Each of the children nodes could represent approximately half of the range represented by their parent.

A segment tree for an `n` element range can be comfortably represented using an array of size $$\approx 4 \ast n$$. (Stack Overflow has a good discussion as to why. If you are not convinced, fret not. We will discuss it later on.)

Segment trees are very intuitive and easy to use when built recursively.

# Recursive methods for Segment Trees
We will use the array `tree[]` to store the nodes of our segment tree (initialized to all zeros). The following scheme (0 - based indexing) is used:

1. The node of the tree is at index `0`. Thus `tree[0]` is the root of our tree.
1. The children of `tree[i]` are stored at `tree[2*i+1]` and `tree[2*i+2]`.
1. We will pad our `arr[]` with extra 0 or null values so that $$n = 2^k$$ (where `n` is the final length of `arr[]` and `k` is a non negative integer.)
1. The leaves of the tree occur at indexes $$2^k - 1$$ to $$2^{k+1} - 2$$.

Do we actually need to pad `arr[]` with zeros?

No, not really. Just ensure that `tree[]` is large enough and always zero-initialized and you don't need to worry about extra leaf nodes not being processed.

What if we started by storing the node of tree at index 11 instead of index 00? How would the positions of related nodes change?

For starters, the children for node at index ii will now be located at indexes $$(2 \ast i)$$ and $$(2 \ast i + 1)$$.The leaves of the tree occur at indexes $$2^k$$ to $$2^{k+1} - 1$$.

## Build Tree
