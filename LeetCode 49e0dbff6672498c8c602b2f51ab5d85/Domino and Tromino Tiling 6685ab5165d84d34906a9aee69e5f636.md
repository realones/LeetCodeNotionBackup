# Domino and Tromino Tiling

#: 790
Difficult: Medium
Tags: DP
link: https://leetcode.com/problems/domino-and-tromino-tiling
Priority: High
Status: Dig More, More Solution, No Idea At First
Created time: June 28, 2023 12:50 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75

You have two types of tiles: a `2 x 1` domino shape and a tromino shape. You may rotate these shapes.

![https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg](https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg)

Given an integer n, return *the number of ways to tile an* `2 x n` *board*. Since the answer may be very large, return it **modulo** `109 + 7`.

In a tiling, every square must be covered by a tile. Two tilings are different if and only if there are two 4-directionally adjacent cells on the board such that exactly one of the tilings has both squares occupied by a tile.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg](https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg)

```
Input: n = 3
Output: 5
Explanation: The five different ways are show above.

```

**Example 2:**

```
Input: n = 1
Output: 1

```

**Constraints:**

- `1 <= n <= 1000`

Solution:

一开始只想到下面几种end with 类型：

没想到例如

```
-
-

--
++

-++
--+

--+
-++

not correct, checked disgussion and it is possible that:
A....B or A....BB or ..
AA..BB    AA....B
```

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int numTilings(int n) {//1~1000
        vector<vector<ll>> dp(n+1,vector<ll>(2,0));
        dp[0][0]=1,dp[1][0]=1;
        for(int i=2;i<n+1;i++){
            dp[i][0]=(dp[i-1][0]+dp[i-2][0]+dp[i-1][1]*2)%M;
            dp[i][1]=(dp[i-2][0]+dp[i-1][1])%M;
        }
        return dp[n][0];
    }
};

/*
from huahua:

end with four types:
A
....-
....-
B
...--
...++
C
...++
....+
D
....+
...++

remain type:
.... type0
....
or
...  type1
....
or
.... type2
...

dp(i,j): subsolution for 1...i, type j
dp(k,0) sum=
    dp(k-1,0) : -A
    dp(k-2,0) : -B
    dp(k-2,1) : -C
    dp(k-2,2) : -D
dp(k,1) sum=
    dp(k-1,0) : -D
    dp(k-2,2) : -lower two
dp(k,2) sum=
    dp(k-1,0) : -C
    dp(k-2,1) : - upper two

improve above to:
dp(k,0) sum=
    dp(k-1,0) 
    dp(k-2,0) 
    dp(k-2,1) * 2 
dp(k,1) sum=
    dp(k-1,0) 
    dp(k-2,1) 
*/
```

Solution - not improved version of above:

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int numTilings(int n) {
        vector<vector<ll>> dp(n+1,vector<ll>(3,0));
        dp[0][0]=1, dp[0][1]=0, dp[0][2]=0;
        dp[1][0]=1, dp[1][1]=1, dp[1][2]=1;
        for(int i=2; i<=n; i++){
            dp[i][0]=(dp[i-2][0] + dp[i-1][0] + dp[i-2][1] + dp[i-2][2])%M;
            dp[i][1]=(dp[i-1][0] + dp[i-1][2])%M;
            dp[i][2]=(dp[i-1][0] + dp[i-1][1])%M;
        }
        return dp[n][0];
    }
};
```

Solution recur:

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int numTilings(int n) {
        vector<vector<int>> dp(n+1,vector<int>(n+1,-1));
        dp[0][0]=1;
        function<int(int,int)> helper = [&](int x, int y){
            if(x<0 || y<0) return 0;
            if(dp[x][y]!=-1) return dp[x][y];
            int res=0;
            if(x==y+1){
                res=(res+helper(x-2,y-1))%M;
                res=(res+helper(x-2,y))%M;
            }
            else if(x+1==y){
                res=(res+helper(x-1,y-2))%M;
                res=(res+helper(x,y-2))%M;
            }
            else if(x==y){
                res=(res+helper(x-1,y-1))%M;
                res=(res+helper(x-2,y-2))%M;
                res=(res+helper(x-2,y-1))%M;
                res=(res+helper(x-1,y-2))%M;
            }
            dp[x][y]=res;
            return res;
        };
        return helper(n,n);
    }
};
```