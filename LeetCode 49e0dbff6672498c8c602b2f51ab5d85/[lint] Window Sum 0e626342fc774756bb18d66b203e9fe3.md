# [lint] Window Sum

#: 604
Difficult: Easy
Tags: Sliding Window, changeAffectIteration
link: https://www.lintcode.com/problem/window-sum
Priority: Pass
Created time: August 3, 2022 1:37 PM
Last edited time: October 19, 2023 6:13 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers

Given an array of n integers, and a moving window(size k), move the window at each iteration from the start of the array, find the `sum` of the element inside the window at each moving.

Example:
**Example 1**

```
Input：array = [1,2,7,8,5], k = 3
Output：[10,17,20]
Explanation：
1 + 2 + 7 = 10
2 + 7 + 8 = 17
7 + 8 + 5 = 20

```

Challenge

Related Questions:
sliding-window-median,moving-average-from-data-stream,sliding-window-maximum,minimum-window-substring

```cpp
class Solution {
public:
    /**
     * @param nums: a list of integers.
     * @param k: length of window.
     * @return: the sum of the element inside the window at each moving.
     */
    vector<int> winSum(vector<int> &nums, int k) {
        // write your code here
        int n=nums.size(); if(n==0) {return {};}
        if(k<1 || k>n) return {};
        vector<int> res;
        int sum=0;
        for(int i=0; i<n; i++){
            //add new val
            sum+=nums[i];
            //rm tail value -[i-k]
            if(i>k-1) sum-=nums[i-k];
            //push to res
            if(i>=k-1) res.push_back(sum);
        }
        return res;
    }
};
```