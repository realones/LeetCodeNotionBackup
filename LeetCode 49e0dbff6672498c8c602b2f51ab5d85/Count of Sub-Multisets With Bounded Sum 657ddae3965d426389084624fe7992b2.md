# Count of Sub-Multisets With Bounded Sum

#: 2902
Difficult: Hard
Tags: Array, Hash, Sliding Window, dp-backpack
link: https://leetcode.com/problems/count-of-sub-multisets-with-bounded-sum/description/
Priority: High
Status: No Idea At First, toRevisit, toSummarize
Created time: October 16, 2023 10:42 PM
Last edited time: October 17, 2023 6:24 PM
source: LC_Contests

You are given a **0-indexed** array `nums` of non-negative integers, and two integers `l` and `r`.

Return *the **count of sub-multisets** within* `nums` *where the sum of elements in each subset falls within the inclusive range of* `[l, r]`.

Since the answer may be large, return it modulo `109 + 7`.

A **sub-multiset** is an **unordered** collection of elements of the array in which a given value `x` can occur `0, 1, ..., occ[x]` times, where `occ[x]` is the number of occurrences of `x` in the array.

**Note** that:

- Two **sub-multisets** are the same if sorting both sub-multisets results in identical multisets.
- The sum of an **empty** multiset is `0`.

**Example 1:**

```
Input: nums = [1,2,2,3], l = 6, r = 6
Output: 1
Explanation: The only subset of nums that has a sum of 6 is {1, 2, 3}.

```

**Example 2:**

```
Input: nums = [2,1,4,2,7], l = 1, r = 5
Output: 7
Explanation: The subsets of nums that have a sum within the range [1, 5] are {1}, {2}, {4}, {2, 2}, {1, 2}, {1, 4}, and {1, 2, 2}.

```

**Example 3:**

```
Input: nums = [1,2,1,3,5,2], l = 3, r = 5
Output: 9
Explanation: The subsets of nums that have a sum within the range [3, 5] are {3}, {5}, {1, 2}, {1, 3}, {2, 2}, {2, 3}, {1, 1, 2}, {1, 1, 3}, and {1, 2, 2}.
```

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `0 <= nums[i] <= 2 * 104`
- Sum of `nums` does not exceed `2 * 104`.
- `0 <= l <= r <= 2 * 104`

comment: no idea, checked wisdompeak

todo: 理解了但是没有真的去实现correct solution，去实现一下。另外整理错位相消在backpack里的应用

Solution TLE:

```cpp
using ii=pair<int,int>;
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int countSubMultisets(vector<int>& nums, int l, int r) {
        unordered_map<int,int> mp;//val,cnt
        for(auto x: nums) mp[x]++;
        vector<ii> v;//val,cnt
        for(auto [val, cnt]: mp) v.emplace_back(val,cnt);
        return (helper(v,r) - helper(v,l-1) + M) %M;
    }

    ll helper(vector<ii>& v, int packSz){
        if(packSz<0) return 0;
        int n=v.size();
        vector<vector<int>> dp(n+1,vector<int>(packSz+1,0));
        dp[0][0]=1;
        for(int i=1;i<dp.size();i++){
            auto [val,cnt]=v[i-1];
            for(int j=0;j<dp[0].size();j++){
                for(int k=0; k<=cnt && j-k*val>=0 ; k++){
                    dp[i][j] += dp[i-1][j-k*val];
                    dp[i][j] %=M;
                }
            }
        }
        //return dp[n][packSz];// not just packSz, but any s that s<=packSz
        return accumulate(dp[n].begin(), dp[n].end(),0LL)%M;
    }
};

/*
intui:
helper(r)-helper(l-1);

2e4: O(n*sqrt(n)): nums.sum = k, then unique element count is sqrt(k) lvl, since x(x+1)/2<=k

backpack, with cnt for each val

dp[i:(1~n)][j:(0~packSz)]: first i elems, to build sum==j, count of multisets

dp[i][j] sum= dp[i-1][j-k*cur], for k: j-k*cur>=0 && k<=cnt

init: dp[0][0]=1, other=0

return:
    dp[n][r] - dp[n][l-1] (if l-1>=0, else 0)

*/
```

Solution wisdomPeak:

下表是等差数列 =》 错位相消

`dp[i][j] = dp[i-1][j] + dp[i-1][j-vi] + dp[i-1][j-vi*2] + ... + dp[i-1][j-vi*ai]`

-
`dp[i][j-vi] = dp[i-1][j-vi] + dp[i-1][j-vi*2] + dp[i-1][j-vi*3] + ... + dp[i-1][j-vi*(ai+1)]`

=

`dp[i][j] = dp[i][j-vi] + dp[i-1][j] - dp[i-1][j-vi*(ai+1)]`

不需要上面的k循环了

==》

上面的转移方程中元素的值v不能为0，否则表达式成了`dp[i][j]=dp[i][j]`失去了意义。解决办法是将这种元素先踢出去。最终考虑零元素取任何个数都可以。故最终将答案乘以count0+1，其中count0是零元素的个数。

`dp[i][j] = (j<v?0:dp[i][j-v]) + dp[i-1][j] - (j<v*(c+1)?0:dp[i-1][j-v*(c+1)]);`

```cpp
using LL = long long;
LL M = 1e9+7;
class Solution {
    vector<pair<int,int>>arr;  
    int count0 = 0;
public:
    int countSubMultisets(vector<int>& nums, int l, int r)     
    {        
        unordered_map<int,int>Map;
        for (int x: nums)
        {
            if (x==0) count0++;
            else Map[x]++;
        }
                                    
        for (auto& p:Map)
            arr.push_back(p);
            
        arr.insert(arr.begin(), {0,0});
        
        return (helper(r) - helper(l-1) + M) % M;        
    }
    
    int helper(int limit)
    {
        if (limit<0) return 0;
        
        int n = arr.size() - 1;
        
        vector<vector<LL>>dp(n+1, vector<LL>(limit+1, 0));
        
        dp[0][0] = 1;
                
        for (int i=1; i<=n; i++)
        {
            auto [v, c] = arr[i];            
            for (int j=0; j<=limit; j++)
            {                
                dp[i][j] = (j<v?0:dp[i][j-v]) + dp[i-1][j] - (j<v*(c+1)?0:dp[i-1][j-v*(c+1)]);
                dp[i][j] = (dp[i][j]+M) % M;
            }
        }
        
        LL ret = 0;
        for (int j=0; j<=limit; j++)
            ret = (ret+dp[n][j]) % M;
                
        return max(1LL, ret) * (count0+1) % M;
    }
};
```