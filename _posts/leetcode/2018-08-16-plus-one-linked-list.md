---
layout: "post"
title: "Leetcode 369: Plus One Linked List"
date: "2018-08-16 23:01"
tags:
  - Leetcode
---

# Question
Given a non-negative integer represented as non-empty a singly linked list of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.

Example:
```
Input:
1->2->3

Output:
1->2->4
```

# Solution
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode plusOne(ListNode head) {
        Stack<ListNode> stack = new Stack<>();
        ListNode node = head;
        while(node != null) {
            stack.push(node);
            node = node.next;
        }
        int carry = 0;
        if(stack.peek().val != 9) {
            stack.peek().val++;
            return head;
        } else {
            stack.pop().val = 0;
            carry = 1;
            while(!stack.isEmpty()) {
                if(stack.peek().val != 9) {
                    stack.peek().val++;
                    return head;
                } else {
                    stack.pop().val = 0;
                }
            }
            node = new ListNode(carry);
            node.next = head;
            return node;
        }
    }
}
```
