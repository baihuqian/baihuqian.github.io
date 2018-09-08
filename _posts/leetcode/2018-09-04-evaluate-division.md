---
layout: "post"
title: "Leetcode 399: Evaluate Division"
date: "2018-09-04 23:24"
tags:
  - Leetcode
---

# Question
Equations are given in the format A / B = k, where A and B are variables represented as strings, and k is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return -1.0.

Example:

Given `a / b = 2.0, b / c = 3.0`.

queries are: `a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? `.

return `[6.0, 0.5, -1.0, 1.0, -1.0 ]`.

The input is: vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries , where equations.size() == values.size(), and the values are positive. This represents the equations. Return vector<double>.

According to the example above:

```
equations = [ ["a", "b"], ["b", "c"] ],
values = [2.0, 3.0],
queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ].
The input is always valid. You may assume that evaluating the queries will result in no division by zero and there is no contradiction.
```

# Graph Solution
```java
class Solution {

    public double[] calcEquation(String[][] equations, double[] values, String[][] queries) {
        Map<String, Map<String, Double>> eqMap = new HashMap<>();
        for(int i = 0; i < equations.length; i++) {
            eqMap.computeIfAbsent(equations[i][0], k -> new HashMap<String, Double>()).put(equations[i][1], values[i]);
            eqMap.computeIfAbsent(equations[i][1], k -> new HashMap<String, Double>()).put(equations[i][0], 1.0 / values[i]);
        }

        double[] res = new double[queries.length];
        for(int i = 0; i < queries.length; i++) {
            if(eqMap.containsKey(queries[i][0]) && eqMap.containsKey(queries[i][1])) {
                if(queries[i][0].equals(queries[i][1]))
                    res[i] = 1.0;
                else
                    res[i] = computeDivision(eqMap, queries[i][0], queries[i][1]);
            } else
                res[i] = -1.0;
        }
        return res;
    }

    private double computeDivision(Map<String, Map<String, Double>> eqMap, String start, String end) {
        Map<String, Double> visited = new HashMap<>();
        Queue<String> queue = new LinkedList<>();
        queue.offer(start);
        visited.put(start, 1.0);

        while(!queue.isEmpty()) {
            String node = queue.poll();
            Map<String, Double> edges = eqMap.get(node);
            double division = visited.get(node);
            for(Map.Entry<String, Double> entry: edges.entrySet()) {
                if(entry.getKey().equals(end))
                    return division * entry.getValue();
                else if (!visited.containsKey(entry.getKey())) {
                    visited.put(entry.getKey(), division * entry.getValue());
                    queue.offer(entry.getKey());
                }
            }
        }
        return -1.0;
    }
}
```
