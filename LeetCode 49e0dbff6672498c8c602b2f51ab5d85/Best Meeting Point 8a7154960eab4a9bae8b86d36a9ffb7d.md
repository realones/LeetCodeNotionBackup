# Best Meeting Point

#: 296
Difficult: Hard
Tags: Array, Math, Matrix, Sort, median
link: https://leetcode.com/problems/best-meeting-point/description/
Priority: High
Status: No Idea At First
Created time: December 3, 2023 3:39 PM
Last edited time: December 16, 2023 8:11 PM
source: facebookFreq
related: Minimum Cost to Make Array Equalindromic (Minimum%20Cost%20to%20Make%20Array%20Equalindromic%200d794d6bb11644538cb2293d38e6dc64.md), Apply Operations to Maximize Frequency Score (Apply%20Operations%20to%20Maximize%20Frequency%20Score%20f642fd9d465b424da0c3f6d8d3255af7.md)

Given an `m x n` binary grid `grid` where each `1` marks the home of one friend, return *the minimal **total travel distance***.

The **total travel distance** is the sum of the distances between the houses of the friends and the meeting point.

The distance is calculated using [Manhattan Distance](http://en.wikipedia.org/wiki/Taxicab_geometry), where `distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/14/meetingpoint-grid.jpg](https://assets.leetcode.com/uploads/2021/03/14/meetingpoint-grid.jpg)

```
Input: grid = [[1,0,0,0,1],[0,0,0,0,0],[0,0,1,0,0]]
Output: 6
Explanation: Given three friends living at (0,0), (0,4), and (2,2).
The point (0,2) is an ideal meeting point, as the total travel distance of 2 + 2 + 2 = 6 is minimal.
So return 6.

```

**Example 2:**

```
Input: grid = [[1,1]]
Output: 1

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `grid[i][j]` is either `0` or `1`.
- There will be **at least two** friends in the `grid`.

```cpp
class Solution {
public:
    int minTotalDistance(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        //xs, ys
        vector<int> xs, ys;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1){
                    xs.push_back(i);
                    ys.push_back(j);
                }
            }
        }
        return getMinSumDist(xs) + getMinSumDist(ys);
    }

    int getMinSumDist(vector<int>& v){
        int n=v.size();
        sort(v.begin(), v.end());
        int median = v[(n-1)/2];
        int res=0;
        for(auto x: v){
            res+=abs(x-median);
        }
        return res;
    }
};

/*
m,n: 1~200
V: 4e4
at least two cells == 1
================================
find a cell, that sum of (dist cell to each 1) is minimum
================================
from: https://leetcode.com/problems/best-meeting-point/solutions/74186/14ms-java-solution/

res = 
    sum of abs(home.x - x) 
    + 
    sum of abs(home.y - y) 
for x and y, they can be calculated seperately

subquestion: find x:
    gather array with all home.x
    sort
    find median - math
    find dist from median to each node
*/
```