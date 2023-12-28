# Missing Element in Sorted Array

#: 1060
Difficult: Medium
Tags: Array, BS_Idx
link: https://leetcode.com/problems/missing-element-in-sorted-array/description/
Priority: High
Created time: July 31, 2023 12:08 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an integer array `nums` which is sorted in **ascending order** and all of its elements are **unique** and given also an integer `k`, return the `kth` missing number starting from the leftmost number of the array.

**Example 1:**

```
Input: nums = [4,7,9,10], k = 1
Output: 5
Explanation: The first missing number is 5.

```

**Example 2:**

```
Input: nums = [4,7,9,10], k = 3
Output: 8
Explanation: The missing numbers are [5,6,8,...], hence the third missing number is 8.

```

**Example 3:**

```
Input: nums = [1,2,4], k = 3
Output: 6
Explanation: The missing numbers are [3,5,6,7,...], hence the third missing number is 6.

```

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `1 <= nums[i] <= 107`
- `nums` is sorted in **ascending order,** and all the elements are **unique**.
- `1 <= k <= 108`

**Follow up:**

Can you find a logarithmic time complexity (i.e.,

```
O(log(n))
```

) solution?

```cpp
class Solution {
public:
    int missingElement(vector<int>& nums, int k) {
        int n=nums.size();
        int l=0,r=n-1;
        //find first ele support enough missing element
        while(l<r){
            int m=l+(r-l)/2;
            //invalid: supportMissingCnt<k
            if(supportMissingCnt(nums, m) < k) l=m+1;
            else r=m;
        }
        //if l supported missing >= k -> l-1
        //el from l
        if(supportMissingCnt(nums, l) >= k) {
            return nums[l-1] + k-supportMissingCnt(nums, l-1);
        }
        return nums[l] + k-supportMissingCnt(nums, l);
    }

    //how many missing elements supported at most on the left of idx
    int supportMissingCnt(vector<int>& nums, int idx){
        return nums[idx] - nums.front() - idx;
    }
};

/*
len: 1~5e4
val: 1~1e7
k:   1~1e8

missing number starting from the leftmost number of the array

for each num: num.front(), +1, +2, .... +k
    if in nums

to speed up:
    BS, first check nums.mid, how many missing number on its left, then decide go left or right

[4,7,9,10], k = 3
l=4, r=10
find mid: 7
    which support 7-1-4 = 2 missing elements

    let l=9
    mid=9
    which support 9-2-4 = 3 missing elements

    so it is between 7 and 9

O(log(n))

*/
```