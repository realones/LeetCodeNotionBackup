# Longest Arithmetic Subsequence

#: 1027
Difficult: Medium
Tags: BS_todo, Hash, dp-TimeSeqFromLastX
link: https://leetcode.com/problems/longest-arithmetic-subsequence/description/
Priority: High
Status: More Solution, No Idea At First, noPatten, toSummarize
Created time: June 23, 2023 12:08 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an array `nums` of integers, return *the length of the longest arithmetic subsequence in* `nums`.

**Note** that:

- A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.
- A sequence `seq` is arithmetic if `seq[i + 1] - seq[i]` are all the same value (for `0 <= i < seq.length - 1`).

**Example 1:**

```
Input: nums = [3,6,9,12]
Output: 4
Explanation: The whole array is an arithmetic sequence with steps of length = 3.

```

**Example 2:**

```
Input: nums = [9,4,7,2,10]
Output: 3
Explanation: The longest arithmetic subsequence is [4,7,10].

```

**Example 3:**

```
Input: nums = [20,1,15,3,10,5,8]
Output: 4
Explanation: The longest arithmetic subsequence is [20,15,10,5].

```

**Constraints:**

- `2 <= nums.length <= 1000`
- `0 <= nums[i] <= 500`

Solution:

this is DP, but seems there is a BS solution from the question tag

```cpp
class Solution {
public:
    int longestArithSeqLength(vector<int>& nums) {
        int n=nums.size();
        int res=2;

        vector<unordered_map<int,int>> dp(n);
        for(int i=0;i<n;i++){
            for(int k=0;k<i;k++){
                int diff=nums[i]-nums[k];
                if(!dp[k].count(diff)){
                    dp[i][diff]=2;
                }
                else{
                    dp[i][diff]= max(dp[i][diff], dp[k][diff]+1);
                }
                res=max(res,dp[i][diff]);
            }
        }
        return res;
    }
};

/*
n: 2~1000
nums[i] 1~500
================================
key: len, diff

dp[i][diff]: 0..i, longest subseq len, with comon diff

dp[i][diff]= 
    dp[k][diff] + 1 (for k:0..i-1, diff=nums[i]-nums[k])
    if not k valid, dp[i][diff]=2

return max of dp[any][any]
================================
T: O(n^2):
    two iteraters
S: O(n^2)
    i: n numbers
    diff:
        i==1: 1 idx to its left: there is only 1 diff
        i==2: 2 idx to its left: there is only 2 diff
        ...
        i==n-1: n-1 idx to its left: there is only n-1 diff
*/
```