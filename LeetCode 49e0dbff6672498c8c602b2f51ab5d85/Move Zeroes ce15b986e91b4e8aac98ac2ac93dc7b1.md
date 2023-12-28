# Move Zeroes

#: 283
Difficult: Easy
Tags: 2pt_SameDir_diffSpeed, Array, swap_put_keep_sequence
link: https://leetcode.com/problems/move-zeroes/
Priority: High
Created time: August 3, 2022 1:37 PM
Last edited time: October 19, 2023 9:58 PM
practicedTimes: 1
source: HuaHua, LC_75, jiuzhang, wisdompeak, 算初TwoPointers
related: Sort Colors (Sort%20Colors%20b67e9929642045f0a0e1548122f219d0.md), Separate Black and White Balls (Separate%20Black%20and%20White%20Balls%20e7002c023486446da8f4067f7b15cddc.md)

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

**Example 1:**

```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]

```

**Example 2:**

```
Input: nums = [0]
Output: [0]

```

**Constraints:**

- `1 <= nums.length <= 104`
- `231 <= nums[i] <= 231 - 1`

**Follow up:**

Could you minimize the total number of operations done?

swap保证顺序, 类似直接put

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return;}
        int toPut=0;
        for(int i=0; i<n; i++){
            if(nums[i]){
                //nums[toPut++]=nums[i];
                swap(nums[toPut++],nums[i]);
            }
        }
    }
};
```

```jsx
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
    int lastNonZeroFoundAt = 0;
    // If the current element is not 0, then we need to
    // append it just in front of last non 0 element we found. 
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] != 0) {
            nums[lastNonZeroFoundAt++] = nums[i];
        }
    }
    // After we have finished processing new elements,
    // all the non-zero elements are already at beginning of array.
    // We just need to fill remaining array with 0's.
    for (int i = lastNonZeroFoundAt; i < nums.size(); i++) {
        nums[i] = 0;
    }
}
};
```