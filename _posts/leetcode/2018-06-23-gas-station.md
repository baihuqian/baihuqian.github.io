---
layout: "post"
title: "Leetcode 134: Gas Station"
date: "2018-06-23 22:44"
tags:
  - Leetcode
  - Hard
  - Review
---

There are N gas stations along a circular route, where the amount of gas at station i is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.

Note:

* If there exists a solution, it is guaranteed to be unique.
* Both input arrays are non-empty and have the same length.
* Each element in the input arrays is a non-negative integer.


Example 1:

```
Input:
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

Output: 3

Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

Example 2:
```
Input:
gas  = [2,3,4]
cost = [3,4,3]

Output: -1

Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

# Solution
**Observation 1**: If car starts at A and can not reach B, then any stations between A and B can not reach B. (B is the first station that A can not reach.)

**Proof by contradiction**: suppose there exists a station C between A and B, that the car can reach B from C. This means the car can reach B with `tank = 0` from C. However, because the car can reach C from A, meaning `tank >= 0` at station C from A. Therefore, the car can reach B from A.

**Observation 2**: If the total number of gas is bigger than the total number of cost. There must be a solution.

**Proof**: If the gas is more than the cost in total, there must be some stations we have enough gas to go through them. Let's say they are green stations. So the other stations are red. The adjacent stations with same color can be joined together as one. Then there must be a red station that can be joined into a precedent green station unless there isn't any red station, because the total gas is more than the total cost. In other words, all of the stations will join into a green station at last.

```python
class Solution(object):
    def canCompleteCircuit(self, gas, cost):
        """
        :type gas: List[int]
        :type cost: List[int]
        :rtype: int
        """

        start, tank, total = 0, 0, 0
        for idx in range(len(cost)):
            tank += gas[idx] - cost[idx]
            if tank < 0:
                start = idx + 1
                total += tank
                tank = 0

        if total + tank < 0:
            return -1
        else:
            return start
```
