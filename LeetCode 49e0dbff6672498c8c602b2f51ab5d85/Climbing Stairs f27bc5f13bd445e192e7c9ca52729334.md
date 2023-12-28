# Climbing Stairs

#: 70
Difficult: Easy
Tags: Array, DP, GoForth
link: https://leetcode.com/problems/climbing-stairs
Priority: Pass
Created time: August 17, 2022 11:27 PM
Last edited time: October 13, 2023 1:02 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初DP, 算高DP上

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

```

**Example 2:**

```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

```

**Constraints:**

- `1 <= n <= 45`

state: f[i]表示跳到第i个位置的方案总数
function: f[i] = f[i-1] + f[i-2]
initialize: f[0] = 1, f[1]=2
answer: f[n] // index from 0~n

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: An integer
     */
    int climbStairs(int n) {
        if(n<=0) return 0;
        vector<int> res(2,0);
        res[0]=1;
        res[1]=2;
        for(int i=2;i<n;i++){
            res[i%2]=res[(i-1)%2]+res[(i-2)%2];
        }
        return res[(n-1)%2];
    }
};
```