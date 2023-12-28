# Squares of a Sorted Array

#: 977
Difficult: Easy
Tags: 2pt_TwoArray, Array, Merge
link: https://leetcode.com/problems/squares-of-a-sorted-array/description/
Priority: Low
Created time: August 17, 2023 2:31 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an integer array `nums` sorted in **non-decreasing** order, return *an array of **the squares of each number** sorted in non-decreasing order*.

**Example 1:**

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].

```

**Example 2:**

```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]

```

**Constraints:**

- `1 <= nums.length <= 104`
- `104 <= nums[i] <= 104`
- `nums` is sorted in **non-decreasing** order.

**Follow up:**

Squaring each element and sorting the new array is very trivial, could you find an

```
O(n)
```

solution using a different approach?

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        for(auto& x: nums) x=pow(x,2);
        auto minItr=min_element(nums.begin(), nums.end());
        vector<int> a1(nums.begin(), minItr);
        reverse(a1.begin(), a1.end());
        vector<int> a2(minItr, nums.end());

        vector<int> res(nums.size());
        int i=0,j=0,k=0;
        while(i<a1.size() && j<a2.size()){
            if(a1[i]<a2[j]) res[k++]=a1[i++];
            else res[k++]=a2[j++];
        }
        while(i<a1.size()) res[k++]=a1[i++];
        while(j<a2.size()) res[k++]=a2[j++];
        return res;
    }
};
/*
option1 - squaring and sort:
O(nlogn)

option 2
T: O(n)
larger ---> smaller ---> larger
reverse left part
merge sort
*/
```