# Merge Intervals

#: 56
Difficult: Medium
Tags: Array, Geometry, Interval, Sort
link: https://leetcode.com/problems/merge-intervals/
Priority: Pass
Created time: October 4, 2022 12:22 PM
Last edited time: November 17, 2023 1:30 AM
practicedTimes: 1
source: AmazonFreq, HuaHua, LC_Top_Interview_150, googleFreq, ticktokFreq
related: Employee Free Time (Employee%20Free%20Time%20ba5fc06f044341e7a68667cb6f414528.md)

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.

**Example 1:**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

```

**Example 2:**

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.

```

**Constraints:**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`

```cpp
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class MyComp{
public:
    bool operator()(Interval l,Interval r){
        return l.start<r.start;
    }
};

class Solution {
public:
    vector<Interval> merge(vector<Interval>& intervals) {
        sort(intervals.begin(),intervals.end(),MyComp());
        
        vector<Interval> res;
        for(auto itv:intervals){
            if(res.empty() || (*res.rbegin()).end < itv.start){
                res.push_back(itv);
            }
            else{//merge
                (*res.rbegin()).end=max((*res.rbegin()).end,itv.end);
            }
        }
            
        return res;
    }
};
```