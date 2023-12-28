# Combination Sum IV

#: 377
Difficult: Medium
Tags: Array, DP, recursion
link: https://leetcode.com/problems/combination-sum-iv/
Priority: Medium
Status: More Solution
Created time: June 28, 2023 12:51 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an array of **distinct** integers `nums` and a target integer `target`, return *the number of possible combinations that add up to* `target`.

The test cases are generated so that the answer can fit in a **32-bit** integer.

**Example 1:**

```
Input: nums = [1,2,3], target = 4
Output: 7
Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.

```

**Example 2:**

```
Input: nums = [9], target = 3
Output: 0

```

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 1000`
- All the elements of `nums` are **unique**.
- `1 <= target <= 1000`

**Follow up:** What if negative numbers are allowed in the given array? How does it change the problem? What limitation we need to add to the question to allow negative numbers?

comment: find other solutions

```cpp
class Solution {
public:
    //len:1~200, val: 1~1000, unique, target:1~1000
    int combinationSum4(vector<int>& nums, int target) {
        int n=nums.size();
        vector<int> dp(target+1,-1);//from mp
        function<int(int)> f = [&](int t){
            if(t<0) return 0;
            if(t==0) return 1;
            if(dp[t]!=-1) return dp[t];
            dp[t]=0;
            for(auto num: nums){
                dp[t]+=f(t-num);
            }
            return dp[t];
        };

        return f(target);
    }
}
```