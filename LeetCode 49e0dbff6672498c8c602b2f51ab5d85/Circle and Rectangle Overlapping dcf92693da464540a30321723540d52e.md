# Circle and Rectangle Overlapping

#: 1401
Difficult: Medium
Tags: Geometry, Math
link: https://leetcode.com/problems/circle-and-rectangle-overlapping/description/
Priority: Medium
Status: No Idea At First
Created time: December 22, 2023 5:13 PM
Last edited time: December 22, 2023 5:13 PM
source: googleFreq

You are given a circle represented as `(radius, xCenter, yCenter)` and an axis-aligned rectangle represented as `(x1, y1, x2, y2)`, where `(x1, y1)` are the coordinates of the bottom-left corner, and `(x2, y2)` are the coordinates of the top-right corner of the rectangle.

Return `true` *if the circle and rectangle are overlapped otherwise return* `false`. In other words, check if there is **any** point `(xi, yi)` that belongs to the circle and the rectangle at the same time.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/02/20/sample_4_1728.png](https://assets.leetcode.com/uploads/2020/02/20/sample_4_1728.png)

```
Input: radius = 1, xCenter = 0, yCenter = 0, x1 = 1, y1 = -1, x2 = 3, y2 = 1
Output: true
Explanation: Circle and rectangle share the point (1,0).

```

**Example 2:**

```
Input: radius = 1, xCenter = 1, yCenter = 1, x1 = 1, y1 = -3, x2 = 2, y2 = -1
Output: false

```

**Example 3:**

![https://assets.leetcode.com/uploads/2020/02/20/sample_2_1728.png](https://assets.leetcode.com/uploads/2020/02/20/sample_2_1728.png)

```
Input: radius = 1, xCenter = 0, yCenter = 0, x1 = -1, y1 = 0, x2 = 0, y2 = 1
Output: true

```

**Constraints:**

- `1 <= radius <= 2000`
- `104 <= xCenter, yCenter <= 104`
- `104 <= x1 < x2 <= 104`
- `104 <= y1 < y2 <= 104`

comment: the ans solution is pretty clever

```cpp
//cp from https://leetcode.com/problems/circle-and-rectangle-overlapping/solutions/563463/c-with-simple-explanation/
// Time: O(1)
// Space: O(1)
class Solution {
public:
    bool checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
        //move center of circle to (0,0)
        x1 -= x_center; x2 -= x_center;
        y1 -= y_center; y2 -= y_center;
        //if existing point (x,y) that (x: x1~x2, y:y1~y2) satisfying x^2 + y^2 <= r^2
        int minX = x1 * x2 > 0 ? min(x1*x1, x2*x2) : 0, minY = y1 * y2 > 0 ? min(y1*y1, y2*y2) : 0;
        return minY + minX <= radius * radius;
    }
};
```