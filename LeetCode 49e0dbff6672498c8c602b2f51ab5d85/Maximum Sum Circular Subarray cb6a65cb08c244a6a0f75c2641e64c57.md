# Maximum Sum Circular Subarray

#: 918
Difficult: Medium
Tags: Contribution>0, DP, Kadane, Prefix
link: https://leetcode.com/problems/maximum-sum-circular-subarray/
Priority: Medium
Created time: May 11, 2023 5:23 PM
Last edited time: November 1, 2023 8:19 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算高FollowUp

Given a **circular integer array** `nums` of length `n`, return *the maximum possible sum of a non-empty **subarray** of* `nums`.

A **circular array** means the end of the array connects to the beginning of the array. Formally, the next element of `nums[i]` is `nums[(i + 1) % n]` and the previous element of `nums[i]` is `nums[(i - 1 + n) % n]`.

A **subarray** may only include each element of the fixed buffer `nums` at most once. Formally, for a subarray `nums[i], nums[i + 1], ..., nums[j]`, there does not exist `i <= k1`, `k2 <= j` with `k1 % n == k2 % n`.

**Example 1:**

```
Input: nums = [1,-2,3,-2]
Output: 3
Explanation: Subarray [3] has maximum sum 3.

```

**Example 2:**

```
Input: nums = [5,-3,5]
Output: 10
Explanation: Subarray [5,5] has maximum sum 5 + 5 = 10.

```

**Example 3:**

```
Input: nums = [-3,-2,-3]
Output: -2
Explanation: Subarray [-2] has maximum sum -2.

```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 3 * 104`
- `3 * 104 <= nums[i] <= 3 * 104`

**Solution**:

circular-取反

```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}
        
        //solution 1: without circular
        int res1=maxSum(nums);
        
        //solution 2: one part in left, another part in right
        //if all neg: use solution 1
        int totalSum=accumulate(nums.begin(), nums.end(), 0);
        
        for(auto& num: nums) num=-num;
        int sum2=maxSum(nums);
        if(-sum2==totalSum) return res1;
            
        int res2=totalSum+sum2;
        return max(res1,res2);
    }
    
    int maxSum(vector<int>& nums){
        int tmp=0,sum=INT_MIN;
        for(auto num: nums){
            tmp+=num;
            sum=max(sum,tmp);
            tmp=max(0,tmp);
        }
        return sum;
    }
};
```