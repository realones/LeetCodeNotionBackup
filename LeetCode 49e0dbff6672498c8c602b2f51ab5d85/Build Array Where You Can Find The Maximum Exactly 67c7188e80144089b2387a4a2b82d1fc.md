# Build Array Where You Can Find The Maximum Exactly K Comparisons

#: 1420
Difficult: Hard
Tags: DP, Prefix
link: https://leetcode.com/problems/build-array-where-you-can-find-the-maximum-exactly-k-comparisons/description/
Priority: High
Status: Dig More, HardToImplement, No Idea At First, toSummarize
Created time: November 24, 2023 3:36 PM
Last edited time: November 24, 2023 3:39 PM
source: AmazonFreq

You are given three integers `n`, `m` and `k`. Consider the following algorithm to find the maximum element of an array of positive integers:

![https://assets.leetcode.com/uploads/2020/04/02/e.png](https://assets.leetcode.com/uploads/2020/04/02/e.png)

You should build the array arr which has the following properties:

- `arr` has exactly `n` integers.
- `1 <= arr[i] <= m` where `(0 <= i < n)`.
- After applying the mentioned algorithm to `arr`, the value `search_cost` is equal to `k`.

Return *the number of ways* to build the array `arr` under the mentioned conditions. As the answer may grow large, the answer **must be** computed modulo `109 + 7`.

**Example 1:**

```
Input: n = 2, m = 3, k = 1
Output: 6
Explanation: The possible arrays are [1, 1], [2, 1], [2, 2], [3, 1], [3, 2] [3, 3]

```

**Example 2:**

```
Input: n = 5, m = 2, k = 3
Output: 0
Explanation: There are no possible arrays that satisfy the mentioned conditions.

```

**Example 3:**

```
Input: n = 9, m = 1, k = 1
Output: 1
Explanation: The only possible array is [1, 1, 1, 1, 1, 1, 1, 1, 1]

```

**Constraints:**

- `1 <= n <= 50`
- `1 <= m <= 100`
- `0 <= k <= n`

toSummarize:

init的写法，看一下dp processing哪一些会出界：例如i==0, k==0, 那就直接修改dp processing从i=1, k=1，开始，然后init i=0，k=0的情况。

comment：

关键还是怎么定义dp的声明涵义

Solution wisdomPeak:

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int numOfArrays(int n, int m, int K) {
        // dp[i][j][k]: the num of ways to build arr[0:i] and max(max[0:i])=j and search cost is k
        vector<vector<vector<ll>>> dp(n,vector<vector<ll>>(m+1,vector<ll>(K+1,0)));
        // init: dp[0][1...m][1]=1;
        for(int j=1;j<=m;j++) dp[0][j][1]=1;
        //dp process
        for(int i=1;i<n;i++){
            for(int j=1;j<=m;j++){
                for(int k=1;k<=K;k++){
                    //case if arr[i] is the largest among arr[0:i] => arr[i]=j
                    ll case1=0;
                    for(int t=1;t<j;t++){
                        case1+=dp[i-1][t][k-1];
                        case1%=M;
                    }
                    //case else => arr[i]<j
                    ll case2=dp[i-1][j][k] * j;
                    case2%=M;
                    dp[i][j][k]=case1+case2;
                    dp[i][j][k]%=M;
                }
            }
        }
        // return: sum of dp[n-1][1...m][K]
        ll res=0;
        for(int j=1; j<=m; j++){
            res+=dp[n-1][j][K];
            res%=M;
        }
        return res;
    }
};

/*
================================
arr:
    len: n: 1~50
    val: 1~m: 1~100
search_cost should =k, 0~n(50)
================================
given: arr.len, max_val, total_cost
algo: for each larger, cost++
-->
so in total k larger than history elem
================================
kranges?
================================
from wisdomPeak: - the youtube video is great!
dp[i][j][k]: the num of ways to build arr[0:i] and max(max[0:i])=j and search cost is k
dp[i][j][k] = 
              case if arr[i] is the largest among arr[0:i] => arr[i]=j
                sum(dp[i-1][1...j-1][k-1]) * 1
            +
              case else => arr[i]<j
                dp[i-1][j][k] * j
init: dp[?][?][0]
return: sum of dp[n-1][1...m][K]
*/
```