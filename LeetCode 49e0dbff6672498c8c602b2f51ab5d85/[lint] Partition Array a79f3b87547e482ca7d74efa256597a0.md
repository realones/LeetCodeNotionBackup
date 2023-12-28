# [lint] Partition Array

#: 31
Difficult: Medium
Tags: 2pt_DiffDir, Partition
link: https://www.lintcode.com/problem/31/
Priority: Low
Status: Dig More
Created time: August 8, 2022 11:20 PM
Last edited time: October 19, 2023 9:30 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers

**Description**

Given an array `nums` of integers and an int `k`, partition the array (i.e move the elements in "nums") such that:

- All elements < *k* are moved to the *left*
- All elements >= *k* are moved to the *right*

Return the partitioning index, i.e the first index *i* nums[*i*] >= *k*.

---

# 

If all elements in `nums` are smaller than `k`, then return `nums.length`0 <= nums.length <= 20000<=*nums*.*length*<=2000

**Example**

**Example 1:**

Input:

```
nums = []
k = 9

```

Output:

```
0

```

Explanation:

Empty array, print 0.

**Example 2:**

Input:

```
nums = [3,2,2,1]
k = 2

```

Output:

```
1

```

Explanation:

the real array is[1,2,2,3].So return 1.

**Challenge**

Can you partition the array in-place and in O(n)*O*(*n*)?

pay attention to nums[r]≥k, in this case, always end with `….r,l…..`

```cpp
class Solution {
public:
    /**
     * @param nums: The integer array you should partition
     * @param k: An integer
     * @return: The index after partition
     */
    int partitionArray(vector<int> &nums, int k) {
        // write your code here
        int n=nums.size(); if(n==0) {return 0;}
        int l=0,r=n-1;
        while(l<=r){
            while(l<=r && nums[l]<k) l++;
            while(l<=r && nums[r]>**=**k) r--;
            if(l<=r){
                swap(nums[l++],nums[r--]);
            }
        }
        return l;
    }
};
```