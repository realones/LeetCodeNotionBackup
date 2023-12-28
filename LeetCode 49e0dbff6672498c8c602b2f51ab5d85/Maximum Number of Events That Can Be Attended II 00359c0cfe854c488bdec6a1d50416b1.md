# Maximum Number of Events That Can Be Attended II

#: 1751
Difficult: Hard
Tags: Array, BS_todo, DP, Sort
link: https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended-ii
Priority: High
Status: No Idea At First
Created time: July 15, 2023 4:11 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given an array of `events` where `events[i] = [startDayi, endDayi, valuei]`. The `ith` event starts at `startDayi` and ends at `endDayi`, and if you attend this event, you will receive a value of `valuei`. You are also given an integer `k` which represents the maximum number of events you can attend.

You can only attend one event at a time. If you choose to attend an event, you must attend the **entire** event. Note that the end day is **inclusive**: that is, you cannot attend two events where one of them starts and the other ends on the same day.

Return *the **maximum sum** of values that you can receive by attending events.*

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60048-pm.png](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60048-pm.png)

```
Input: events = [[1,2,4],[3,4,3],[2,3,1]], k = 2
Output: 7
Explanation:Choose the green events, 0 and 1 (0-indexed) for a total value of 4 + 3 = 7.
```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60150-pm.png](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60150-pm.png)

```
Input: events = [[1,2,4],[3,4,3],[2,3,10]], k = 2
Output: 10
Explanation: Choose event 2 for a total value of 10.
Notice that you cannot attend any other event as they overlap, and that you donot have to attend k events.
```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60703-pm.png](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60703-pm.png)

```
Input: events = [[1,1,1],[2,2,2],[3,3,3],[4,4,4]], k = 3
Output: 9
Explanation: Although the events do not overlap, you can only attend 3 events. Pick the highest valued three.
```

**Constraints:**

- `1 <= k <= events.length`
- `1 <= k * events.length <= 106`
- `1 <= startDayi <= endDayi <= 109`
- `1 <= valuei <= 106`

**todo**: 

1. I used the first solution in Editorial, more solution from Editorial:
2. also, it looks like backpack dp, should be solved in that way, maybe like “k sum” question, or investigate the connection between this question and backpack dp

My thought already very close, see solution comments for details, but failed on analyzed time complexity, which is O(nk * logn), in other word, failed to used DP to improve from dfs

**Solution**:

```cpp
class Solution {
public:
    /*
    events: start, end, val
    attend <=k events
    */
    int maxValue(vector<vector<int>>& events, int k) {
        int n=events.size();
        sort(events.begin(), events.end());
        vector<vector<int>> dp(n,vector<int>(k+1,0));
        return dfs(events,dp,0,k);
    }

    int dfs(vector<vector<int>>& events, vector<vector<int>>& dp, int idx, int k){
        int n=events.size();
        if(k==0 || idx==n) return 0;
        int start=events[idx][0], end=events[idx][1], val=events[idx][2];
        if(dp[idx][k]!=0) return dp[idx][k];
        //not take
        int notTake=dfs(events, dp, idx+1,k);
        //take  --  nxtIdx= bs events to find fisrt start that > end
        int nxtIdx=upper_bound(events.begin(), events.end(), vector<int>{end,INT_MAX,INT_MAX}) - events.begin();
        int take=val+dfs(events, dp, nxtIdx,k-1);
        dp[idx][k] = max(notTake, take);
        return dp[idx][k];
    }

};

/*
1 <= k <= events.length
1 <= k * events.length <= 1e6
1 <= startDayi <= endDayi <= 1e9
1 <= valuei <= 1e6
================================
my thoughts:
from n items, pick k, each one has diff time slot, get max val;
similar to backpack, find all combination - for each item - take / not_take, use curSize to cut branches and memorization dp

for each one, take or not take, then bs to find nxt avaliable time slot
k times

================================
from Editorial:
approach1: Top-down DP + BS

Intuition:
dfs(idx): maximum val in range events[cur~n-1]
    = max of following action options:
        1. skip cur event
            = dfs(idx+1)
        2. attend cur event
            gain cur val, find nxt_idx we can attend using BS:
            = curVal+dfs(nxt_idx)
===>
dfs(idx,k) = max(dfs(idx+1,k), curVal+dfs(nxt_idx,k-1))
with memo[idx,k]

Algorithm:
sort events by start time
build dp[n][k+1]

dfs(idx, cnt):
    try to read from dp[idx][cnt]
    if(cnt==0 || idx==n) return 0;
    dfs(idx,cnt) = max(dfs(idx+1,cnt), curVal+dfs(nxt_idx,cnt-1))

return dp[0][k]

Complexity:
T: O(nklogn)
S: nk

================================
approach2:

================================
approach3:

================================
approach4:

================================
approach5:

*/
```