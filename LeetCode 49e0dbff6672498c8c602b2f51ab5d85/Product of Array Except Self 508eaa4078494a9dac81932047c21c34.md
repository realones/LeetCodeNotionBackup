# Product of Array Except Self

#: 238
Difficult: Medium
Tags: Array, Prefix
link: https://leetcode.com/problems/product-of-array-except-self/
Priority: Medium
Status: More Solution, No Idea At First
Created time: May 30, 2023 9:46 PM
Last edited time: November 17, 2023 1:31 AM
practicedTimes: 1
source: LC_75, LC_Top_Interview_150, ticktokFreq
related: Construct Product Matrix (Construct%20Product%20Matrix%20ce82e79220e14f549d95dfdb98b816f0.md)

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]

```

**Example 2:**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]

```

**Constraints:**

- `2 <= nums.length <= 1e5`
- `30 <= nums[i] <= 30`
- The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

**Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n=nums.size();
        vector<int> res(n,1);
        //l to r
        int tmp=1;
        for(int i=1; i<n; i++){
            tmp*=nums[i-1];
            res[i]*=tmp;
        }
        //r to l
        tmp=1;
        for(int i=n-2; i>=0; i--){
            tmp*=nums[i+1];
            res[i]*=tmp;
        }
        return res;
    }
};
```