# Minimum Area Rectangle

#: 939
Difficult: Medium
Tags: Array, Geometry, Hash, Math, Sort
link: https://leetcode.com/problems/minimum-area-rectangle/description/
Priority: Medium
Status: More Solution, No Idea At First
Created time: December 20, 2023 6:51 PM
Last edited time: December 20, 2023 6:52 PM
source: googleFreq

You are given an array of points in the **X-Y** plane `points` where `points[i] = [xi, yi]`.

Return *the minimum area of a rectangle formed from these points, with sides parallel to the X and Y axes*. If there is not any such rectangle, return `0`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/08/03/rec1.JPG](https://assets.leetcode.com/uploads/2021/08/03/rec1.JPG)

```
Input: points = [[1,1],[1,3],[3,1],[3,3],[2,2]]
Output: 4

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/08/03/rec2.JPG](https://assets.leetcode.com/uploads/2021/08/03/rec2.JPG)

```
Input: points = [[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]
Output: 2

```

**Constraints:**

- `1 <= points.length <= 500`
- `points[i].length == 2`
- `0 <= xi, yi <= 4 * 104`
- All the given points are **unique**.

comment: to check lee215’s another solution

```java
//cp to pass
class Solution {
    public int minAreaRect(int[][] points) {
        Map<Integer, Set<Integer>> map = new HashMap<>();
        for (int[] p : points) {
            if (!map.containsKey(p[0])) {
                map.put(p[0], new HashSet<>());
            }
            map.get(p[0]).add(p[1]);
        }
        int min = Integer.MAX_VALUE;
        for (int[] p1 : points) {
            for (int[] p2 : points) {
                if (p1[0] == p2[0] || p1[1] == p2[1]) { // if have the same x or y
                    continue;
                }
                if (map.get(p1[0]).contains(p2[1]) && map.get(p2[0]).contains(p1[1])) { // find other two points
                    min = Math.min(min, Math.abs(p1[0] - p2[0]) * Math.abs(p1[1] - p2[1]));
                }
            }
        }
        return min == Integer.MAX_VALUE ? 0 : min;
    }
}

/*
points.len: 1~500
x,y: 0~4e4
================================
 1(x1,y1)       2(x1,y2)

 3(x2,y1)       4(x2,y2)
================================

solution 1: brute force - O(n^4) TLE
    for node1
        for node2
            for node3
                for node4

solutiion 2: hashMap
    intui: 
        we can get rect just from (x1,y1) and (x2,y2),
        then calcu node 2 and 3 if existing in cache
    observation:
        must x1!=x2, and y1!=y2
    imple:
        for node1
            for node2
    T: O(n^2)
*/
```