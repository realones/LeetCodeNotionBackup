# Valid Triangle Number

#: 611
Difficult: Medium
Tags: 2pt_DiffDir, Greedy, Math
link: https://leetcode.com/problems/valid-triangle-number/
Priority: High
Created time: August 6, 2022 11:59 PM
Last edited time: October 19, 2023 10:30 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初TwoPointers

Given an integer array `nums`, return *the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle*.

**Example 1:**

```
Input: nums = [2,2,3,4]
Output: 3
Explanation: Valid combinations are:
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3

```

**Example 2:**

```
Input: nums = [4,2,3,4]
Output: 4

```

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`

Solution:

sort

a+b>c

for each c

2pt for a,b

```cpp
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int n=nums.size(); if(n<3) {return 0;}
        sort(nums.begin(), nums.end());
        int res=0;
        for(int r=2;r<n;r++){
            int l=0, m=r-1;
            while(l<m){
                //invalid: l++
                if(nums[l]+nums[m]<=nums[r]){l++;}
                //valid:  res+=m-l?,m--; 
                else{
                    res+=m-l;
                    m--;
                }
            }
        }
        return res;
    }
};
```