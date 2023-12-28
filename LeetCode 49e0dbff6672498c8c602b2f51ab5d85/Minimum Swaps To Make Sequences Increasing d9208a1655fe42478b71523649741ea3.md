# Minimum Swaps To Make Sequences Increasing

#: 801
Difficult: Hard
Tags: Array, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/description/
Priority: Medium
Created time: June 28, 2023 12:11 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given two integer arrays of the same length `nums1` and `nums2`. In one operation, you are allowed to swap `nums1[i]` with `nums2[i]`.

- For example, if `nums1 = [1,2,3,8]`, and `nums2 = [5,6,7,4]`, you can swap the element at `i = 3` to obtain `nums1 = [1,2,3,4]` and `nums2 = [5,6,7,8]`.

Return *the minimum number of needed operations to make* `nums1` *and* `nums2` ***strictly increasing***. The test cases are generated so that the given input always makes it possible.

An array `arr` is **strictly increasing** if and only if `arr[0] < arr[1] < arr[2] < ... < arr[arr.length - 1]`.

**Example 1:**

```
Input: nums1 = [1,3,5,4], nums2 = [1,2,3,7]
Output: 1
Explanation:
Swap nums1[3] and nums2[3]. Then the sequences are:
nums1 = [1, 3, 5, 7] and nums2 = [1, 2, 3, 4]
which are both strictly increasing.

```

**Example 2:**

```
Input: nums1 = [0,3,5,8,9], nums2 = [2,1,4,6,9]
Output: 1

```

**Constraints:**

- `2 <= nums1.length <= 105`
- `nums2.length == nums1.length`
- `0 <= nums1[i], nums2[i] <= 2 * 105`

```cpp
class Solution {
public:
    int minSwap(vector<int>& nums1, vector<int>& nums2) {//same len: 2~1e5, val: 0~2e5
        int n=nums1.size();
        vector<vector<int>> dp(n,vector<int>(2,INT_MAX));
        dp[0][0]=0, dp[0][1]=1;
        function valid = [&](int i, bool preSwap, bool curSwap)->bool{
            int p1=nums1[i-1], c1=nums1[i];
            int p2=nums2[i-1], c2=nums2[i];
            if(preSwap) swap(p1,p2);
            if(curSwap) swap(c1,c2);
            return p1<c1 && p2<c2;
        };
        for(int i=1; i<n; i++){
            dp[i][0]= min(valid(i,false,false)?dp[i-1][0]:INT_MAX, valid(i,true,false)?dp[i-1][1]:INT_MAX);
            dp[i][1]= min(valid(i,false,true)?dp[i-1][0]:INT_MAX, valid(i,true,true)?dp[i-1][1]:INT_MAX)+1;
        }
        return min(dp.back()[0], dp.back()[1]);
    }
};

/*
swap nums1[i], nums2[i]
purpose: make both arr inc
return: min step of swap
validity guaranteed
================================

0: not_swap, 1: swap
f(i,not_swap): from 0~i, both inc, min op steps
f(i,swap): from 0~i, both inc, min op steps

f(i,not_swap)= f(i)(any can make inc with out swap) + 0
f(i,swap)= f(i)(any can make inc with out swap) + 1

init:
    f(0,not_swap)=0
    f(0,swap)=1

return:
    f.back
*/
```