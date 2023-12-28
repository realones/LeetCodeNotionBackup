# Sum of Absolute Differences in a Sorted Array

#: 1685
Difficult: Medium
Tags: Array, Math, Prefix
link: https://leetcode.com/problems/sum-of-absolute-differences-in-a-sorted-array/
Priority: Low
Created time: November 27, 2023 12:18 AM
Last edited time: November 27, 2023 12:18 AM
source: leetcode_daily

You are given an integer array `nums` sorted in **non-decreasing** order.

Build and return *an integer array* `result` *with the same length as* `nums` *such that* `result[i]` *is equal to the **summation of absolute differences** between* `nums[i]` *and all the other elements in the array.*

In other words, `result[i]` is equal to `sum(|nums[i]-nums[j]|)` where `0 <= j < nums.length` and `j != i` (**0-indexed**).

**Example 1:**

```
Input: nums = [2,3,5]
Output: [4,3,5]
Explanation: Assuming the arrays are 0-indexed, then
result[0] = |2-2| + |2-3| + |2-5| = 0 + 1 + 3 = 4,
result[1] = |3-2| + |3-3| + |3-5| = 1 + 0 + 2 = 3,
result[2] = |5-2| + |5-3| + |5-5| = 3 + 2 + 0 = 5.

```

**Example 2:**

```
Input: nums = [1,4,6,8,10]
Output: [24,15,13,15,21]

```

**Constraints:**

- `2 <= nums.length <= 105`
- `1 <= nums[i] <= nums[i + 1] <= 104`

```cpp
class Solution {
public:
    vector<int> getSumAbsoluteDifferences(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}

        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }

        vector<int> suffixSum(nums);
        for(int i=n-2;i>=0;i--){
            suffixSum[i]+=suffixSum[i+1];
        }

        vector<int> res(n);
        for(int i=0; i<n; i++){
            int left=abs(nums[i]*(i) - ((i-1<0)?0:prefixSum[i-1]));//left
            int right=abs(nums[i]*(n-i-1) - ((i+1>n-1)?0:suffixSum[i+1]));//right
            res[i]=left+right;
        }
        return res;
    }
};

/*
nums:
    len: 2~1e5
    val: 1~1e4
================================
nums: incOrEq
res[i] = sum(|nums[i]-nums[j]|), j:0~n-1, j!=i
================================

res[i] = sum(|nums[i]-nums[j]|), j:0~n-1, j!=i
sort and maintain the idx mapping- not required, already sorted
res[i] = 
    abs: cur*(lenOfRight) - suffixSum(right)
    +
    abs: cur*(lenOfLeft) - prefixSum(left)
*/
```