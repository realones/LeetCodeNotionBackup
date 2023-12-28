# Sort Array By Parity

#: 905
Difficult: Easy
Tags: Partition
link: https://leetcode.com/problems/sort-array-by-parity/
Priority: Low
Created time: August 9, 2022 11:13 AM
Last edited time: October 19, 2023 9:30 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers

Given an integer array `nums`, move all the even integers at the beginning of the array followed by all the odd integers.

Return ***any array** that satisfies this condition*.

**Example 1:**

```
Input: nums = [3,1,2,4]
Output: [2,4,3,1]
Explanation: The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.

```

**Example 2:**

```
Input: nums = [0]
Output: [0]

```

**Constraints:**

- `1 <= nums.length <= 5000`
- `0 <= nums[i] <= 5000`

(todo: better solutions skipped than partition first then interleaving)

```cpp
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return nums;}
        int l=0, r=n-1;
        while(l<=r){
            while(l<=r && nums[l]%2==0){l++;}
            while(l<=r && nums[r]%2==1){r--;}
            if(l<=r){
                swap(nums[l++],nums[r--]);
            }
        }
        return nums;
    }
};
```