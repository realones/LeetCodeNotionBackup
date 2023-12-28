# Interval List Intersections

#: 986
Difficult: Medium
Tags: 2pt_TwoArray, Array, Interval
link: https://leetcode.com/problems/interval-list-intersections/description/
Priority: Medium
Status: HardToImplement, No Idea At First
Created time: August 17, 2023 1:32 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given two lists of closed intervals, `firstList` and `secondList`, where `firstList[i] = [starti, endi]` and `secondList[j] = [startj, endj]`. Each list of intervals is pairwise **disjoint** and in **sorted order**.

Return *the intersection of these two interval lists*.

A **closed interval** `[a, b]` (with `a <= b`) denotes the set of real numbers `x` with `a <= x <= b`.

The **intersection** of two closed intervals is a set of real numbers that are either empty or represented as a closed interval. For example, the intersection of `[1, 3]` and `[2, 4]` is `[2, 3]`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/01/30/interval1.png](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)

```
Input: firstList = [[0,2],[5,10],[13,23],[24,25]], secondList = [[1,5],[8,12],[15,24],[25,26]]
Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]

```

**Example 2:**

```
Input: firstList = [[1,3],[5,9]], secondList = []
Output: []

```

**Constraints:**

- `0 <= firstList.length, secondList.length <= 1000`
- `firstList.length + secondList.length >= 1`
- `0 <= starti < endi <= 109`
- `endi < starti+1`
- `0 <= startj < endj <= 109`
- `endj < startj+1`

comment:

思路不太直观

具体实现要分情况讨论

```cpp
class Solution {
public:
    vector<vector<int>> intervalIntersection(vector<vector<int>>& firstList, vector<vector<int>>& secondList) {
        if(firstList.empty() || secondList.empty()) return {};
        int m=firstList.size(), n=secondList.size();
        vector<vector<int>> res;
        int i=0,j=0;
        while(i<m && j<n){
            vector<int>& aRange=firstList[i];
            vector<int>& bRange=secondList[j];
            int aStart=aRange[0], aEnd=aRange[1];
            int bStart=bRange[0], bEnd=bRange[1];
            int start=-1,end=-1;
            //start
            if(isCoveredBy(aStart, bRange)) start=aStart;
            else if(isCoveredBy(bStart, aRange)) start=bStart;
            //end
            if(isCoveredBy(aEnd, bRange)) end=aEnd;
            else if(isCoveredBy(bEnd, aRange)) end=bEnd;
            //update i,j
            if(aEnd<bEnd) i++;
            else j++;
            //update res
            if(start!=-1 && end!=-1) res.push_back({start,end});
        }
        return res;
    }

    bool isCoveredBy(int x, vector<int>& range){
        return x>=range[0] && x<=range[1];
    }
};

/*
sweep line
    for each key point, 
2pt for A and B
    i,j
    if i_s small, use i_s, if not covered by B[j], skip it
    if j_s small, ...
    if i_e small, ..., i++
    if j_e small, ..., j++
*/
```