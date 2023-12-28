# K Closest Points to Origin

#: 973
Difficult: Medium
Tags: Geometry, PQ, TopK
link: https://leetcode.com/problems/k-closest-points-to-origin/
Priority: Pass
Created time: August 15, 2022 1:06 PM
Last edited time: October 21, 2023 1:42 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初Hash&Heap

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., `√(x1 - x2)2 + (y1 - y2)2`).

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)

```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].

```

**Example 2:**

```
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.

```

**Constraints:**

- `1 <= k <= points.length <= 104`
- `104 < xi, yi < 104`

```cpp
class MyComp{
public:
    bool operator()(vector<int>& p1, vector<int>& p2){
        int x1=p1[0], y1=p1[1], dist1=pow(x1,2)+pow(y1,2);
        int x2=p2[0], y2=p2[1], dist2=pow(x2,2)+pow(y2,2);
        if(dist1==dist2){
            if(x1==x2) return y1<y2;
            return x1<x2;
        }
        return dist1<dist2;
    }
};

class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        vector<vector<int>> res;
        int n=points.size(); if(n==0) {return res;}
        priority_queue<vector<int>,vector<vector<int>>,MyComp> pq;//far to near
        for(auto point: points){
            pq.push(point);
            if(pq.size()>k){pq.pop();}
        }
        while(!pq.empty()){
            res.push_back(pq.top()); pq.pop();
        }
        return res;
    }
};
```