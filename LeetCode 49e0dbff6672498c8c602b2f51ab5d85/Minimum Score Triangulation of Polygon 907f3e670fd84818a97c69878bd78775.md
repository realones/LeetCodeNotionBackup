# Minimum Score Triangulation of Polygon

#: 1039
Difficult: Medium
Tags: Array, Geometry, circle, dp-Pyr
link: https://leetcode.com/problems/minimum-score-triangulation-of-polygon/
Priority: High
Status: No Idea At First
Created time: September 25, 2023 12:55 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You have a convex `n`-sided polygon where each vertex has an integer value. You are given an integer array `values` where `values[i]` is the value of the `ith` vertex (i.e., **clockwise order**).

You will **triangulate** the polygon into `n - 2` triangles. For each triangle, the value of that triangle is the product of the values of its vertices, and the total score of the triangulation is the sum of these values over all `n - 2` triangles in the triangulation.

Return *the smallest possible total score that you can achieve with some triangulation of the polygon*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/25/shape1.jpg](https://assets.leetcode.com/uploads/2021/02/25/shape1.jpg)

```
Input: values = [1,2,3]
Output: 6
Explanation: The polygon is already triangulated, and the score of the only triangle is 6.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/02/25/shape2.jpg](https://assets.leetcode.com/uploads/2021/02/25/shape2.jpg)

```
Input: values = [3,7,4,5]
Output: 144
Explanation: There are two triangulations, with possible scores: 3*7*5 + 4*5*7 = 245, or 3*4*5 + 3*4*7 = 144.
The minimum score is 144.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/02/25/shape3.jpg](https://assets.leetcode.com/uploads/2021/02/25/shape3.jpg)

```
Input: values = [1,3,1,4,1,5]
Output: 13
Explanation: The minimum score triangulation has score 1*1*3 + 1*1*4 + 1*1*5 + 1*1*1 = 13.

```

**Constraints:**

- `n == values.length`
- `3 <= n <= 50`
- `1 <= values[i] <= 100`

comment: not able to handle the circle situation, checked the wisdompeak solution.

Solution - dp-pyr:

```cpp
class Solution {
public:
    int minScoreTriangulation(vector<int>& nums) { //n:3~50, val:1~100
        int n=nums.size();
        vector<vector<int>> dp(n,vector<int>(n,0));//0 as both out of range ones to reduce special handling and for non-calculated ones
        for(int i=0,j; j=i+2,j<n; i++){
            dp[i][j]=nums[i]*nums[i+1]*nums[i+2];
        }
        for(int len=4;len<=n;len++){//len of a brick
            for(int l=0,r;r=l+len-1,r<n;l++){//for each brick
                dp[l][r]=INT_MAX;
                for(int x=l+1; x<=r-1; x++){
                    dp[l][r]=min(dp[l][r],
                                 dp[l][x] + dp[x][r] + nums[l]*nums[x]*nums[r]);
                }
            }
        }
        return dp[0][n-1];
    }
};

/*
score: sum of (triangle a*b*c), for all n-2 triangles
return: smallest score
================================
thought1:
dp[vertexs]: all vertex, the min score
dp[vertexs] min= f(3 vertex) + dp[t1] + dp[t2] + dp[t3], for each 3 vertex
    f(3 vertex): find 3 vertex from vertexs
    divide the rest into possible three groups
        dp[t1]: 
        dp[t2]:
        dp[t3]:
return dp[all vertexs]

=====>
vertexs can be represent as a range of nums
=====>
circlr arr - need to put start,end also
dp[l,r]: range l,r, the min score
dp[l,r] = dp[range1] v1 dp[range2] v2 dp[range3]

return dp[0,n-1]

================================
thought2:
or, cut the out most one, then cut another new out most one, ...
dp[vertexs]: cur set of vertex, the min score

dp[vertexs] min= f(any out most one) + dp[rest of triangle]
return dp[all vertexs]

===>
actually, one time cut only one vertex
dp[nodes] min= f(any node x) + dp[nodes-x]

dp-pyr:
dp[l,r] = dp[(l,r)-x] + f(x)

init: dp[x]=

return: dp[0][n-1]
================================
from wisdompeak:
*idea: add more constraints to dp as long as we can get the result:
*intui: always starting spliting using edge formed by first node and last node
e.g.
       0  1
    5        2
       4  3

    node: 0 1 2 3 4 5
    use edge [0,5], choose one node as another triangle vertex
    then can be split by
        [0,5] + 1: [] [1 2 3 4 5]
        [0,5] + 2: [0 1 2] [2 3 4 5]
        [0,5] + 3: [0 1 2 3] [3 4 5]
        [0,5] + 4: [0 1 2 3 4] []
    
=======>
dp[l][r] min= dp[l,x] + dp[x,r] + [l]*[x]*[r],  for x: [l+1, r-1]

init:
if len(l,r) ==3, return [0]*[1]*[2]
if len(l,r) <=3, return 0

return dp[0][n-1]
*/
```