# Count Vowels Permutation

#: 1220
Difficult: Hard
Tags: Graph, dp-TimeSeqFromLastOne-Power, dp-bfs-stepByStep
link: https://leetcode.com/problems/count-vowels-permutation/description/
Priority: Low
Created time: June 28, 2023 12:42 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Knight Dialer (Knight%20Dialer%20ad2dc741f775475aadc6ce3fa768f12f.md), Number of Ways to Stay in the Same Place After Some Steps (Number%20of%20Ways%20to%20Stay%20in%20the%20Same%20Place%20After%20Som%2091772bfb41a84c87b0a4d56f1f7f74bd.md)

Given an integer `n`, your task is to count how many strings of length `n` can be formed under the following rules:

- Each character is a lower case vowel (`'a'`, `'e'`, `'i'`, `'o'`, `'u'`)
- Each vowel `'a'` may only be followed by an `'e'`.
- Each vowel `'e'` may only be followed by an `'a'` or an `'i'`.
- Each vowel `'i'` **may not** be followed by another `'i'`.
- Each vowel `'o'` may only be followed by an `'i'` or a `'u'`.
- Each vowel `'u'` may only be followed by an `'a'.`

Since the answer may be too large, return it modulo `10^9 + 7.`

**Example 1:**

```
Input: n = 1
Output: 5
Explanation: All possible strings are: "a", "e", "i" , "o" and "u".

```

**Example 2:**

```
Input: n = 2
Output: 10
Explanation: All possible strings are: "ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" and "ua".

```

**Example 3:**

```
Input: n = 5
Output: 68
```

**Constraints:**

- `1 <= n <= 2 * 10^4`

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int countVowelPermutation(int n) {
        unordered_map<int,unordered_set<int>> g;
        g[0].insert({1});
        g[1].insert({0,2});
        g[2].insert({0,1,3,4});
        g[3].insert({2,4});
        g[4].insert({0});

        vector<vector<ll>> dp(n,vector<ll>(5,0));
        for(int j=0; j<5; j++){ dp[0][j]=1; }
        for(int i=0;i<n-1;i++){//i->i+1
            for(int j=0;j<5;j++){
                for(int nj: g[j]){
                    dp[i+1][nj]+=dp[i][j];
                    dp[i+1][nj]%=M;
                }
            }
        }
        return accumulate(dp.back().begin(), dp.back().end(),0LL)%M;
    }
};

//if input n is big, use binary lifting
/*
0 1 2 3 4
a e i o u

0:1
1:0,2
2:2
3:2,4
4:0
*/
```