# Employee Free Time

#: 759
Difficult: Hard
Tags: Array, Interval, PQ, Sort
link: https://leetcode.com/problems/employee-free-time/description/
Priority: Medium
Status: More Solution, dup
Created time: June 27, 2023 11:11 PM
Last edited time: December 20, 2023 11:05 AM
source: HuaHua, googleFreq
related: Merge Intervals (Merge%20Intervals%2004fe6adb17504669bb5150b41edbe4e8.md)

We are given a list `schedule` of employees, which represents the working time for each employee.

Each employee has a list of non-overlapping `Intervals`, and these intervals are in sorted order.

Return the list of finite intervals representing **common, positive-length free time** for *all* employees, also in sorted order.

(Even though we are representing `Intervals` in the form `[x, y]`, the objects inside are `Intervals`, not lists or arrays. For example, `schedule[0][0].start = 1`, `schedule[0][0].end = 2`, and `schedule[0][0][0]` is not defined).  Also, we wouldn't include intervals like [5, 5] in our answer, as they have zero length.

**Example 1:**

```
Input: schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]
Output: [[3,4]]
Explanation: There are a total of three employees, and all common
free time intervals would be [-inf, 1], [3, 4], [10, inf].
We discard any intervals that contain inf as they aren't finite.

```

**Example 2:**

```
Input: schedule = [[[1,3],[6,7]],[[2,4]],[[2,5],[9,12]]]
Output: [[5,6],[7,9]]

```

**Constraints:**

- `1 <= schedule.length , schedule[i].length <= 50`
- `0 <= schedule[i].start < schedule[i].end <= 10^8`

```cpp
/*
// Definition for an Interval.
class Interval {
public:
    int start;
    int end;

    Interval() {}

    Interval(int _start, int _end) {
        start = _start;
        end = _end;
    }
};
*/

using ii=pair<int,int>;

class Solution {
public:
    vector<Interval> employeeFreeTime(vector<vector<Interval>> schedule) {
        vector<ii> v;
        for(auto& s: schedule){
            for(auto& e: s){
                int l=e.start, r=e.end;
                v.emplace_back(l,r);
            }
        }
        sort(v.begin(), v.end());
        vector<ii> res;
        for(auto [l,r]: v){
            if(res.empty() || res.back().second<l) res.emplace_back(l,r);
            else res.back().second=max(res.back().second, r);
        }

        vector<Interval> ans;
        for(int i=1;i<res.size();i++){
            ans.emplace_back(res[i-1].second, res[i].first);
        }

        return ans;
    }
};

/*
e.g. 1
    1 2 3 4 5 6 7 8 9 10
    [_)     [_)
    [___)
          [___________)
e.g. 2
    1 2 3 4 5 6 7 8 9 10 11 12
    [___)     [_)
      [___)
      [_____)       [_______)
================================
schedule:
    len: 1~50
    elem.len: 1~50
    start,end: 0~1e8
================================
merge intervals
    put together
    sort by start - then end
*/
```