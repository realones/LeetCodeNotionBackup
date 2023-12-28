# [lint] Maximum Number in Mountain Sequence

#: 585
Difficult: Medium
Tags: BS_Idx
link: https://www.lintcode.com/problem/585/
Priority: Low
Created time: July 8, 2022 12:32 PM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: jiuzhang, 算初BS

**Description**

Given a mountain sequence of `n` integers which increase firstly and then decrease, find the mountain top(Maximum).

---

Arrays are strictly incremented, strictly decreasing

**Example**

Example 1:

```
Input: nums = [1, 2, 4, 8, 6, 3]
Output: 8

```

Example 2:

```
Input: nums = [10, 9, 8, 7],
Output: 10

```

```cpp
class Solution {
public:
    /*
     * @param nums: a mountain sequence which increase firstly and then decrease
     * @return: then mountain top
     */
    int mountainSequence(vector<int> &nums) {
        int l=0,r=nums.size()-1;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums[m]<nums[m+1]) l=m+1;
            else r=m;
        }
        return nums[l];
    }
};
```