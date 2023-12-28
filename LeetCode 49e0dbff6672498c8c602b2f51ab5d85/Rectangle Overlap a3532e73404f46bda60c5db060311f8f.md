# Rectangle Overlap

#: 836
Difficult: Easy
Tags: Geometry, Math
link: https://leetcode.com/problems/rectangle-overlap/
Priority: Low
Created time: October 10, 2022 9:52 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

An axis-aligned rectangle is represented as a list `[x1, y1, x2, y2]`, where `(x1, y1)` is the coordinate of its bottom-left corner, and `(x2, y2)` is the coordinate of its top-right corner. Its top and bottom edges are parallel to the X-axis, and its left and right edges are parallel to the Y-axis.

Two rectangles overlap if the area of their intersection is **positive**. To be clear, two rectangles that only touch at the corner or edges do not overlap.

Given two axis-aligned rectangles `rec1` and `rec2`, return `true` *if they overlap, otherwise return* `false`.

**Example 1:**

```
Input: rec1 = [0,0,2,2], rec2 = [1,1,3,3]
Output: true

```

**Example 2:**

```
Input: rec1 = [0,0,1,1], rec2 = [1,0,2,1]
Output: false

```

**Example 3:**

```
Input: rec1 = [0,0,1,1], rec2 = [2,2,3,3]
Output: false

```

**Constraints:**

- `rec1.length == 4`
- `rec2.length == 4`
- `109 <= rec1[i], rec2[i] <= 109`
- `rec1` and `rec2` represent a valid rectangle with a non-zero area.

```cpp
class Solution {
public:
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        int r1x1=rec1[0];
        int r1y1=rec1[1];
        int r1x2=rec1[2];
        int r1y2=rec1[3];
        
        int r2x1=rec2[0];
        int r2y1=rec2[1];
        int r2x2=rec2[2];
        int r2y2=rec2[3];
        
        return !(
            r2y2<=r1y1 || //Left
            r2y1>=r1y2 || //Right
            r2x1>=r1x2 || //Up
            r2x2<=r1x1 //Down
        );
    }
};

/*
Solution 1 check position(consider when not intersect then do reverse)
not intersection:
    r2 is on the left/ right/ up / down side of r1

Solution 2 chech area(image intersection shadow on x/y axis)

max(r1x1, r2x1)
should <
min(r1x2, r2x2)
    
AND

max(r1y1, r2y1)
should <
min(r1y2, r2y2)
*/
```