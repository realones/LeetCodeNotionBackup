# Minimum Difficulty of a Job Schedule

#: 1335
Difficult: Hard
Tags: dp-K_Ranges
link: https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/
Priority: Medium
Created time: December 11, 2022 3:03 PM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路

You want to schedule a list of jobs in `d` days. Jobs are dependent (i.e To work on the `ith` job, you have to finish all the jobs `j` where `0 <= j < i`).

You have to finish **at least** one task every day. The difficulty of a job schedule is the sum of difficulties of each day of the `d` days. The difficulty of a day is the maximum difficulty of a job done on that day.

You are given an integer array `jobDifficulty` and an integer `d`. The difficulty of the `ith` job is `jobDifficulty[i]`.

Return *the minimum difficulty of a job schedule*. If you cannot find a schedule for the jobs return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/16/untitled.png](https://assets.leetcode.com/uploads/2020/01/16/untitled.png)

```
Input: jobDifficulty = [6,5,4,3,2,1], d = 2
Output: 7
Explanation: First day you can finish the first 5 jobs, total difficulty = 6.
Second day you can finish the last job, total difficulty = 1.
The difficulty of the schedule = 6 + 1 = 7

```

**Example 2:**

```
Input: jobDifficulty = [9,9,9], d = 4
Output: -1
Explanation: If you finish a job per day you will still have a free day. you cannot find a schedule for the given jobs.

```

**Example 3:**

```
Input: jobDifficulty = [1,1,1], d = 3
Output: 3
Explanation: The schedule is one job per day. total difficulty will be 3.

```

**Constraints:**

- `1 <= jobDifficulty.length <= 300`
- `0 <= jobDifficulty[i] <= 1000`
- `1 <= d <= 10`

```jsx
class Solution {
public:
    int minDifficulty(vector<int>& nums, int K) {
        int n=nums.size(); if(n==0) {return 0;}
        if(K>n) return -1;
        //calcu difficulty
        vector<vector<int>> diff(n,vector<int>(n,0));
        for(int i=0; i<n; i++){
            diff[i][i]=nums[i];
        }
        for(int len=2;len<=n;len++){//len of a brick
            for(int l=0,r;r=l+len-1,r<n;l++){//for each brick
                diff[l][r]=max(diff[l+1][r],diff[l][r-1]);
            }
        }
        //calcu res
        vector<vector<int>> dp(n,vector<int>(K+1,INT_MAX/2));//assign invalid
        for(int i=0;i<n;i++){
            for(int k=1;k<=min(i+1,K);k++){
                for(int j=i;j>=k-1;j--){
                    dp[i][k]=min(dp[i][k], (j-1>=0?dp[j-1][k-1]:0) + diff[j][i]);
                }
            }
        }
        return dp.back().back();
    }
};
```