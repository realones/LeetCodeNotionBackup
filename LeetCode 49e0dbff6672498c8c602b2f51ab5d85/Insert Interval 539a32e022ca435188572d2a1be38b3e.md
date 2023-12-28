# Insert Interval

#: 57
Difficult: Medium
Tags: Array, BS_lower_upper_bound, Geometry, Interval
link: https://leetcode.com/problems/insert-interval/
Priority: High
Status: Dig More, toSummarize
Created time: October 6, 2022 1:28 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150, googleFreq

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` *after the insertion*.

**Example 1:**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]

```

**Example 2:**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

```

**Constraints:**

- `0 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 105`
- `intervals` is sorted by `starti` in **ascending** order.
- `newInterval.length == 2`
- `0 <= start <= end <= 105`

Solution 1:

```cpp
bool cmpEnd(const vector<int> &x, const vector<int> &y) {
    if(x[1]==y[1]){
        return x[0]<y[0];
    }
    return x[1]<y[1];
}

class Solution {
public:
    //KKK AAA BBB CCC DDD
    //     NNNNNNN
    
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int newS=newInterval[0], newE=newInterval[1];
        //find AAA
				//find first block which end >= newStart
        auto it1 = lower_bound(intervals.begin(), intervals.end(), vector<int>{0,newS}, cmpEnd);
        //find CCC
				//find last block which start <= newEnd
				//use upper bound to find the nxt one, so [start,nxt)
        auto it2 = upper_bound(intervals.begin(), intervals.end(), vector<int>{newE,INT_MAX});
        //new interval to add
        int it1start = it1!=intervals.end()?(*it1)[0]:INT_MAX;
        newInterval[0]=min(newInterval[0],it1start);
        int it2prevEnd = it2!=intervals.begin()?(*prev(it2))[1]:INT_MIN;
        newInterval[1]=max(newInterval[1],it2prevEnd);
        //delete AAA till CCC
        intervals.erase(it1,it2);
        //insert NNN
        intervals.insert(it1,newInterval);
        return intervals;
    }
};
```

Solution 2:

```cpp
class Solution {
public:
    //KKK AAA BBB CCC DDD
    //     NNNNNNN
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        //find A
        auto iter1=lower_bound(intervals.begin(),intervals.end(),newInterval);
        int leftBound=newInterval[0];
        if(iter1!=intervals.begin() && (*prev(iter1))[1]>=newInterval[0]){
            iter1=prev(iter1);
            leftBound=(*iter1)[0];
        }
        //find C
        auto iter2=upper_bound(intervals.begin(),intervals.end(),vector<int>({newInterval[1],INT_MAX}));//should be INT_MAX but not 0
        int rightBound=newInterval[1];
        if(iter2!=intervals.begin() && (*prev(iter2))[1]>newInterval[1]){
            rightBound=(*prev(iter2))[1];
        }
        //rm from A till D
        intervals.erase(iter1,iter2);
        //add N
        intervals.insert(iter1,{leftBound,rightBound});
        return intervals;
    }
};
```