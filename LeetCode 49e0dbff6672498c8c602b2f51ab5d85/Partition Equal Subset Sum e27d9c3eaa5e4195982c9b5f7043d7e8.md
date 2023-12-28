# Partition Equal Subset Sum

#: 416
Difficult: Medium
Tags: Array, dp-backpack
link: https://leetcode.com/problems/partition-equal-subset-sum/
Priority: Low
Created time: June 28, 2023 12:51 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an integer array `nums`, return `true` *if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or* `false` *otherwise*.

**Example 1:**

```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].

```

**Example 2:**

```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.

```

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n=nums.size();
        int total=accumulate(nums.begin(), nums.end(),0);
        if(total%2==1) return false;
        int packSz=total/2;
        vector<vector<bool>> dp(n+1,vector<bool>(packSz+1,false));
        dp[0][0]=true;
        for(int i=1; i<n+1; i++){
            int cur=nums[i-1];
            for(int j=0; j<packSz+1; j++){
                dp[i][j]=
                    dp[i-1][j] ||//notTake
                    (j-cur>=0?dp[i-1][j-cur]:false);//take
            }
        }
        return dp.back().back();
    }
};
/*
backpack for packSz=sum/2
*/
```