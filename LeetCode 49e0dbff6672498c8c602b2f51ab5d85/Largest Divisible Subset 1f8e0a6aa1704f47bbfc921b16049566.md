# Largest Divisible Subset

#: 368
Difficult: Medium
Tags: Array, DP, GoForth, UsePreResults, dp-TimeSeqFromLastX
link: https://leetcode.com/problems/largest-divisible-subset/
Priority: Medium
Created time: August 18, 2022 12:25 PM
Last edited time: October 22, 2023 12:14 AM
practicedTimes: 1
source: jiuzhang, wp动归套路, 算初DP

Given a set of **distinct** positive integers `nums`, return the largest subset `answer` such that every pair `(answer[i], answer[j])` of elements in this subset satisfies:

- `answer[i] % answer[j] == 0`, or
- `answer[j] % answer[i] == 0`

If there are multiple solutions, return any of them.

**Example 1:**

```
Input: nums = [1,2,3]
Output: [1,2]
Explanation: [1,3] is also accepted.

```

**Example 2:**

```
Input: nums = [1,2,4,8]
Output: [1,2,4,8]

```

**Constraints:**

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 2 * 109`
- All the integers in `nums` are **unique**.

dp+记录parent

```cpp
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        int n=nums.size();
        vector<int> res;
        if(n==0) return res;
        int maxNum=1;
        int end=-1;
        sort(nums.begin(),nums.end());
        vector<int> dp(n,1);
        vector<int> parent(n,-1);
        
        for(int k=0;k<n;k++){
            for(int i=0;i<k;i++){
                if(nums[k]%nums[i]==0 &&dp[i]+1>dp[k]){
                    dp[k]=dp[i]+1;
                    parent[k]=i;//simulate linked list from k to start
                }
            }
            if(dp[k]>=maxNum){
                maxNum=dp[k];
                end=k;
            }   
        }
        
         for(int i = 0; i < maxNum; i++){
            res.push_back(nums[end]);
            end=parent[end];
        }
        
        return res;
    }
};
```