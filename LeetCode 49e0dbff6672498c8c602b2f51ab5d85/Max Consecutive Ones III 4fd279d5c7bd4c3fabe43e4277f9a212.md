# Max Consecutive Ones III

#: 1004
Difficult: Medium
Tags: Array, BS_Val, Prefix, Sliding Window
link: https://leetcode.com/problems/max-consecutive-ones-iii
Priority: High
Status: No Idea At First
Created time: July 2, 2023 11:22 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75
related: Maximize the Confusion of an Exam (Maximize%20the%20Confusion%20of%20an%20Exam%207d877ecb977d41eda6373d9b4a6e0e78.md)

Given a binary array `nums` and an integer `k`, return *the maximum number of consecutive* `1`*'s in the array if you can flip at most* `k` `0`'s.

**Example 1:**

```
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

**Example 2:**

```
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.
- `0 <= k <= nums.length`

Solution dp - TLE:

state machine

```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int res=0;
        int n=nums.size();
        vector<vector<int>> dp(n+1,vector<int>(k+1,-1));
        dp[0][0]=0;
        for(int i=1; i<n+1; i++){
            int cur=nums[i-1];
            for(int j=0; j<k+1; j++){
                if(cur==0){
                    if(j==0) dp[i][j]=0;
                    else{
                        dp[i][j]=dp[i-1][j-1]+1;
                    }
                }
                else{
                    dp[i][j]=dp[i-1][j]+1;
                }
                res=max(res,dp[i][j]);
            }
        }
        return res;
    }
}
```

Solution sliding window:

```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int n=nums.size(); if(n==0) {return 0;}
        int res=0;
        for(int l=0,r=0;r<n;r++){
            if(nums[r]==0) k--;
            while(k<0) {
                if(nums[l]==0) k++;
                l++;
            };
            res=max(res,r-l+1);
        }
        return res;
    }
};

/*
Intuition - from lee215
Translation:
Find the longest subarray with at most K zeros.
*/
```