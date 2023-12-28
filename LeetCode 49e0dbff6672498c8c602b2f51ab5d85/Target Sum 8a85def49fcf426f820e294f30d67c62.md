# Target Sum

#: 494
Difficult: Medium
Tags: dp-backpack
link: https://leetcode.com/problems/target-sum/
Priority: High
Status: Dig More
Created time: December 31, 2022 4:55 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路
related: Last Stone Weight II (Last%20Stone%20Weight%20II%20d176e566332940948976aaca36fd3d1a.md)

You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

**Example 1:**

```
Input: nums = [1,1,1,1,1], target = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3

```

**Example 2:**

```
Input: nums = [1], target = 1
Output: 1

```

**Constraints:**

- `1 <= nums.length <= 20`
- `0 <= nums[i] <= 1000`
- `0 <= sum(nums[i]) <= 1000`
- `1000 <= target <= 1000`

solution 2: backpack

```jsx
/*
backpack dp w/ offset
can use package dp because constraits of the range is small
*/

class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int n=nums.size(); if(n==0) {return 0;}
        int sum=accumulate(nums.begin(),nums.end(),0);
        if(target<-sum || target>sum) return 0;//no abs required, since all positive
        int offset=sum;
        nums.insert(nums.begin(),0);//so curVal won't be nums[i-1]
        
        vector<vector<int>> dp(n+1,vector<int>(sum+1+offset,0));
        dp[0][0+offset]=1;
        
        for(int i=1; i<n+1; i++){
            int curVal=nums[i];
            for(int s=-sum; s<=sum; s++){//+sum since idx can't be negtive
                dp[i][s+offset] = 
                    ((-sum<=s-curVal && s-curVal<=sum) ? dp[i-1][s-curVal+offset] : 0)
                    + 
                    ((-sum<=s+curVal && s+curVal<=sum) ? dp[i-1][s+curVal+offset] : 0)
                    ;
            }
        }
        
        return dp[n][target+offset];
    }
};
```

solution1: recursive handle each element - 记忆化搜索

```cpp
class Solution {
public:
    unordered_map<int,unordered_map<int,int>> dp;
    int findTargetSumWays(vector<int>& nums, int target) {
        int n=nums.size();
        target=abs(target);
        return helper(nums,0,target);
    }
    
    int helper(vector<int>& nums, int start, int target){
        if(start==nums.size()) {
            if(target==0) return 1;
            else return 0; 
        }
        
        if(dp[start].count(target)) return dp[start][target];
        
        dp[start][target] = helper(nums,start+1,target+nums[start]) + helper(nums,start+1,target-nums[start]);
        return dp[start][target];
    }
};
```

Solution 3: backpack - mp version

backpack seems like bfs version of 1 by 1 element

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int n=nums.size();
        int total=accumulate(nums.begin(), nums.end(),0);
        if(target<-total || target>total) return 0;

        vector<unordered_map<int,int>> dp(n+1);
        dp[0][0]=1;

        for(int i=1;i<n+1;i++){
            int cur=nums[i-1];
            for(int j=-total;j<=total;j++){
                dp[i][j]=
                    ((j-cur<-total || j-cur>total)? 0: dp[i-1][j-cur])
                    +
                    ((j+cur<-total || j+cur>total)? 0: dp[i-1][j+cur])
                    ;
            }
        }
        return dp[n][target];
    }
};
/*
dp[first ith elem: 0~n][accumulate: -total~total]
init:
dp[0][0]=1, other=0

dp[i][j]+=
        pre+cur: dp[i-1][j'], j'+cur=j
        pre-cur: dp[i-1][j'], j'-cur=j

return: dp[n][target]
*/
```