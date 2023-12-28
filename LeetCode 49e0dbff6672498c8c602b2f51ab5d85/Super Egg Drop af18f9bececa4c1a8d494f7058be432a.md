# Super Egg Drop

#: 887
Difficult: Hard
Tags: BS_todo, dp-noPattern
link: https://leetcode.com/problems/super-egg-drop/
Priority: High
Status: No Idea At First, NotUnderstand, noPatten, out of scope, toRevisit
Created time: May 19, 2023 12:25 PM
Last edited time: October 17, 2023 6:24 PM
source: HuaHua, wp动归套路

You are given `k` identical eggs and you have access to a building with `n` floors labeled from `1` to `n`.

You know that there exists a floor `f` where `0 <= f <= n` such that any egg dropped at a floor **higher** than `f` will **break**, and any egg dropped **at or below** floor `f` will **not break**.

Each move, you may take an unbroken egg and drop it from any floor `x` (where `1 <= x <= n`). If the egg breaks, you can no longer use it. However, if the egg does not break, you may **reuse** it in future moves.

Return *the **minimum number of moves** that you need to determine **with certainty** what the value of* `f` is.

**Example 1:**

```
Input: k = 1, n = 2
Output: 2
Explanation:
Drop the egg from floor 1. If it breaks, we know that f = 0.
Otherwise, drop the egg from floor 2. If it breaks, we know that f = 1.
If it does not break, then we know f = 2.
Hence, we need at minimum 2 moves to determine with certainty what the value of f is.

```

**Example 2:**

```
Input: k = 2, n = 6
Output: 3

```

**Example 3:**

```
Input: k = 3, n = 14
Output: 4

```

**Constraints:**

- `1 <= k <= 100`
- `1 <= n <= 104`

Solution - TLE:

dp[k][n] min= 1 + max(break: dp[k-1][i-1], not_break: dp[k][n-i]), i: 1~n

O(kn^2)

```cpp
class Solution {
public:
    int superEggDrop(int k, int n) {
        vector<vector<int>> dp(n+1,vector<int>(k+1,INT_MAX));
        for(int i=0; i<=n; i++) dp[i][1]=i;
        for(int i=0; i<=k; i++) dp[0][i]=0;

        function<int(int,int)> f =[&](int n, int k)->int{
            if(n<k) return f(n,n);//cut branch
            if(dp[n][k]!=INT_MAX) return dp[n][k];
            int res=INT_MAX;
            for(int x=1;x<=n;x++){
                res=min(res,
                        1 + max(
                                f(x-1,k-1),//break
                                f(n-x,k)//not break
                            )
                       );
            }
            dp[n][k]=res;
            return res;
        };

        return f(n,k);
    }
};

/*
k eggs
floor: 1~n
    can be all good
    can be all bad
    must be:
        g g g b b b
    find the first bad, can be 0~n
================================
f(n,k): 1~n floors, k eggs, min num of moves

f(n,k) min= for x:1~n
    1 + 
    max of
        case 1:     break on floor x: f(1~x-1,k-1)
        case 2: not break on floor x: f(x+1~n,k  )

init/corner:
f(?,1) = num of floors
f(0,?) = 0

return: f(n,k)

================================>>
improve from above:
f(n,k): n floor cnt, k eggs, min num of moves
f(n,k) min= for x:1~n
    1 + 
    max of
        case 1:     break on floor x: f(x-1, k-1)
        case 2: not break on floor x: f(n-x, k  )
init/corner:
    f(m,1) = m
    f(0,t) = 0
    f(m,t) = f(m,m) if t>m //cut branch
return: f(n,k)
*/
```

Solution 2:

init and return part not clarified

```cpp
class Solution {
public:
    int superEggDrop(int k, int n) {
        vector<vector<int>> dp(n+1,vector<int>(k+1,0));
        int m=0;
        while(dp[m][k]<n){
            m++;
            for(int x=1;x<=k;x++)
                dp[m][x]=dp[m-1][k-1] + dp[m-1][k]+1;
        }
        return m;
    }
};

/*
lee215:

dp[m][k]: k eggs, m moves, max num of floors we can check

dp[m][k] = dp[m - 1][k - 1] + dp[m - 1][k] + 1,

dp[m][k] = 
    1st move to the floor dp[m-1][k-1]+1,
        if egg break, we can check dp[m-1][k-1] floors
        if egg not break: then we can check dp[m-1][k] floors

================================to understand:    
assume:
    dp[m-1][k-1] = n0, when break
    dp[m-1][k] = n1, when not_break
the first floor to check is n0+1.
    if egg breaks, F must be in [1,n0] floors, we can use m-1 moves and k-1 eggs to find out F is which one.
    if egg doesn't breaks and F is in [n0+1 +1, n0+1 +n1] floors, we can use m-1 moves and k eggs to find out F is which one.
So, with m moves and k eggs, we can find out F in n0+n1+1 floors, whichever F is
================================

dp[m][k] is the number of combinations and increase exponentially to n

init:

return:

T: O(KlogN) to process
S: O(NK)
*/
```