# Binary Trees With Factors

#: 823
Difficult: Medium
Tags: Array, Hash, Sort, dp-TimeSeqFromLastX
link: https://leetcode.com/problems/binary-trees-with-factors/
Priority: Medium
Created time: June 27, 2023 11:48 PM
Last edited time: October 28, 2023 1:51 AM
source: HuaHua, leetcode_daily

Given an array of unique integers, `arr`, where each integer `arr[i]` is strictly greater than `1`.

We make a binary tree using these integers, and each number may be used for any number of times. Each non-leaf node's value should be equal to the product of the values of its children.

Return *the number of binary trees we can make*. The answer may be too large so return the answer **modulo** `109 + 7`.

**Example 1:**

```
Input: arr = [2,4]
Output: 3
Explanation: We can make these trees:[2], [4], [4, 2, 2]
```

**Example 2:**

```
Input: arr = [2,4,5,10]
Output: 7
Explanation: We can make these trees:[2], [4], [5], [10], [4, 2, 2], [10, 2, 5], [10, 5, 2].
```

**Constraints:**

- `1 <= arr.length <= 1000`
- `2 <= arr[i] <= 109`
- All the values of `arr` are **unique**.

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int numFactoredBinaryTrees(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n=nums.size();
        //mp: val - idx
        unordered_map<int,int> mp;
        //dp
        vector<ll> dp(n,1);//at least itself
        for(int i=0; i<n; i++){
            for(int k=0;k<i;k++){
                if(nums[i]%nums[k]==0 && mp.count(nums[i]/nums[k])){
                    dp[i]+= dp[k] * dp[mp[nums[i]/nums[k]]];
                    dp[i]%=M;
                }
            }
            mp[nums[i]]=i; 
        }
        return accumulate(dp.begin(), dp.end(),0LL) % M;
    }
};
```