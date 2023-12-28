# Ways to Split Array Into Good Subarrays

#: 2750
Difficult: Medium
Tags: Array, Math
link: https://leetcode.com/problems/minimum-operations-to-make-the-integer-zero/
Priority: Medium
Status: HardToImplement
Created time: June 24, 2023 9:38 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a binary array `nums`.

A subarray of an array is **good** if it contains **exactly** **one** element with the value `1`.

Return *an integer denoting the number of ways to split the array* `nums` *into **good** subarrays*. As the number may be too large, return it **modulo** `109 + 7`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

```
Input: nums = [0,1,0,0,1]
Output: 3
Explanation: There are 3 ways to split nums into good subarrays:
- [0,1] [0,0,1]
- [0,1,0] [0,1]
- [0,1,0,0] [1]

```

**Example 2:**

```
Input: nums = [0,1,0]
Output: 1
Explanation: There is 1 way to split nums into good subarrays:
- [0,1,0]

```

**Constraints:**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 1`

Solution:

idea is straightforward but some corner cases so a bit hard to implement

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int numberOfGoodSubarraySplits(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}
        int oneCnt=0;
        for(auto num: nums){
            if(num==1) oneCnt++;
            if(oneCnt>1) break;
        }
        std::cout << "oneCnt" << " -- " << oneCnt << std::endl;
        if(oneCnt==0) return 0;
        if(oneCnt==1) return 1;
        
        ll res=1;
        int last1Idx=-1;
        for(int i=0; i<n; i++){
            if(nums[i]==1){
                if(last1Idx!=-1){
                    int k=i-last1Idx;//len of 0s between two 1s
                    std::cout << "k" << " -- " << k << std::endl;
                    
                    res=(res*(k))%M;
                }
                last1Idx=i;
            }
        }
        
        return (res+M) % M;
    }
}
```