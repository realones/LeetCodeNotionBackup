# Subarray Sums Divisible by K

#: 974
Difficult: Medium
Tags: Array, Hash, Prefix
link: https://leetcode.com/problems/subarray-sums-divisible-by-k/submissions/1118757245/
Priority: Low
Created time: December 13, 2023 2:28 AM
Last edited time: December 13, 2023 2:29 AM
source: microsoftFreq

Given an integer array `nums` and an integer `k`, return *the number of non-empty **subarrays** that have a sum divisible by* `k`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

```
Input: nums = [4,5,0,-2,-3,1], k = 5
Output: 7
Explanation: There are 7 subarrays with a sum divisible by k = 5:
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]

```

**Example 2:**

```
Input: nums = [5], k = 9
Output: 0

```

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `104 <= nums[i] <= 104`
- `2 <= k <= 104`

```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        int n=nums.size();
        unordered_map<int,int> mp;//modVal - cnt
        mp[0]=1;
        int res=0;
        int sum=0;
        for(auto x: nums){
            sum+=x;
            //update res
            res+=mp[sum%k];
            //add to mp
            mp[sum%k]++;
        }
        return res;
    }
};

/*
return: num of non-empty subarrs with sum%k==0;
================================
nums:
    len: 1~3e4
    val: -1e4~1e4
k: 2~1e4
================================
prefixSumMod

prefix[j] == prefix[i] -> find
maintain mp[modVal,cnt]
*/
```