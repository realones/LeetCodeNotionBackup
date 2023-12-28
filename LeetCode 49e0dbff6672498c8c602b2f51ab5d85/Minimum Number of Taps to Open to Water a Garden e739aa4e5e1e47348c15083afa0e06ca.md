# Minimum Number of Taps to Open to Water a Garden

#: 1326
Difficult: Hard
Tags: Array, Contribution, DP, Greedy, Interval, Sort
link: https://leetcode.com/problems/minimum-number-of-taps-to-open-to-water-a-garden/
Priority: Medium
Status: More Solution
Created time: June 27, 2023 11:15 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily
related: Video Stitching (Video%20Stitching%209eced5e0ba234a4fb1d99daae5a9ffd7.md)

There is a one-dimensional garden on the x-axis. The garden starts at the point `0` and ends at the point `n`. (i.e., the length of the garden is `n`).

There are `n + 1` taps located at points `[0, 1, ..., n]` in the garden.

Given an integer `n` and an integer array `ranges` of length `n + 1` where `ranges[i]` (0-indexed) means the `i-th` tap can water the area `[i - ranges[i], i + ranges[i]]` if it was open.

Return *the minimum number of taps* that should be open to water the whole garden, If the garden cannot be watered return **-1**.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/16/1685_example_1.png](https://assets.leetcode.com/uploads/2020/01/16/1685_example_1.png)

```
Input: n = 5, ranges = [3,4,1,1,0,0]
Output: 1
Explanation: The tap at point 0 can cover the interval [-3,3]
The tap at point 1 can cover the interval [-3,5]
The tap at point 2 can cover the interval [1,3]
The tap at point 3 can cover the interval [2,4]
The tap at point 4 can cover the interval [4,4]
The tap at point 5 can cover the interval [5,5]
Opening Only the second tap will water the whole garden [0,5]

```

**Example 2:**

```
Input: n = 3, ranges = [0,0,0,0]
Output: -1
Explanation: Even if you activate all the four taps you cannot water the whole garden.

```

**Constraints:**

- `1 <= n <= 104`
- `ranges.length == n + 1`
- `0 <= ranges[i] <= 100`

todo: dp tag in leetcode?

Solution:

hmm, i am happy to get it resolved pretty fast in one try.

```cpp
using ii=pair<int,int>;

bool cmp(const ii &x, const ii &y) {
    if(x.first != y.first) return x.first<y.first;
    else return x.second>y.second;

}

class Solution {
public:
    int minTaps(int n, vector<int>& ranges) {
        //get intervals
        vector<ii> intervals;
        for(int i=0; i<=n; i++){
            intervals.push_back({i-ranges[i],i+ranges[i]});
        }
        sort(intervals.begin(), intervals.end(),cmp);
        if(intervals.front().first>0) return -1;
        //process
        int coveredR=0;
        int newCoveredR=0;
        int cnt=0;
        int i=0;
        while(coveredR<n){//not all covered
            if(i>n) return -1;
            while(i<=n && intervals[i].first<=coveredR){
                newCoveredR= max(newCoveredR, intervals[i].second);
                i++;
            }
            if(newCoveredR<=coveredR) return -1;
            coveredR=newCoveredR;
            cnt++;
        }
        return cnt;
    }
};

/*
sort by l <- then r ->

---------- use
----skip (r not cover new)
  ------- skip (r not cover new)
    ------------ skip
        ----- skip
       ---------------- use (for all with left covered, pick the one reached the most
*/

/*
assume n=10

0 1 2 3 4 5 6 7 8 9 10

get intervals,

sort by begin,

find min number of intervals to cover all: [0,n]

---------- use
  ------- skip
    ------------ use
               ---------- use

sort by l <- then r ->

---------- use
----skip (r not cover new)
  ------- skip (r not cover new)
    ------------ skip
        ----- skip
       ---------------- use (for all with left covered, pick the one reached the most)
*/
```