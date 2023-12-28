# Soup Servings

#: 808
Difficult: Medium
Tags: DP, Math, Probability
link: https://leetcode.com/problems/soup-servings
Priority: High
Status: No Idea At First, toSummarize
Created time: August 3, 2023 1:41 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

There are two types of soup: **type A** and **type B**. Initially, we have `n` ml of each type of soup. There are four kinds of operations:

1. Serve `100` ml of **soup A** and `0` ml of **soup B**,
2. Serve `75` ml of **soup A** and `25` ml of **soup B**,
3. Serve `50` ml of **soup A** and `50` ml of **soup B**, and
4. Serve `25` ml of **soup A** and `75` ml of **soup B**.

When we serve some soup, we give it to someone, and we no longer have it. Each turn, we will choose from the four operations with an equal probability `0.25`. If the remaining volume of soup is not enough to complete the operation, we will serve as much as possible. We stop once we no longer have some quantity of both types of soup.

**Note** that we do not have an operation where all `100` ml's of **soup B** are used first.

Return *the probability that **soup A** will be empty first, plus half the probability that **A** and **B** become empty at the same time*. Answers within `10-5` of the actual answer will be accepted.

**Example 1:**

```
Input: n = 50
Output: 0.62500
Explanation: If we choose the first two operations, A will become empty first.
For the third operation, A and B will become empty at the same time.
For the fourth operation, B will become empty first.
So the total probability of A becoming empty first plus half the probability that A and B become empty at the same time, is 0.25 * (1 + 1 + 0.5 + 0) = 0.625.

```

**Example 2:**

```
Input: n = 100
Output: 0.71875

```

**Constraints:**

- `0 <= n <= 109`

总结：

1. 学会用ceil and floor
2. 总结这种概率题
    1. 每个final state的概率初始化
    2. 变成不同状态的转移方程： 0.25*(state1, state2,…)
3. 当N比较大，结果趋近于一的优化

Solution 1: from lee215

"Note that we do not have the operation where all 100 ml's of soup B are used first. "
It's obvious that A is easier to be empty than B. And when N gets bigger, we have less chance to run out of B first.
So as N increases, our result increases and it gets closer to 100 percent = 1.

Answers within 10^-5 of the true value will be accepted as correct.
Now it's obvious that when N is big enough, result is close enough to 1 and we just need to return 1.
When I incresed the value of N, I find that:
When N = 4800, the result = 0.999994994426
When N = 4801, the result = 0.999995382315
So if N>= 4800, just return 1 and it will be enough.

```cpp
class Solution {
public:
    double soupServings(int n) {
        if(n>4800) return 1;
        n = n/25 + (n%25>0?1:0);
        unordered_map<int,unordered_map<int,double>> dp;

        return helper(dp,n,n);
    }
    double helper(unordered_map<int,unordered_map<int,double>>& dp, int a, int b) {
        if(a<=0 && b<=0) return 0.5;
        if(a<=0) return 1;
        if(b<=0) return 0;
        if(dp[a][b]!=0) return dp[a][b];
        dp[a][b]=0.25*(
            helper(dp, a-4, b  )+
            helper(dp, a-3, b-1)+
            helper(dp, a-2, b-2)+
            helper(dp, a-1, b-3)
        );
        return dp[a][b];
    }
};

/*
soup
    type A - n ml
    type B - n ml

ops -equal posibility:
- A100 B0
- A75  B25
- A50  B50
- A25  B75

corner case 1:
    if remaining vol of soup not enough but >0, serve as mush as possible

stop when:
    any==0

return:
    probability A==0 first + half the probability A and B ==0 at same time

dp(A_remain , B_remain) = posibility

init: -- *I don't know this part*
dp(<=0,<=0)=1/2
dp(<=0,> 0)=1
dp(> 0,<=0)=0

process:
dp(n,n) = 0.25*
    (
        dp(n-100,n   )+
        dp(n-75 ,n-25)+
        dp(n-50 ,n-50)+
        dp(n-25 ,n-72)
    )

================================
constraints:
n: 0~1e9 : too large

n=n/25 + (n%25>0?1:0)
dp(n,n) = 0.25*
    (
        dp(n-4,n  )+
        dp(n-3,n-1)+
        dp(n-2,n-2)+
        dp(n-1,n-3)
    )
*/
```

Solution 2: from Editorial

```cpp
/*
from Editorial to get streak
*/
class Solution {
public:
    double soupServings(int n) {
        int m = ceil(n / 25.0);
        unordered_map<int, unordered_map<int, double>> dp;

        function<double(int, int)> calculateDP = [&](int i, int j) -> double {
            if (i <= 0 && j <= 0) {
                return 0.5;
            }
            if (i <= 0) {
                return 1;
            }
            if (j <= 0) {
                return 0;
            }
            if (dp[i].count(j)) {
                return dp[i][j];
            }
            return dp[i][j] = (calculateDP(i - 4, j) + calculateDP(i - 3, j - 1) +
                               calculateDP(i - 2, j - 2) + calculateDP(i - 1, j - 3)) /
                              4;
        };

        for (int k = 1; k <= m; k++) {
            if (calculateDP(k, k) > 1 - 1e-5) {
                return 1;
            }
        }
        return calculateDP(m, m);
    }
};
```