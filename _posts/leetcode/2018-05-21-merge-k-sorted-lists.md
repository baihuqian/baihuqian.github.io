---
layout: "post"
title: "Leetcode 21: Merge k Sorted Lists"
date: "2018-05-21 23:43"
tags:
  - Leetcode
---

# Question

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

Example:
```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

# Solution
Use a min heap to store the head of the lists. $$O(nlogk)$$ time complexity and $$O(k)$$ space complexity.

```scala
import scala.collection.mutable.PriorityQueue
/**
 * Definition for singly-linked list.
 * class ListNode(var _x: Int = 0) {
 *   var next: ListNode = null
 *   var x: Int = _x
 * }
 */
object Solution {
    def mergeKLists(lists: Array[ListNode]): ListNode = {
        if(lists.size == 0)
            return null

        implicit object ListNodeOrdering extends Ordering[ListNode] {
            override def compare(n1: ListNode, n2: ListNode): Int = {
                -(n1.x compare n2.x)
            }
        }

        val queue = new PriorityQueue[ListNode]()
        lists.foreach { l =>
            if(l != null)
                queue.enqueue(l)
        }

        if(queue.headOption.isEmpty)
            return null

        val head = queue.dequeue()
        var prev = head
        if(head.next != null) {
            queue.enqueue(head.next)
        }
        while(queue.headOption.isDefined) {
            val node = queue.dequeue()
            prev.next = node
            prev = node
            if(node.next != null) {
                queue.enqueue(node.next)
            }
        }
        head
    }
}
```
