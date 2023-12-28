# [lint] Total Occurrence of Target

#: 462
Difficult: Easy
Tags: BS_Idx
link: https://www.lintcode.com/problem/total-occurrence-of-target/description
Priority: Pass
Created time: July 6, 2022 11:37 AM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: jiuzhang, 算初BS

Given a target number and an integer array sorted in ascending order. Find the total number of occurrences of target in the array.

Example:
Example1:

```
Input: [1, 3, 3, 4, 5] and target = 3,
Output: 2.

```

Example2:

```
Input: [2, 2, 3, 4, 6] and target = 4,
Output: 1.

```

Example3:

```
Input: [1, 2, 3, 4, 5] and target = 6,
Output: 0.

```

Challenge
Time complexity in O(logn)

Related Questions:
search-for-a-range

```cpp
class Solution {
public:
    /**
     * @param A: A an integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */
    int totalOccurrence(vector<int> &nums, int target) {
        int n=nums.size(); if(n==0) {return 0;}
        int left,right;
        int l,r;
        l=0,r=n-1;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums[m]<target){l=m+1;}
            else{r=m;}
        }
        if(nums[l]==target){left=l;}
        else{return 0;}
        l=0,r=n-1;
        while(l<r){
            int m=l+((long)r-l+1)/2;
            if(nums[m]>target){r=m-1;}
            else{l=m;}
        }
        right=r;
        return right-left+1;
    }
};
```