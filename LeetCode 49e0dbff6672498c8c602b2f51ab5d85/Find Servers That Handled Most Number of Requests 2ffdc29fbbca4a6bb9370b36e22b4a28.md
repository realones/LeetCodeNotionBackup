# Find Servers That Handled Most Number of Requests

#: 1606
Difficult: Hard
Tags: OrderSet, PQ
link: https://leetcode.com/problems/find-servers-that-handled-most-number-of-requests/
Priority: High
Status: Dig More, HardToImplement, More Solution, No Idea At First
Created time: October 8, 2022 9:50 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, googleFreq

You have `k` servers numbered from `0` to `k-1` that are being used to handle multiple requests simultaneously. Each server has infinite computational capacity but **cannot handle more than one request at a time**. The requests are assigned to servers according to a specific algorithm:

- The `ith` (0-indexed) request arrives.
- If all servers are busy, the request is dropped (not handled at all).
- If the `(i % k)th` server is available, assign the request to that server.
- Otherwise, assign the request to the next available server (wrapping around the list of servers and starting from 0 if necessary). For example, if the `ith` server is busy, try to assign the request to the `(i+1)th` server, then the `(i+2)th` server, and so on.

You are given a **strictly increasing** array `arrival` of positive integers, where `arrival[i]` represents the arrival time of the `ith` request, and another array `load`, where `load[i]` represents the load of the `ith` request (the time it takes to complete). Your goal is to find the **busiest server(s)**. A server is considered **busiest** if it handled the most number of requests successfully among all the servers.

Return *a list containing the IDs (0-indexed) of the **busiest server(s)***. You may return the IDs in any order.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/08/load-1.png](https://assets.leetcode.com/uploads/2020/09/08/load-1.png)

```
Input: k = 3, arrival = [1,2,3,4,5], load = [5,2,3,3,3]
Output: [1]
Explanation:
All of the servers start out available.
The first 3 requests are handled by the first 3 servers in order.
Request 3 comes in. Server 0 is busy, so it's assigned to the next available server, which is 1.
Request 4 comes in. It cannot be handled since all servers are busy, so it is dropped.
Servers 0 and 2 handled one request each, while server 1 handled two requests. Hence server 1 is the busiest server.

```

**Example 2:**

```
Input: k = 3, arrival = [1,2,3,4], load = [1,2,1,2]
Output: [0]
Explanation:
The first 3 requests are handled by first 3 servers.
Request 3 comes in. It is handled by server 0 since the server is available.
Server 0 handled two requests, while servers 1 and 2 handled one request each. Hence server 0 is the busiest server.

```

**Example 3:**

```
Input: k = 3, arrival = [1,2,3], load = [10,12,11]
Output: [0,1,2]
Explanation: Each server handles a single request, so they are all considered the busiest.

```

**Constraints:**

- `1 <= k <= 105`
- `1 <= arrival.length, load.length <= 105`
- `arrival.length == load.length`
- `1 <= arrival[i], load[i] <= 109`
- `arrival` is **strictly increasing**.

```cpp
/*
maintain free servers and busy servers
free: a list of server
				step2: add freed servers from busy to free
        step3: find(lower_bound) -> sorted
        step4: remove 
				-> set O(logk)
			init with all
busy: a list of {finishTime=a+l, server}, 
				step1: rm severs with finishTime <= current arrival time
        step5: add to lis
				- > pq<greater<>>
			init empty

T: O(nlogk)
S: O(k)
*/

class Solution {
public:
    vector<int> busiestServers(int k, vector<int>& arrival, vector<int>& load) {
        set<int> free;
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<>> busy;
        
        //init: for k servers, put into free set
        for(int i=0;i<k;i++){
            free.insert(i);
        }
        
        vector<int> requestCnt(k,0);
        
        for(int i=0;i<arrival.size();i++){
            //for all busy ones with finish time <= arrival time, pop to free
            while(!busy.empty() && busy.top().first<=arrival[i]){
                int server = busy.top().second; busy.pop();
                free.insert(server);
            }
            //if free empty, ingore
            if(free.empty()) continue;
            //if free.low_bound find, or free.first
            auto iter=free.lower_bound(i%k);
            if(iter==free.end()) iter=free.begin();
            //- use it and put to busy
            requestCnt[*iter]++;
            busy.push({arrival[i]+load[i],*iter});
            free.erase(iter);
        }
        
        int maxCnt=*max_element(requestCnt.begin(), requestCnt.end());
        vector<int> res;
        for(int i=0; i<k; i++){
            if(requestCnt[i]==maxCnt){res.push_back(i);}
        }
        return res;
    }
};
```