---
layout: "post"
title: "Leetcode 332: Reconstruct Itinerary"
date: "2018-08-11 16:03"
tags:
  - Leetcode
  - DFS
  - Review
---

# Question
Given a list of airline tickets represented by pairs of departure and arrival airports [from, to], reconstruct the itinerary in order. All of the tickets belong to a man who departs from JFK. Thus, the itinerary must begin with JFK.

Note:

1. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].
1. All airports are represented by three capital letters (IATA code).
1. You may assume all tickets form at least one valid itinerary.

Example 1:

```
Input: tickets = [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
Output: ["JFK", "MUC", "LHR", "SFO", "SJC"]
```

Example 2:

```
Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"]. But it is larger in lexical order.
```

# Solution
This is to find a Eulerian path in a directed graph. Thus, start from "JFK", we can apply the Hierholzer's algorithm to find a Eulerian path in the graph which is a valid reconstruction.

Hierholzer's algorithm is as follows:
* Choose any starting vertex v, and follow a trail of edges from that vertex until returning to v. It is not possible to get stuck at any vertex other than v, because the even degree of all vertices ensures that, when the trail enters another vertex w there must be an unused edge leaving w. The tour formed in this way is a closed tour, but may not cover all the vertices and edges of the initial graph.
* As long as there exists a vertex u that belongs to the current tour but that has adjacent edges not part of the tour, start another trail from u, following unused edges until returning to u, and join the tour formed in this way to the previous tour.

By using a data structure such as a doubly linked list to maintain the set of unused edges incident to each vertex, to maintain the list of vertices on the current tour that have unused edges, and to maintain the tour itself, the individual operations of the algorithm (finding unused edges exiting each vertex, finding a new starting vertex for a tour, and connecting two tours that share a vertex) may be performed in constant time each, so the overall algorithm takes linear time, $$O(E)$$.

First, we go DFS from `JFK` until we get stuck. Then, we construct path backwards. When walking backwards through the DFS path, if a node has outgoing path, then perform DFS there, which is guaranteed to return to the starting node, forming a cycle. Then the cycle is merged into the main DFS path.

When we perform DFS, we choose the outgoing edge with the lowest lexical order.

Example:

![Reconstruct Itinerary](http://www.stefan-pochmann.info/misc/reconstruct-itinerary.png)

From JFK we first go DFS: JFK -> A -> C -> D -> A. There we're stuck, so we write down A as the end of the route and retreat back to D. There we see the unused outgoing edge to B and follow it: D -> B -> C -> JFK -> D. Then we're stuck again, retreat and write down the airports while doing so: Write down D before the already written A, then JFK before the D, etc. When we're back from our cycle at D, the written route is D -> B -> C -> JFK -> D -> A. Then we retreat further along the original path, prepending C, A and finally JFK to the route, ending up with the route JFK -> A -> C -> D -> B -> C -> JFK -> D -> A.

Iterative:
```java
class Solution {
    public List<String> findItinerary(String[][] tickets) {
        Map<String, PriorityQueue<String>> dests = new HashMap<>();

        for(String[] t: tickets) {
            dests.computeIfAbsent(t[0], k -> new PriorityQueue<String>()).add(t[1]);
        }
        List<String> res = new LinkedList<>();
        Stack<String> stack = new Stack<>();
        stack.push("JFK");
        while(!stack.empty()) {
            while(dests.containsKey(stack.peek()) && !dests.get(stack.peek()).isEmpty())
                stack.push(dests.get(stack.peek()).poll());
            res.add(0, stack.pop());
        }
        return res;
    }
}
```

Recursive:
```java
public class Solution {

    Map<String, PriorityQueue<String>> flights;
    LinkedList<String> path;

    public List<String> findItinerary(String[][] tickets) {
        flights = new HashMap<>();
        path = new LinkedList<>();
        for (String[] ticket : tickets) {
            flights.putIfAbsent(ticket[0], new PriorityQueue<>());
            flights.get(ticket[0]).add(ticket[1]);
        }
        dfs("JFK");
        return path;
    }

    public void dfs(String departure) {
        PriorityQueue<String> arrivals = flights.get(departure);
        while (arrivals != null && !arrivals.isEmpty())
            dfs(arrivals.poll());
        path.addFirst(departure);
    }
}
```
