# Number of Sets of K Non-Overlapping Line Segments

#: 1621
Difficult: Medium
Tags: DP
link: https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/
Priority: High
Status: Dig More, In Progress, No Idea At First
Created time: June 15, 2023 4:51 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Given `n` points on a 1-D plane, where the `ith` point (from `0` to `n-1`) is at `x = i`, find the number of ways we can draw **exactly** `k` **non-overlapping** line segments such that each segment covers two or more points. The endpoints of each segment must have **integral coordinates**. The `k` line segments **do not** have to cover all `n` points, and they are **allowed** to share endpoints.

Return *the number of ways we can draw* `k` *non-overlapping line segments.* Since this number can be huge, return it **modulo** `109 + 7`.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/07/ex1.png](https://assets.leetcode.com/uploads/2020/09/07/ex1.png)

```
Input: n = 4, k = 2
Output: 5
Explanation: The two line segments are shown in red and blue.
The image above shows the 5 different ways {(0,2),(2,3)}, {(0,1),(1,3)}, {(0,1),(2,3)}, {(1,2),(2,3)}, {(0,1),(1,2)}.

```

**Example 2:**

```
Input: n = 3, k = 1
Output: 3
Explanation: The 3 ways are {(0,1)}, {(0,2)}, {(1,2)}.

```

**Example 3:**

```
Input: n = 30, k = 7
Output: 796297179
Explanation: The total number of possible ways to draw 7 line segments is 3796297200. Taking this number modulo 109 + 7 gives us 796297179.

```

**Constraints:**

- `2 <= n <= 1000`
- `1 <= k <= n-1`

Solution 1 TLE:

```cpp
using ll=long long;
int M=1e9+7;

class Solution {
public:
    int numberOfSets(int n, int K) {
        vector<vector<vector<ll>>> dp(n,vector<vector<ll>>(n,vector<ll>(K+1,0)));
        for(int l=0;l<n;l++){
            for(int r=l;r<n;r++){
                dp[l][r][1]=r-l;
            }
        }

        for(int len=2;len<=n;len++){//len of a brick
            for(int l=0,r;r=l+len-1,r<n;l++){//for each brick
                for(int k=2; k<=K; k++){
                    for(int x=l;x<=r;x++){
                        dp[l][r][k]+= (dp[l][x][k-1] * dp[x][r][1]);
                        dp[l][r][k]%=M;
                    }
                }
            }
        }

        int res=0;
        for(int j=0; j<n; j++){
            res+= dp[0][j][K];
            res%=M;
        }
        return res;
    }
};

/*
**Intuition**
fixed end idx to dedup
start idx, end idx -> cnt of ways to draw this line -> recur / itera

dp[l][r][k] = sum of dp[l][x][k-1]*dp[x][r][1]

**Explanation**

**Complexity**

*/
```

Solution 2 TLE:

```cpp
using ll=long long;
int M=1e9+7;

class Solution {
public:
    int numberOfSets(int n, int K) {
        vector<vector<ll>> dp(n,vector<ll>(K+1,0));
        for(int i=0;i<n;i++){//[0...i]
            dp[i][1]=i;
        }

        for(int i=0;i<n;i++){
            for(int k=2; k<=K; k++){
                for(int x=0;x<=i;x++){
                    dp[i][k]+= (dp[x][k-1] * (i-x));
                    dp[i][k]%=M;
                }
            }
        }

        int res=0;
        for(int i=0; i<n; i++){
            res+= dp[i][K];
            res%=M;
        }
        return res;
    }
};

/*
**Intuition**
fixed end idx to dedup
start idx, end idx -> cnt of ways to draw this line -> recur / itera

dp[l][r][k] = sum of dp[l][x][k-1]*dp[x][r][1]

**Explanation**

**Complexity**

*/
```

Solution 3:

```cpp

```