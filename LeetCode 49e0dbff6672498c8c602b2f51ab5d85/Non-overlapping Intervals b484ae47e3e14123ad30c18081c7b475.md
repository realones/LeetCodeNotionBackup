# Non-overlapping Intervals

#: 435
Difficult: Medium
Tags: Array, DP, Greedy, Interval, Sort
link: https://leetcode.com/problems/non-overlapping-intervals/
Priority: Medium
Status: More Solution
Created time: July 10, 2023 3:42 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return *the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping*.

**Example 1:**

```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.

```

**Example 2:**

```
Input: intervals = [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.

```

**Example 3:**

```
Input: intervals = [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.

```

**Constraints:**

- `1 <= intervals.length <= 105`
- `intervals[i].length == 2`
- `5 * 104 <= starti < endi <= 5 * 104`

todo: i see dp tag, so there should be more solution for dp

Solution: greedy + sort

```cpp
bool cmp(const vector<int> &x, const vector<int> &y) {
    if(x[0]==y[0]) return x[1]<y[1];
    return x[0]<y[0];
}

class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int n=intervals.size(); if(n==0) {return 0;}
        sort(intervals.begin(), intervals.end(), cmp);
        int res=0;
        int preEnd=INT_MIN;
        for(auto& itv: intervals){
            int start=itv[0], end=itv[1];
            if(start<preEnd){
                res++;
                preEnd=min(preEnd,end);
            }
            else{
                preEnd=end;
            }
        }
        return res;
    }
};

/*

sort by left

for each, if start < preEnd
    case 1
    XXXXX
    XXXXXXX
    
    case 2
    XXXXX
     XXXXXX

    case 1 & case 2: not use the new one, preEnd keep the same
    
    case 3
    XXXXX
     XX
    
    case 3: use the new one, preEnd updated to the new one

*
```