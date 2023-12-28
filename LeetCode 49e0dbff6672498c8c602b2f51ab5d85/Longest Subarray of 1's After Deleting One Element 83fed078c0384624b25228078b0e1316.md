# Longest Subarray of 1's After Deleting One Element

#: 1493
Difficult: Medium
Tags: Array, Sliding Window, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element
Priority: Medium
Status: More Solution
Created time: July 4, 2023 5:32 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75, leetcode_daily

Given a binary array `nums`, you should delete one element from it.

Return *the size of the longest non-empty subarray containing only* `1`*'s in the resulting array*. Return `0` if there is no such subarray.

**Example 1:**

```
Input: nums = [1,1,0,1]
Output: 3
Explanation: After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.

```

**Example 2:**

```
Input: nums = [0,1,1,1,0,1,1,0,1]
Output: 5
Explanation: After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].

```

**Example 3:**

```
Input: nums = [1,1,1]
Output: 2
Explanation: You must delete one element.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.

Solution state machine: 

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}
        int nf=0,f=-1;
        int res=0;
        for(auto x: nums){
            int nxt_nf, nxt_f;
            if(x==0){
                nxt_nf=0;
                nxt_f=nf+1;
            }
            else{
                nxt_nf=nf+1;
                nxt_f=f+1;
            }
            nf=nxt_nf, f=nxt_f;
            res=max({res,f,nf});
        }
        return res-1;
    }
};

//similar to question flipping one
/*.              1        0
not_flipped.  nf+1        0
flipped        f+1        nf+1

rtn: max(nf,f)-1
*
```