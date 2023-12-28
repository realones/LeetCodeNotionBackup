# Number of Music Playlists

#: 920
Difficult: Hard
Tags: Combinatorics, Math, dp-noPattern
link: https://leetcode.com/problems/number-of-music-playlists/
Priority: High
Status: Dig More, More Solution, No Idea At First, noPatten
Created time: May 19, 2023 12:25 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily, wp动归套路

Your music player contains `n` different songs. You want to listen to `goal` songs (not necessarily different) during your trip. To avoid boredom, you will create a playlist so that:

- Every song is played **at least once**.
- A song can only be played again only if `k` other songs have been played.

Given `n`, `goal`, and `k`, return *the number of possible playlists that you can create*. Since the answer can be very large, return it **modulo** `1e9 + 7`.

**Example 1:**

```
Input: n = 3, goal = 3, k = 1
Output: 6
Explanation: There are 6 possible playlists: [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], and [3, 2, 1].

```

**Example 2:**

```
Input: n = 2, goal = 3, k = 0
Output: 6
Explanation: There are 6 possible playlists: [1, 1, 2], [1, 2, 1], [2, 1, 1], [2, 2, 1], [2, 1, 2], and [1, 2, 2].

```

**Example 3:**

```
Input: n = 2, goal = 3, k = 1
Output: 2
Explanation: There are 2 possible playlists: [1, 2, 1] and [2, 1, 2].

```

**Constraints:**

- `0 <= k < n <= goal <= 100`

Solution:

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int numMusicPlaylists(int n, int goal, int k) {
        vector<vector<ll>> dp(goal+1,vector<ll>(n+1,0));
        dp[1][1]=n;
        for(int i=2;i<=goal;i++){
            for(int j=1;j<=n;j++){
                dp[i][j]=
                    ((j-k>0? dp[i-1][j]*(j-k) : 0 )+
                    (n-j+1>0? dp[i-1][j-1]*(n-j+1) : 0 ))%M;
            }
        }
        return dp[goal][n];
    }
};

/*
n items
build arr size = goal
same item can occur after k other items in between

return: how many builds
================================
k: 0~n-1
n: 1~goal
goal: n~100
================================
return f goal n = ?

fij: i play positions, j songs played

fij =
    f[i][j-1]: no
    f[i-1][j] * j-k (only if j-k>0)
    f[i-1][j-1] * n-(j-1) (only if n-j>0)

init: f 1 1 = n
*
```