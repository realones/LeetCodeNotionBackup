# Power of Heroes

#: 2681
Difficult: Hard
Tags: Array, Math, Prefix, Sort
link: https://leetcode.com/problems/power-of-heroes/
Priority: High
Status: No Idea At First
Created time: June 16, 2023 5:36 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** integer array `nums` representing the strength of some heroes. The **power** of a group of heroes is defined as follows:

- Let `i0`, `i1`, ... ,`ik` be the indices of the heroes in a group. Then, the power of this group is `max(nums[i0], nums[i1], ... ,nums[ik])2 * min(nums[i0], nums[i1], ... ,nums[ik])`.

Return *the sum of the **power** of all **non-empty** groups of heroes possible.* Since the sum could be very large, return it **modulo** `109 + 7`.

**Example 1:**

```
Input: nums = [2,1,4]
Output: 141
Explanation:
1st group: [2] has power = 22 * 2 = 8.
2nd group: [1] has power = 12 * 1 = 1.
3rd group: [4] has power = 42 * 4 = 64.
4th group: [2,1] has power = 22 * 1 = 4.
5th group: [2,4] has power = 42 * 2 = 32.
6th group: [1,4] has power = 42 * 1 = 16.
7th group: [2,1,4] has power = 42 * 1 = 16.
The sum of powers of all groups is 8 + 1 + 64 + 4 + 32 + 16 + 16 = 141.

```

**Example 2:**

```
Input: nums = [1,1,1]
Output: 7
Explanation: A total of 7 groups are possible, and the power of each group will be 1. Therefore, the sum of the powers of all groups is 7.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`

```cpp
using ll=long long;
using ii=pair<int,int>;
ll M=1e9+7;

using ll=long long;

class Solution {
public:
    //sum of subsets minEle*maxEle^2
    int sumOfPower(vector<int>& nums) {//len: 1e5
        sort(nums.begin(), nums.end());
        ll res=0;
        ll sum=0;
        for(ll num: nums){
            res+=(num*num%M) * (sum+num) %M;
            sum= (sum*2 + num)%M;
        }
        return (res+M)%M;
    }
};

/*
sort nums - O nlogn
for idx 0 -> n-1
    sum of e.g.
    [0] is minEle, which has 2^(n-1) subsets for right side,
    on right side:
        sum of e.g.
            [n-1] is max Ele, which has 2^(n-2) subset in the mid of [0] and [n-1]
            [n-2] is max Ele, which has 2^(n-3) subset in the mid of [0] and [n-2]
            ...
res=[0] * (2^(n-2)*[n-1]^2 + 2^(n-3)*[n-2]^2+ ... + 2^0*[0]^2)
    +
    [1]*...
    +
    ...
    +
    [n-1]*...
================================
option1:
for l: -->. 0..n-1
    for r:  <-- n-1..l

option2:
for l: -->
    for r: -->

option3: (use this one), since it can help reuse previous result
for r: -->
    for l: -->

optionX:
...
================================

[X Y Z] i X X X X
nums[i]^2 * (sum of
Z * 2^0
Y * 2^1
X * 2^2
+ nums[i])

[X Y Z W] i X X X
nums[i]^2 * (sum of
W * 2^0
Z * 2^1
Y * 2^2
X * 2^3
+ nums[i])

sum = sum * 2 + nums[i-1]
res += nums[i]^2 * (sum+nums[i])
*/
```