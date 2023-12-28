# Subarray Sum Equals K

#: 560
Difficult: Medium
Tags: Hash, Prefix
link: https://leetcode.com/problems/subarray-sum-equals-k/
Priority: Low
Status: checkBetterSolution
Created time: June 27, 2023 9:28 PM
Last edited time: November 20, 2023 3:33 AM
source: HuaHua, appleFreq

Given an array of integers `nums` and an integer `k`, return *the total number of subarrays whose sum equals to* `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

```
Input: nums = [1,1,1], k = 2
Output: 2

```

**Example 2:**

```
Input: nums = [1,2,3], k = 3
Output: 2

```

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `1000 <= nums[i] <= 1000`
- `107 <= k <= 107`

comment: only beat 5%, check better solution

Solution:

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_multiset<int> prefixSum;
        prefixSum.insert(0);//init
        int sum=0;
        int res=0;
        for(auto x: nums){
            sum+=x;
            res+=prefixSum.count(sum-k);
            prefixSum.insert(sum);
        }
        return res;
    }
};
```