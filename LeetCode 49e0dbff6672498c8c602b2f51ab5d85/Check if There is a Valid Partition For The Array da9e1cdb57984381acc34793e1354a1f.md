# Check if There is a Valid Partition For The Array

#: 2369
Difficult: Medium
Tags: Array, dp-TimeSeqFromLastOne-Power
link: https://leetcode.com/problems/check-if-there-is-a-valid-partition-for-the-array/description/
Priority: Low
Created time: August 15, 2023 7:31 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given a **0-indexed** integer array `nums`. You have to partition the array into one or more **contiguous** subarrays.

We call a partition of the array **valid** if each of the obtained subarrays satisfies **one** of the following conditions:

1. The subarray consists of **exactly** `2` equal elements. For example, the subarray `[2,2]` is good.
2. The subarray consists of **exactly** `3` equal elements. For example, the subarray `[4,4,4]` is good.
3. The subarray consists of **exactly** `3` consecutive increasing elements, that is, the difference between adjacent elements is `1`. For example, the subarray `[3,4,5]` is good, but the subarray `[1,3,5]` is not.

Return `true` *if the array has **at least** one valid partition*. Otherwise, return `false`.

**Example 1:**

```
Input: nums = [4,4,4,5,6]
Output: true
Explanation: The array can be partitioned into the subarrays [4,4] and [4,5,6].
This partition is valid, so we return true.

```

**Example 2:**

```
Input: nums = [1,1,1,2]
Output: false
Explanation: There is no valid partition for this array.

```

**Constraints:**

- `2 <= nums.length <= 105`
- `1 <= nums[i] <= 106`

```cpp
class Solution {
public:
    bool validPartition(vector<int>& nums) {
        int n=nums.size();
        if(n==2) return nums[0]==nums[1];
        vector<bool> dp(n,false);
        dp[0]=false;
        dp[1]=nums[0]==nums[1];
        dp[2]=(nums[0]==nums[1]&&nums[1]==nums[2]) || 
              (nums[0]+1==nums[1]&&nums[1]+1==nums[2]);
            
        for(int i=3; i<n; i++){
            dp[i]=(
                (dp[i-2] && nums[i-1]==nums[i])
                ||
                (dp[i-3] && nums[i-2]==nums[i-1] && nums[i-1]==nums[i])
                ||
                (dp[i-3] && nums[i-2]+1==nums[i-1] && nums[i-1]+1==nums[i])
            );
        }
        return dp.back();
    }
};

/*
len: 2~1e5
val: 1~1e5
================================
2,2
3,3,3
1,2,3
================================
dp i: valid for 0...i, i:0~n-1
dp i = any true (dp k && [k+1,i]), k at most i-3
init
    dp0 false
    dp1 if true if [0]==[1]
    dp3 if true if [0]==[1]==[2] or inc inc
return: dp[n-1]
================================
T/S O(n)
*
```