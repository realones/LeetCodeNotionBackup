# Water and Jug Problem

#: 365
Difficult: Medium
Tags: BFS, DFS, Math
link: https://leetcode.com/problems/water-and-jug-problem/description/
Priority: High
Status: More Solution, No Idea At First
Created time: December 20, 2023 7:01 PM
Last edited time: December 20, 2023 7:07 PM
source: googleFreq

You are given two jugs with capacities `jug1Capacity` and `jug2Capacity` liters. There is an infinite amount of water supply available. Determine whether it is possible to measure exactly `targetCapacity` liters using these two jugs.

If `targetCapacity` liters of water are measurable, you must have `targetCapacity` liters of water contained **within one or both buckets** by the end.

Operations allowed:

- Fill any of the jugs with water.
- Empty any of the jugs.
- Pour water from one jug into another till the other jug is completely full, or the first jug itself is empty.

**Example 1:**

```
Input: jug1Capacity = 3, jug2Capacity = 5, targetCapacity = 4
Output: true
Explanation: The famousDie Hard example

```

**Example 2:**

```
Input: jug1Capacity = 2, jug2Capacity = 6, targetCapacity = 5
Output: false

```

**Example 3:**

```
Input: jug1Capacity = 1, jug2Capacity = 2, targetCapacity = 3
Output: true

```

**Constraints:**

- `1 <= jug1Capacity, jug2Capacity, targetCapacity <= 106`

comment: todo GCD math solution

Solution BFS:

bfs different type of operations, record the status of the two jugs

```cpp
# cp from https://leetcode.com/problems/water-and-jug-problem/solutions/83709/breadth-first-search-with-explanation/

class Solution(object):
    def canMeasureWater(self, x, y, z):
        """
        :type x: int
        :type y: int
        :type z: int
        :rtype: bool
        """
        # if x > y:
        #     temp = x;
        #     x = y;
        #     y = temp;
            
        if z > x + y:
            return False;
        
        # set the initial state will empty jars;
        queue = [(0, 0)];
        visited = set((0, 0));
        while len(queue) > 0:
            a, b = queue.pop(0);
            if a + b == z:
                return True;
            
            states = set()
            
            states.add((x, b)) # fill jar x;
            states.add((a, y)) # fill jar y;
            states.add((0, b)) # empty jar x;
            states.add((a, 0)) # empty jar y;
            states.add((min(x, b + a), 0 if b < x - a else b - (x - a))) # pour jar y to x;
            states.add((0 if a + b < y else a - (y - b), min(b + a, y))) # pour jar x to y;

            for state in states:
                if state in visited:
                    continue;
                queue.append(state)
                visited.add(state);
                
        return False;
```