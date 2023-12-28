# [lint] Subarray Sum

#: 138
Difficult: Easy
Tags: Array, Contribution==, Hash, Prefix
link: https://www.lintcode.com/problem/138/
Priority: Low
Created time: August 3, 2022 1:37 PM
Last edited time: October 17, 2023 6:45 PM
practicedTimes: 1
source: jiuzhang, 算初Hash&Heap, 算初LinkedList&Array, 算高FollowUp

**Description**

Given an integer array, find a subarray where the sum of numbers is **zero**. Your code should return the index of the first number and the index of the last number.

There is at least one subarray that it's sum equals to zero.

**Example**

**Example 1:**

```
Input:  [-3, 1, 2, -3, 4]
Output: [0, 2] or [1, 3].
Explanation: return anyone that the sum is 0.

```

**Example 2:**

```
Input:  [-3, 1, -4, 2, -3, 4]
Output: [1,5]

```

map[presum,idx], 想象平面坐标系，一条线代表了prefixSum的变化，如果线往前走的时候，两个点高度一样了，就说明找到了。

prefixSum中如果有两个一样的数，就找到了

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number and the index of the last number
     */
    vector<int> subarraySum(vector<int> &nums) {
        // write your code here
        int n=nums.size(); if(n==0) {return {-1,-1};}
        unordered_map<int,int> mp;
        int sum=0;
        mp[0]=-1;
        for(int i=0; i<n; i++){
            sum+=nums[i];
            if(mp.count(sum)){
                return {mp[sum]+1, i};
            }
            mp[sum]=i;
        }
        return {-1,-1};
    }
};
```