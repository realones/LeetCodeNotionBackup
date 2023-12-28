# Maximum Strength of a Group

#: 2708
Difficult: Medium
Tags: dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/maximum-strength-of-a-group/
Priority: Medium
Created time: May 28, 2023 9:49 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** integer array `nums` representing the score of students in an exam. The teacher would like to form one **non-empty** group of students with maximal **strength**, where the strength of a group of students of indices `i0`, `i1`, `i2`, ... , `ik` is defined as `nums[i0] * nums[i1] * nums[i2] * ... * nums[ik]`.

Return *the maximum strength of a group the teacher can create*.

**Example 1:**

```
Input: nums = [3,-1,-5,2,5,-9]
Output: 1350
Explanation: One way to form a group of maximal strength is to group the students at indices [0,2,3,4,5]. Their strength is 3 * (-5) * 2 * 5 * (-9) = 1350, which we can show is optimal.

```

**Example 2:**

```
Input: nums = [-4,-5,-4]
Output: 20
Explanation: Group the students at indices [0, 1] . Then, we’ll have a resulting strength of 20. We cannot achieve greater strength.

```

**Constraints:**

- `1 <= nums.length <= 13`
- `9 <= nums[i] <= 9`

Solution:

```cpp
typedef long long ll;
class Solution {
public:
    long long maxStrength(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}    
        ll maxv=1, minv=1;
        for(auto num:nums){
            ll nxtMaxv=max({maxv*num,minv*num,maxv,minv});
            ll nxtMinv=min({maxv*num,minv*num,maxv,minv});
            maxv=nxtMaxv;
            minv=nxtMinv;
        }
        
        ll maxElement=*max_element(nums.begin(),nums.end());
        if(maxv==1 && maxElement<=0){return maxElement;}

        return maxv;
    }
};

/*
find subsequence/subset(sz>=1) that multiply res is maximum

for each num

use it, not use it

maxv = max of (maxv or minv * cur, maxv or minv)
minv = min of (maxv or minv * cur, maxv or minv)

combination

state machine

*/
```