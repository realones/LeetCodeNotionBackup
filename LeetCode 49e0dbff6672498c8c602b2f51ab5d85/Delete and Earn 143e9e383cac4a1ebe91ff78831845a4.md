# Delete and Earn

#: 740
Difficult: Medium
Tags: dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/delete-and-earn/
Priority: Medium
Created time: June 27, 2023 9:53 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: House Robber (House%20Robber%20e654ada840b84406a958d430aba01166.md)

You are given an integer array `nums`. You want to maximize the number of points you get by performing the following operation any number of times:

- Pick any `nums[i]` and delete it to earn `nums[i]` points. Afterwards, you must delete **every** element equal to `nums[i] - 1` and **every** element equal to `nums[i] + 1`.

Return *the **maximum number of points** you can earn by applying the above operation some number of times*.

**Example 1:**

```
Input: nums = [3,4,2]
Output: 6
Explanation: You can perform the following operations:
- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = [2].
- Delete 2 to earn 2 points. nums = [].
You earn a total of 6 points.

```

**Example 2:**

```
Input: nums = [2,2,3,3,3,4]
Output: 9
Explanation: You can perform the following operations:
- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = [3,3].
- Delete a 3 again to earn 3 points. nums = [3].
- Delete a 3 once more to earn 3 points. nums = [].
You earn a total of 9 points.
```

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `1 <= nums[i] <= 104`

comment:

1. check better solution since Runtime Beats 6.40%of users with C++
2. to summarize the state_machine dp with init of dp[n+1] that i for dp, x for input

Solution:

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int deleteAndEarn(vector<int>& nums) {
        map<int,int> mp;//num, cnt
        for(auto x: nums) mp[x]++;
        vector<ii> v;
        for(auto [num,cnt]: mp) v.push_back({num,cnt});
        v.insert(v.begin(), {-1,-1});
        int n=v.size();
        vector<vector<int>> dp(n,vector<int>(2,INT_MIN));//0:not_use, 1:use
        dp[0][0]=0;
        for(int i=1; i<n; i++){
            dp[i][0]=max(dp[i-1][0], dp[i-1][1]);
            dp[i][1]=max(dp[i-1][0], (v[i-1].first+1==v[i].first)?0:dp[i-1][1]) + v[i].first*v[i].second;
        }
        return max(dp.back()[0], dp.back()[1]);
    }
};

/*
pick any number, delete the neighbors

pick one number -> pick all same numbers

so maintain by:
    [(num,cnt)]
sort by num

e.g. 2*3 3*4 4*1 6*2

use it
not use it

dp - statemachine
================================
state: 
    not_use = max(preNU, preU)
    use = max(preNU, connected?0:preU) + cur

init:
    use=INT_MIN(invalid)
    not_use=0
    init connected=false -> nums.insert(begin, (0,0))

return max[last_u,last_nu]

*/
```