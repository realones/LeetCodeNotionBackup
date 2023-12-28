# Number of Recent Calls

#: 933
Difficult: Easy
Tags: Data_stream, Design, queue
link: https://leetcode.com/problems/number-of-recent-calls/
Priority: Medium
Status: More Solution
Created time: July 2, 2023 2:06 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

You have a `RecentCounter` class which counts the number of recent requests within a certain time frame.

Implement the `RecentCounter` class:

- `RecentCounter()` Initializes the counter with zero recent requests.
- `int ping(int t)` Adds a new request at time `t`, where `t` represents some time in milliseconds, and returns the number of requests that has happened in the past `3000` milliseconds (including the new request). Specifically, return the number of requests that have happened in the inclusive range `[t - 3000, t]`.

It is **guaranteed** that every call to `ping` uses a strictly larger value of `t` than the previous call.

**Example 1:**

```
Input
["RecentCounter", "ping", "ping", "ping", "ping"]
[[], [1], [100], [3001], [3002]]
Output
[null, 1, 2, 3, 3]

Explanation
RecentCounter recentCounter = new RecentCounter();
recentCounter.ping(1);     // requests = [1], range is [-2999,1], return 1
recentCounter.ping(100);   // requests = [1,100], range is [-2900,100], return 2
recentCounter.ping(3001);  // requests = [1,100,3001], range is [1,3001], return 3
recentCounter.ping(3002);  // requests = [1,100,3001,3002], range is [2,3002], return 3

```

**Constraints:**

- `1 <= t <= 109`
- Each test case will call `ping` with **strictly increasing** values of `t`.
- At most `104` calls will be made to `ping`.

Solution:

vector.erase(first,last) 时间复杂度是O(n-k), k是删除的数量

用queue的话是O(k)

既然是stream，queue应该更好

```cpp
class RecentCounter {
public:
    vector<int> nums;

    RecentCounter() {
        
    }
    
    int ping(int t) {
        nums.push_back(t);
        auto it=lower_bound(nums.begin(), nums.end(), t-3000);
        nums.erase(nums.begin(), it);
        return nums.size();
    }
};

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter* obj = new RecentCounter();
 * int param_1 = obj->ping(t);
 *
```