# Minimize the Maximum Difference of Pairs

#: 2616
Difficult: Medium
Tags: Array, BS_Val, Greedy, Sort
link: https://leetcode.com/problems/minimize-the-maximum-difference-of-pairs/description/
Priority: High
Status: No Idea At First, NotUnderstand
Created time: August 8, 2023 9:05 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given a **0-indexed** integer array `nums` and an integer `p`. Find `p` pairs of indices of `nums` such that the **maximum** difference amongst all the pairs is **minimized**. Also, ensure no index appears more than once amongst the `p` pairs.

Note that for a pair of elements at the index `i` and `j`, the difference of this pair is `|nums[i] - nums[j]|`, where `|x|` represents the **absolute** **value** of `x`.

Return *the **minimum** **maximum** difference among all* `p` *pairs.* We define the maximum of an empty set to be zero.

**Example 1:**

```
Input: nums = [10,1,2,7,1,3], p = 2
Output: 1
Explanation: The first pair is formed from the indices 1 and 4, and the second pair is formed from the indices 2 and 5.
The maximum difference is max(|nums[1] - nums[4]|, |nums[2] - nums[5]|) = max(0, 1) = 1. Therefore, we return 1.

```

**Example 2:**

```
Input: nums = [4,2,1,2], p = 1
Output: 0
Explanation: Let the indices 1 and 3 form a pair. The difference of that pair is |2 - 2| = 0, which is the minimum we can attain.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 109`
- `0 <= p <= (nums.length)/2`

todo: prove the part marked in following comment

```cpp
class Solution {
public: 
    int minimizeMax(vector<int>& nums, int p) {
        int n=nums.size();//1~1e5, val: 0~1e9
        if(p==0) return 0; // 0~nums.len/2
        sort(nums.begin(), nums.end());
        int l=0,r=nums.back()-nums.front();
        while(l<r){
            int m=l+(r-l)/2;
            if(!valid(nums,p,m)) {l=m+1;}//if not qualify
            else {r=m;}
        }
        return l;
    }

    bool valid(vector<int>& nums, int p, int maxDiff){
        int n=nums.size();
        int cnt=0;
        for(int i=0; i<n-1;){
            int diff=nums[i+1]-nums[i];
            if(diff<=maxDiff){
                cnt++;
                i+=2;
            }
            else{i++;}
        }
        return cnt>=p;
    }
};

/*
find p pairs
find the min of (max pair diff)
================================
bs_val
sort
l: 0, r: back-front
valid(maxDiff): return if can find >= p pairs has diff <= maxDiff
    if previous valid diff will affect following ones?
        yes: "ensure no index appears more than once amongst the p pairs."
    for i: 0~n-2 -> diff=[i+1]-[i]
        if diff is valid i+=2
        else i++
    this is greedy solution, why it works?
        to be proved
    return actualCnt>=p
================================
2p items
my thought 1 - no in the implementation:
sort -> try to find max gap of pairs in the sliding window, size is 2p
maintain a fixed size sliding window
    for first x items to build the window: main dec stack, max is alwasy the largest(left) item in stk
do twice for odd and even since not sharing the idx
*/
```