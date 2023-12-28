# Maximum Number of Achievable Transfer Requests

#: 1601
Difficult: Hard
Tags: Array, BackTracking, Bit Manipulation, Enumeration
link: https://leetcode.com/problems/maximum-number-of-achievable-transfer-requests
Priority: High
Status: HardToImplement, More Solution, No Idea At First
Created time: July 4, 2023 5:10 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

We have `n` buildings numbered from `0` to `n - 1`. Each building has a number of employees. It's transfer season, and some employees want to change the building they reside in.

You are given an array `requests` where `requests[i] = [fromi, toi]` represents an employee's request to transfer from building `fromi` to building `toi`.

**All buildings are full**, so a list of requests is achievable only if for each building, the **net change in employee transfers is zero**. This means the number of employees **leaving** is **equal** to the number of employees **moving in**. For example if `n = 3` and two employees are leaving building `0`, one is leaving building `1`, and one is leaving building `2`, there should be two employees moving to building `0`, one employee moving to building `1`, and one employee moving to building `2`.

Return *the maximum number of achievable requests*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/10/move1.jpg](https://assets.leetcode.com/uploads/2020/09/10/move1.jpg)

```
Input: n = 5, requests = [[0,1],[1,0],[0,1],[1,2],[2,0],[3,4]]
Output: 5
Explantion: Let's see the requests:
From building 0 we have employees x and y and both want to move to building 1.
From building 1 we have employees a and b and they want to move to buildings 2 and 0 respectively.
From building 2 we have employee z and they want to move to building 0.
From building 3 we have employee c and they want to move to building 4.
From building 4 we don't have any requests.
We can achieve the requests of users x and b by swapping their places.
We can achieve the requests of users y, a and z by swapping the places in the 3 buildings.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/09/10/move2.jpg](https://assets.leetcode.com/uploads/2020/09/10/move2.jpg)

```
Input: n = 3, requests = [[0,0],[1,2],[2,1]]
Output: 3
Explantion: Let's see the requests:
From building 0 we have employee x and they want to stay in the same building 0.
From building 1 we have employee y and they want to move to building 2.
From building 2 we have employee z and they want to move to building 1.
We can achieve all the requests.
```

**Example 3:**

```
Input: n = 4, requests = [[0,3],[3,1],[1,2],[2,0]]
Output: 4

```

**Constraints:**

- `1 <= n <= 20`
- `1 <= requests.length <= 16`
- `requests[i].length == 2`
- `0 <= fromi, toi < n`

Solution 1:

wisdom_peak solution - 

bitmask to find all subset, can be improved by next_permutation / Gospher's Hack(out of scope)

```cpp
class Solution {
public:
    int maximumRequests(int n, vector<vector<int>>& requests) {
        int m=requests.size();
        int res=0;
        for(int state=0; state<(1<<m); state++){
            if(valid(state, n , requests)){
                std::cout << "valid" << std::endl;
                res=max(res,s2c(state));
            }
        }
        return res;
    }

    bool valid(int s, int n, vector<vector<int>>& requests){
        int buildings[n];
        for(int i=0; i<n; i++){buildings[i]=0;}

        int m=requests.size();
        for(int i=0; i<m; i++){
            if((s>>i)&1){
                buildings[requests[i][0]]--;
                buildings[requests[i][1]]++;
            }
        }
        for(int i=0; i<n; i++){
            if(buildings[i]!=0) return false;
        }
        return true;
    }

    int s2c(int s){
        int res=0;
        while(s){
            res+=s&1;
            s>>=1;
        }
        return res;
    }
};

/*
bitmask to find all subset:

for each state
    if valid
        update res with cur reqCnt

T: 2^m(each state) * (m(each request) + n(buildings))

Improvement:
11111 ok? -> 5
11110 11101 11011 10111 01111 ok? -> 4
...

next_permutation / Gospher's Hack(out of scope)

*/
```