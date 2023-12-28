# Range Sum Query

#: 303
Difficult: Easy
Tags: Array, Design, Prefix
link: https://leetcode.com/problems/range-sum-query-immutable/
Priority: Pass
Created time: June 28, 2023 12:56 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an integer array `nums`, handle multiple queries of the following type:

1. Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

**Example 1:**

```
Input
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3

```

**Constraints:**

- `1 <= nums.length <= 104`
- `105 <= nums[i] <= 105`
- `0 <= left <= right < nums.length`
- At most `104` calls will be made to `sumRange`.

```cpp
class NumArray {
public:
    vector<int> prefixSum;

    NumArray(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        prefixSum=nums;
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
    }
    
    int sumRange(int l, int r) {
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }

};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(left,right);
 *
```