# Largest Sum of Averages

#: 813
Difficult: Medium
Tags: dp-K_Ranges
link: https://leetcode.com/problems/largest-sum-of-averages/
Priority: Medium
Created time: December 1, 2022 4:13 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路

You are given an integer array `nums` and an integer `k`. You can partition the array into **at most** `k` non-empty adjacent subarrays. The **score** of a partition is the sum of the averages of each subarray.

Note that the partition must use every integer in `nums`, and that the score is not necessarily an integer.

Return *the maximum **score** you can achieve of all the possible partitions*. Answers within `10-6` of the actual answer will be accepted.

**Example 1:**

```
Input: nums = [9,1,2,3,9], k = 3
Output: 20.00000
Explanation:
The best choice is to partition nums into [9], [1, 2, 3], [9]. The answer is 9 + (1 + 2 + 3) / 3 + 9 = 20.
We could have also partitioned nums into [9, 1], [2], [3, 9], for example.
That partition would lead to a score of 5 + 2 + 6 = 13, which is worse.

```

**Example 2:**

```
Input: nums = [1,2,3,4,5,6,7], k = 4
Output: 20.50000

```

**Constraints:**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 104`
- `1 <= k <= nums.length`

```cpp
class Solution {
public:
    double largestSumOfAverages(vector<int>& nums, int K) {
        int n=nums.size(); if(n==0) {return 0;}
        
        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        
        vector<vector<double>> dp(n,vector<double>(K+1,0));
        
        for(int i=0; i<n; i++){ dp[i][0]=INT_MIN; }//assign invalid number
        
        for(int i=0;i<n;i++){
            for(int k=1;k<=min(i+1,K);k++){
                for(int j=i;j>=k-1;j--){
                    double aveji=getPrefixSum(prefixSum,j,i)*1.0/(i-j+1);
                    dp[i][k]=max(dp[i][k], (j-1>=0?dp[j-1][k-1]:0) + aveji);
                }
            }
        }
        int res=0;
        //return *max_element(dp.back().begin(), dp.back().end());
				return dp.back().back();
    }
    
    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```