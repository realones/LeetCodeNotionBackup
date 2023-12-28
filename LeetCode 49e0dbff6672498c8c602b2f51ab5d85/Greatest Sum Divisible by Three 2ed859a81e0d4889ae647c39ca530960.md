# Greatest Sum Divisible by Three

#: 1262
Difficult: Medium
Tags: Array, DP, Greedy, Math, OrderMap, Sort
link: https://leetcode.com/problems/greatest-sum-divisible-by-three/
Priority: Low
Status: More Solution
Created time: September 24, 2023 10:17 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an integer array `nums`, return *the **maximum possible sum** of elements of the array such that it is divisible by three*.

**Example 1:**

```
Input: nums = [3,6,5,1,8]
Output: 18
Explanation: Pick numbers 3, 6, 1 and 8 their sum is 18 (maximum sum divisible by 3).
```

**Example 2:**

```
Input: nums = [4]
Output: 0
Explanation: Since 4 is not divisible by 3, do not pick any number.

```

**Example 3:**

```
Input: nums = [1,2,3,4,4]
Output: 12
Explanation: Pick numbers 1, 3, 4 and 4 their sum is 12 (maximum sum divisible by 3).

```

**Constraints:**

- `1 <= nums.length <= 4 * 104`
- `1 <= nums[i] <= 104`

Solution math:

```cpp
class Solution {
public:
    int maxSumDivThree(vector<int>& nums) {
        int total=accumulate(nums.begin(), nums.end(),0);
        int remain = total%3;
        if(remain==0) return total;
        multiset<int> st1;//remain 1
        multiset<int> st2;//remain 2
        for(auto x: nums){
            if(x%3==1) st1.insert(x);
            else if(x%3==2) st2.insert(x);
        }
        if(remain==1){
            int op1=st1.size()>=1? total-*st1.begin(): 0;
            int op2=st2.size()>=2? total-*st2.begin()-*next(st2.begin()): 0;
            return max(op1,op2);
        }
        else{//if(remain==2){
            int op1=st2.size()>=1? total-*st2.begin(): 0;
            int op2=st1.size()>=2? total-*st1.begin()-*next(st1.begin()) :0;
            return max(op1,op2);
        }
    }
}
```