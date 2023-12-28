# Divide Array Into Arrays With Max Difference

#: 2966
Difficult: Medium
Tags: Array, Sliding Window, Sort
link: https://leetcode.com/problems/divide-array-into-arrays-with-max-difference/description/
Priority: Low
Created time: December 16, 2023 9:45 PM
Last edited time: December 16, 2023 9:46 PM
source: LC_Contests

You are given an integer array `nums` of size `n` and a positive integer `k`.

Divide the array into one or more arrays of size `3` satisfying the following conditions:

- **Each** element of `nums` should be in **exactly** one array.
- The difference between **any** two elements in one array is less than or equal to `k`.

Return *a* **2D** *array containing all the arrays. If it is impossible to satisfy the conditions, return an empty array. And if there are multiple answers, return **any** of them.*

**Example 1:**

```
Input: nums = [1,3,4,8,7,9,3,5,1], k = 2
Output: [[1,1,3],[3,4,5],[7,8,9]]
Explanation: We can divide the array into the following arrays: [1,1,3], [3,4,5] and [7,8,9].
The difference between any two elements in each array is less than or equal to 2.
Note that the order of elements is not important.

```

**Example 2:**

```
Input: nums = [1,3,3,2,7,3], k = 3
Output: []
Explanation: It is not possible to divide the array satisfying all the conditions.

```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 105`
- `n` is a multiple of `3`.
- `1 <= nums[i] <= 105`
- `1 <= k <= 105`

```cpp
class Solution {
public:
    vector<vector<int>> divideArray(vector<int>& nums, int k) {
        int n=nums.size();// if(n==0) {return 0;}
        if(n%3!=0) return {};
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        for(int l=0,r ;r=l+2,r<n; l+=3){
            int diff= max({nums[l],nums[l+1],nums[l+2]}) - min({nums[l],nums[l+1],nums[l+2]});
            if(diff>k) return {};
            res.push_back({nums[l],nums[l+1],nums[l+2]});
        }
        return res;
    }
};
/*
divide into multi groups with sz==3
each group
    max diff <=k
================================
n,k: 1e5
================================
validation by sz%3
divide into sz/3 groups

backtracking - TLE
sort and try to group from left to right - nlogn
*/
```