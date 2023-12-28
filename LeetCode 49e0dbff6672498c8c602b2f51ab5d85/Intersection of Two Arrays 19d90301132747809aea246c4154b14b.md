# Intersection of Two Arrays

#: 349
Difficult: Easy
Tags: Array
link: https://leetcode.com/problems/intersection-of-two-arrays/
Priority: Pass
Created time: August 3, 2022 1:29 PM
Last edited time: October 16, 2023 5:00 PM
practicedTimes: 1
source: jiuzhang, 算初LinkedList&Array

Given two integer arrays `nums1` and `nums2`, return *an array of their intersection*. Each element in the result must be **unique** and you may return the result in **any order**.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]

```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.

```

**Constraints:**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        //solution 1.sort, 2.hash 3.BS
        unordered_set<int> st1(nums1.begin(),nums1.end());
        unordered_set<int> st2(nums2.begin(),nums2.end());
        vector<int> res;
        for(auto num:st1){
            if(st2.count(num)) res.push_back(num);
        }
        
        return res;
    }
};
```