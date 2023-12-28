# Meeting Rooms II

#: 253
Difficult: Medium
Tags: Sweep-Line
link: https://leetcode.com/problems/meeting-rooms-ii/
Priority: Low
Created time: May 23, 2023 3:49 PM
Last edited time: November 17, 2023 1:31 AM
practicedTimes: 1
source: leetcode_daily, ticktokFreq

Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return *the minimum number of conference rooms required*.

**Example 1:**

```
Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2

```

**Example 2:**

```
Input: intervals = [[7,10],[2,4]]
Output: 1

```

**Constraints:**

- `1 <= intervals.length <= 104`
- `0 <= starti < endi <= 106`

```cpp
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        int res=0;
        vector<vector<int>> times;
        for(auto itv: intervals){
            times.push_back({itv[0],1});//idx - left or not, if same idx, right first
            times.push_back({itv[1],0});
        }
        sort(times.begin(), times.end());
        int tmp=0;
        for(auto t: times){
            if(t[1]) tmp++;
            else tmp--;
            res=max(res,tmp);
        }
        return res;
    }
};
```