---
layout: "post"
title: "Segment Tree"
date: "2018-08-08 23:25"
tags:
  - Algorithm
---

A segment tree is a binary tree where each node represents an interval. Generally a node would store one or more properties of an interval which can be queried later.
![Segment Tree](https://leetcode.com/media/original_images/segtree_intro_1.png)

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
We will use the array `tree[]` to store the nodes of our segment tree (initialized to all zeros). The following scheme (1-based indexing) is used:

1. The node of the tree is at index `1`. Thus `tree[1]` is the root of our tree.
1. The children of `tree[i]` are stored at `tree[2*i]` and `tree[2*i+1]`.
1. We will pad our `arr[]` with extra 0 or null values so that $$n = 2^k$$ (where `n` is the final length of `arr[]` and `k` is a non negative integer.)
1. The leaves of the tree occur at indexes $$2^k$$ to $$2^{k+1} - 1$$. If `n` is the final lenght of `arr[]`, the leaves are from `n` to `2 * n - 1` (inclusive).

Do we actually need to pad `arr[]` with zeros?

No, not really. Just ensure that `tree[]` is large enough and always zero-initialized and you don't need to worry about extra leaf nodes not being processed.

## Build Tree
We will use a very effective bottom-up approach to build segment tree. We already know from the above that if some node `p` holds the sum of $$[i \ldots j]$$ range, its left and right children hold the sum for range $$[i \ldots \frac{i + j}{2}]$$ and $$[\frac{i + j}{2} + 1, j]$$ respectively.

Therefore to find the sum of node `p`, we need to calculate the sum of its right and left child in advance.

We begin from the leaves, initialize them with input array elements $$a[0, 1, \ldots, n-1]$$. Then we move upward to the higher level to calculate the parents' sum till we get to the root of the segment tree.

```python
def buildTree(nums):
    length = len(nums)
    tree = [0] * (2 * length)
    for i, n in enumerate(nums):
        tree[length + i] = n
    for i in range(n - 1, 0, -1):
        tree[i] = tree[2 * i] + tree[2 * i + 1]
    return tree
```

```java
int[] tree;
int n;
public NumArray(int[] nums) {
    if (nums.length > 0) {
        n = nums.length;
        tree = new int[n * 2];
        buildTree(nums);
    }
}

private void buildTree(int[] nums) {
    for (int i = n, j = 0;  i < 2 * n; i++,  j++)
        tree[i] = nums[j];
    for (int i = n - 1; i > 0; --i)
        tree[i] = tree[i * 2] + tree[i * 2 + 1];
}
```

Time and space complexity are about $$2n$$.

## Update the Value of an element
This is similar to Build Tree. We update the value of the leaf node of our tree which corresponds to the updated element. Later the changes are propagated through the upper levels of the tree straight to the root.

```python
def updateValue(i, val):
    i += len(nums)
    tree[i] = val
    i //= 2
    while i > 0:
      tree[i] = tree[i * 2] + tree[i * 2 + 1]
      i //= 2
```

```java
void update(int pos, int val) {
    pos += n;
    tree[pos] = val;
    while (pos > 0) {
        int left = pos;
        int right = pos;
        if (pos % 2 == 0) {
            right = pos + 1;
        } else {
            left = pos - 1;
        }
        // parent is updated after child is updated
        tree[pos / 2] = tree[left] + tree[right];
        pos /= 2;
    }
}
```

Time complexity is $$O(logn)$$ and space complexity is $$O(1)$$.

## Query
We can find range sum query $$[L, R]$$ using segment tree in the following way:

Algorithm hold loop invariant:

$$l \le r$$ and sum of $$[L \ldots l]$$ and $$[r \ldots R]$$ has been calculated, where $$l$$ and $$r$$ are the left and right boundary of calculated sum. Initially we set $$l$$ to the leaf at $$L$$ and $$r$$ to leaf at $$R$$. Range $$[l, r]$$ shrinks on each iteration till range borders meets after approximately $$\log n$$ iterations of the algorithm.

* Loop till $$l \le r$$
 * Check if l is right child of its parent P
    * l is right child of P. Then P contains sum of range of l and another child smaller than l (which is outside the range $$[l, r]$$) and we don't need parent P sum. Add l to `sum` and set l to point to the right of P on the upper level.
    * l is the left child of P. Then parent P contains sum of range which lies in $$[l, r]$$. Move l to P.
 * Check if r is left child of its parent P
    * r is left child of P. Then P contains sum of range of r and another child larger than r (which is outside the range $$[l, r]$$) and we don't need parent P sum. Add r to `sum` and set r to the left of P on the upper level.
    * r is the right child of P. Then parent P contains sum of range which lies in $$[l, r]$$. Move r to P.

```python
def sumRnage(l, r):
    sum = 0
    l += n
    r += n

    while l <= r:
        if l % 2 == 1:
            sum += tree[l]
            l += 1
        if r % 2 == 0:
            sum += tree[r]
            r -= 1
        l //= 2
        r //= 2

    return sum
```

```java
public int sumRange(int l, int r) {
    // get leaf with value 'l'
    l += n;
    // get leaf with value 'r'
    r += n;
    int sum = 0;
    while (l <= r) {
        if ((l % 2) == 1) {
           sum += tree[l];
           l++;
        }
        if ((r % 2) == 0) {
           sum += tree[r];
           r--;
        }
        l /= 2;
        r /= 2;
    }
    return sum;
}
```

Time complexity is $$O(logn)$$ because on each iteration of the algorithm we move one level up. The space complexity is $$O(1)$$.
