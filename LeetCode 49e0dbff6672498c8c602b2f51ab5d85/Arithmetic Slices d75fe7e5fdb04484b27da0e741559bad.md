# Arithmetic Slices

#: 413
Difficult: Medium
Tags: Array, DP
link: https://leetcode.com/problems/arithmetic-slices/description/
Priority: Low
Status: More Solution
Created time: December 24, 2023 4:55 PM
Last edited time: December 24, 2023 4:56 PM
source: googleFreq

An integer array is called arithmetic if it consists of **at least three elements** and if the difference between any two consecutive elements is the same.

- For example, `[1,3,5,7,9]`, `[7,7,7,7]`, and `[3,-1,-5,-9]` are arithmetic sequences.

Given an integer array `nums`, return *the number of arithmetic **subarrays** of* `nums`.

A **subarray** is a contiguous subsequence of the array.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: 3
Explanation: We have 3 arithmetic slices in nums: [1, 2, 3], [2, 3, 4] and [1,2,3,4] itself.

```

**Example 2:**

```
Input: nums = [1]
Output: 0

```

**Constraints:**

- `1 <= nums.length <= 5000`
- `1000 <= nums[i] <= 1000`

comment: why DP tag

```cpp
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& nums) {
        if(nums.size()<3) return 0;
        nums.push_back(INT_MAX/2);//end sign
        int n=nums.size();// if(n==0) {return 0;}
        int res=0;
        int len=2;//init, and (n-2)*(n-1)/2=0, so good
        for(int i=2; i<n; i++){
            if(nums[i]-nums[i-1]==nums[i-1]-nums[i-2]) len++;
            else {
                res+=(len-2)*(len-1)/2;//accumulate res
                len=2;//init val
            }
        }
        return res;
    }
};

//find longest arithmetic subarr, res+= (n-2) * (n-1) /2
// longest arithmetic aubarr 有排他性exclusive，如果找到A，不可能有B和A有重合部分
```