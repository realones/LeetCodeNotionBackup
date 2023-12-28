# Count K-Subsequences of a String With Maximum Beauty

#: 2842
Difficult: Hard
Tags: Combinatorics, Greedy, Hash, Math, string
link: https://leetcode.com/problems/count-k-subsequences-of-a-string-with-maximum-beauty/
Priority: Medium
Status: HardToImplement, checkBetterSolution
Created time: September 8, 2023 9:26 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a string `s` and an integer `k`.

A **k-subsequence** is a **subsequence** of `s`, having length `k`, and all its characters are **unique**, **i.e**., every character occurs once.

Let `f(c)` denote the number of times the character `c` occurs in `s`.

The **beauty** of a **k-subsequence** is the **sum** of `f(c)` for every character `c` in the k-subsequence.

For example, consider `s = "abbbdd"` and `k = 2`:

- `f('a') = 1`, `f('b') = 3`, `f('d') = 2`
- Some k-subsequences of `s` are:
    - `"**ab**bbdd"` -> `"ab"` having a beauty of `f('a') + f('b') = 4`
    - `"**a**bbb**d**d"` -> `"ad"` having a beauty of `f('a') + f('d') = 3`
    - `"a**b**bb**d**d"` -> `"bd"` having a beauty of `f('b') + f('d') = 5`

Return *an integer denoting the number of k-subsequences whose **beauty** is the **maximum** among all **k-subsequences***. Since the answer may be too large, return it modulo `109 + 7`.

A subsequence of a string is a new string formed from the original string by deleting some (possibly none) of the characters without disturbing the relative positions of the remaining characters.

**Notes**

- `f(c)` is the number of times a character `c` occurs in `s`, not a k-subsequence.
- Two k-subsequences are considered different if one is formed by an index that is not present in the other. So, two k-subsequences may form the same string.

**Example 1:**

```
Input: s = "bcca", k = 2
Output: 4
Explanation: From s we have f('a') = 1, f('b') = 1, and f('c') = 2.
The k-subsequences of s are:
bcca having a beauty of f('b') + f('c') = 3
bcca having a beauty of f('b') + f('c') = 3
bcca having a beauty of f('b') + f('a') = 2
bccahaving a beauty of f('c') + f('a') = 3
bcca having a beauty of f('c') + f('a') = 3
There are 4 k-subsequences that have the maximum beauty, 3.
Hence, the answer is 4.

```

**Example 2:**

```
Input: s = "abbcd", k = 4
Output: 2
Explanation: From s we have f('a') = 1, f('b') = 2, f('c') = 1, and f('d') = 1.
The k-subsequences of s are:
abbcd having a beauty of f('a') + f('b') + f('c') + f('d') = 5
abbcd having a beauty of f('a') + f('b') + f('c') + f('d') = 5
There are 2 k-subsequences that have the maximum beauty, 5.
Hence, the answer is 2.

```

**Constraints:**

- `1 <= s.length <= 2 * 105`
- `1 <= k <= s.length`
- `s` consists only of lowercase English letters.

comment:

the overflow part when input is big is hard to implement

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int countKSubsequencesWithMaxBeauty(string s, int k) {
        ll res=1;
        int n=s.size();
        unordered_map<char,int> freq;
        for(char c: s) freq[c]++;
        map<int, unordered_set<char>,greater<>> freq_chars;//freq large to small
        for(auto [c,f]: freq) freq_chars[f].insert(c);
        
        for(auto& [f,st]: freq_chars){
            if(k>st.size()){//cant cover all k
                res*=modPow(f,st.size(),M); res%=M;
            }
            if(k<=st.size()){//last f, return after calcu
                res*= BinomialCoefficient(st.size(), k); res%=M;
                res*= modPow(f,k,M); res%=M;
                return res;
            }
            k-=st.size();
        }
        
        return 0;
    }
    
    ll BinomialCoefficient(const int n, const int k) {
      vector<ll> aSolutions(k);
      aSolutions[0] = n - k + 1;

      for (int i = 1; i < k; ++i) {
        aSolutions[i] = aSolutions[i - 1] * (n - k + 1 + i) / (i + 1);
        aSolutions[i]%=M;
      }

      return aSolutions[k - 1];
    }

    int modPow(int x, unsigned int y, unsigned int m) {
        if (y == 0)
            return 1;
        long p = modPow(x, y / 2, m) % m;
        p = (p * p) % m;
        return y % 2 ? (p * x) % m : p;
    }
};
/*
hash mp c_freq
tree mp freq_[c]

from most freq to less freq in tree_mp
    freq_st
        if(sz+=st.size() >=k) can cover all k, then use the chars so far
        else goto next itr

then from most freq to less freq

for most freq, pick all - for each char in that freq, res*=freq
for next freq, pick all - ...
for last freq, pick x out of st.sz, x is the remaining of k after used by above chars, C (st.sz, remain)

*/
```