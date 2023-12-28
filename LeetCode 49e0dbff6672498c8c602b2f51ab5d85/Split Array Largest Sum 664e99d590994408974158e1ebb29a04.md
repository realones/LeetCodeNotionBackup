# Split Array Largest Sum

#: 410
Difficult: Hard
Tags: BS_todo, Greedy, dp-K_Ranges
link: https://leetcode.com/problems/split-array-largest-sum/
Priority: Medium
Created time: December 4, 2022 11:50 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路

Given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.

Return *the minimized largest sum of the split*.

A **subarray** is a contiguous part of the array.

**Example 1:**

```
Input: nums = [7,2,5,10,8], k = 2
Output: 18
Explanation: There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.

```

**Example 2:**

```
Input: nums = [1,2,3,4,5], k = 2
Output: 9
Explanation: There are four ways to split nums into two subarrays.
The best way is to split it into [1,2,3] and [4,5], where the largest sum among the two subarrays is only 9.

```

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 106`
- `1 <= k <= min(50, nums.length)`

```cpp
class Solution {
public:
    int splitArray(vector<int>& nums, int K) {
        int n=nums.size(); if(n==0) {return 0;}
        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        
        vector<vector<int>> dp(n,vector<int>(K+1,INT_MAX));
        for(int i=0; i<n; i++){
            for(int k=1; k<=min(K,i+1); k++){
                for(int j=i; j>=k-1; j--){
                    int tmp=max(j-1>=0?dp[j-1][k-1]:0,getPrefixSum(prefixSum,j,i));
                    dp[i][k]=min(dp[i][k], tmp);
                }
            }
        }
        
        return dp.back().back();
    }
    
    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```