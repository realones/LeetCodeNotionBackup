# Maximum Number of Events That Can Be Attended

#: 1353
Difficult: Medium
Tags: Array, Greedy, PQ, Sort
link: https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended
Priority: High
Status: No Idea At First
Created time: June 27, 2023 11:15 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75

You are given an array of `events` where `events[i] = [startDayi, endDayi]`. Every event `i` starts at `startDayi` and ends at `endDayi`.

You can attend an event `i` at any day `d` where `startTimei <= d <= endTimei`. You can only attend one event at any time `d`.

Return *the maximum number of events you can attend*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/02/05/e1.png](https://assets.leetcode.com/uploads/2020/02/05/e1.png)

```
Input: events = [[1,2],[2,3],[3,4]]
Output: 3
Explanation: You can attend all the three events.
One way to attend them all is as shown.
Attend the first event on day 1.
Attend the second event on day 2.
Attend the third event on day 3.

```

**Example 2:**

```
Input: events= [[1,2],[2,3],[3,4],[1,2]]
Output: 4

```

**Constraints:**

- `1 <= events.length <= 105`
- `events[i].length == 2`
- `1 <= startDayi <= endDayi <= 105`

```cpp
class Solution {
public:
    int maxEvents(vector<vector<int>>& events) {
        sort(events.begin(), events.end());
        priority_queue<int,vector<int>,greater<>> pq;

        int res=0;
        int i=0;
        for(int d=1;d<=1e5;d++){
            while(!pq.empty() && pq.top()<d) pq.pop();
            while(i<events.size() && events[i][0]==d) pq.push(events[i++][1]);
            if(!pq.empty()){
                pq.pop();
                res++;
            }
        }
        return res;
    }
};

/*
from lee215:
greedy:

sort events by starting date
pq to keep current open events (store the end date, top is earlist end date one) -> pq_greater<>

for each day from 1 to 1e5
    pop events that has ended
    if new event starts that day, add new event to pq
    attend the top event from pq(earlist end date one) - greedy, and inc res

*/
```