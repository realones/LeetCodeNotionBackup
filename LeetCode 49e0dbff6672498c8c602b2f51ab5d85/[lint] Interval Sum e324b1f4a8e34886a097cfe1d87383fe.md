# [lint] Interval Sum

#: 206
Difficult: Medium
Tags: Prefix, SegmentTree_idx
link: https://www.lintcode.com/problem/206/
Priority: Pass
Created time: August 29, 2022 5:32 PM
Last edited time: October 27, 2023 4:54 PM
practicedTimes: 1
source: jiuzhang, 算高SegmentTree

Description

Given an integer array (index from 0 to n-1, where n is the size of this array), and an query list. Each query has two integers `[start, end]`. For each query, calculate the sum of numbers between index start and end in the given array and return in the result list.

*Contact me on wechat to get **Amazon、Google** requent Interview questions . (wechat id : **jiuzhang0607**)*

# 

We suggest you finish problem [Segment Tree Build](https://www.lintcode.com/problem/segment-tree-build/), [Segment Tree Query](https://www.lintcode.com/problem/segment-tree-query/) and [Segment Tree Modify](https://www.lintcode.com/problem/segment-tree-modify/) first.

Example

**Example 1:**

```
Input: array = [1,2,7,8,5],  queries = [(0,4),(1,2),(2,4)]
Output: [23,9,20]

```

**Example 2:**

```
Input: array = [4,3,1,2],  queries = [(1,2),(0,2)]
Output: [4,8]

```

Challenge

O(logN) time for each query

```cpp
/**
 * Definition of Interval:
 * class Interval {
 * public:
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param a: An integer list
     * @param queries: An query list
     * @return: The result list
     */
    vector<long long> intervalSum(vector<int> &nums, vector<Interval> &queries) {
        // write your code here
        int n=nums.size(); if(n==0) {return {};}
        vector<long long> prefixSum(nums.begin(), nums.end());
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        vector<long long> res;
        for(auto q: queries){
            int l=q.start, r=q.end;
            res.push_back(getPrefixSum(prefixSum,l,r));
        }
        return res;
    }

    int getPrefixSum(vector<long long>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```