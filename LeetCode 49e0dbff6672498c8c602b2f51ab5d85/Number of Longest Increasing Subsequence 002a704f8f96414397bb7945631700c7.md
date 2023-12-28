# Number of Longest Increasing Subsequence

#: 673
Difficult: Medium
Tags: Array, BinaryIndexedTree, DP, SegmentTree_todo, dp-TimeSeqFromLastX
link: https://leetcode.com/problems/number-of-longest-increasing-subsequence/
Priority: High
Status: HardToImplement, More Solution
Created time: July 21, 2023 12:26 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily
related: Longest Increasing Subsequence (Longest%20Increasing%20Subsequence%20dbe9919e92d04b90928f4e005fc84e25.md)

Given an integer array `nums`, return *the number of longest increasing subsequences.*

**Notice** that the sequence has to be **strictly** increasing.

**Example 1:**

```
Input: nums = [1,3,5,4,7]
Output: 2
Explanation: The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].

```

**Example 2:**

```
Input: nums = [2,2,2,2,2]
Output: 5
Explanation: The length of the longest increasing subsequence is 1, and there are 5 increasing subsequences of length 1, so output 5.

```

**Constraints:**

- `1 <= nums.length <= 2000`
- `106 <= nums[i] <= 106`

Solution:

dp from last all

从LIS题目出发，intuition很容易想，思路和实现卡了一下，可能因为半夜刷题

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int n=nums.size();//n: 1~2000, val: -1e6~1e6
        int cnt=0, longestLen=0;
        vector<ii> dp(n,{1,1});//{longest len with cur, cnt of longest len =1}
        for(int i=0; i<n; i++){
            for(int k=0; k<i; k++){
                if(nums[k]<nums[i]){//inc
                    if(dp[i].first<dp[k].first+1){
                        dp[i].first=dp[k].first+1;
                        dp[i].second=dp[k].second;
                    }
                    else if(dp[i].first==dp[k].first+1){
                        dp[i].second+=dp[k].second;
                    }
                }
            }

            if(dp[i].first>longestLen){
                longestLen=dp[i].first;
                cnt=dp[i].second;
            }
            else if(dp[i].first==longestLen){
                cnt+=dp[i].second;
            }
        }
        return cnt;
    }
};

/*
for each
    dp from last all, longest, maintain a count
    
    if inc 
        update longest len
        if longest len get bigger, 
    
*/
```