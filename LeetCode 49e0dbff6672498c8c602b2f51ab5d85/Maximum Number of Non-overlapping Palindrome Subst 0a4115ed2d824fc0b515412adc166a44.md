# Maximum Number of Non-overlapping Palindrome Substrings

#: 2472
Difficult: Hard
Tags: DP, string
link: https://leetcode.com/problems/maximum-number-of-non-overlapping-palindrome-substrings/description/
Priority: High
Status: Dig More, HardToImplement, More Solution, No Idea At First
Created time: December 13, 2023 12:38 AM
Last edited time: December 13, 2023 1:18 AM
source: walmartFreq

You are given a string `s` and a **positive** integer `k`.

Select a set of **non-overlapping** substrings from the string `s` that satisfy the following conditions:

- The **length** of each substring is **at least** `k`.
- Each substring is a **palindrome**.

Return *the **maximum** number of substrings in an optimal selection*.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

```
Input: s = "abaccdbbd", k = 3
Output: 2
Explanation: We can select the substrings underlined in s = "abaccdbbd". Both "aba" and "dbbd" are palindromes and have a length of at least k = 3.
It can be shown that we cannot find a selection with more than two valid substrings.

```

**Example 2:**

```
Input: s = "adbcda", k = 2
Output: 0
Explanation: There is no palindrome substring of length at least 2 in the string.

```

**Constraints:**

- `1 <= k <= s.length <= 2000`
- `s` consists of lowercase English letters.

Solution greedy votrubac:

```cpp
class Solution {
public:
    int maxPalindromes(string s, int k) {
        int n=s.size();
        if(k==1) return n;
        //pyr
        vector<vector<bool>> dp(n+1,vector<bool>(n+1,false));
        for(int i=0;i<n;i++){ dp[i][i]=true; }
        for(int i=0;i<n-1;i++){ dp[i][i+1]=s[i]==s[i+1]; }
        for(int len=3;len<=k+1;len++){
            for(int l=0,r;r=l+len-1,r<n;l++){
                dp[l][r]= s[l]==s[r] && dp[l+1][r-1];
            }
        }
        //greedy
        int res=0;
        for(int l=0,r; r=l+k-1,r<n;){
            res+=dp[l][r] || dp[l][r+1];
            if(dp[l][r]) l=r+1;
            else if(dp[l][r+1]) l=r+2;
            else l++;
        }
        return res;
    }
};
```

Solution: not finished, to complex

```cpp
class Solution {
public:
    int core1=0, core2=1;
    int maxPalindromes(string s, int k) {
        int n=s.size();
        int oddLen,evenLen;
        if(k%2==1){
            oddLen=k, evenLen=k+1;
        }
        else{
            oddLen=k+1, evenLen=k;
        }
        int oddSpan = (oddLen+1) / 2;
        int evenSpan = evenLen / 2;

        //func
        auto isPalin = [&](int i, int span, int core) -> bool{
            if(core==core1){
                int l=i,r=i;
                for(int k=0;i<span;k++){
                    if(l<0 || r>n-1) return false;
                    if(s[l]!=s[r]) return false;
                    l--,r++;
                }
                return true;
            }
            else{
                int l=i,r=i+1;
                for(int k=0;i<span;k++){
                    if(l<0 || r>n-1) return false;
                    if(s[l]!=s[r]) return false;
                    l--,r++;
                }
                return true;
            }
        };

        vector<vector<int>> f(n,vector<int>(2,0));
        for(int i=0; i<n; i++){
            f[i][core1] = max({
                    isPalin(i,oddSpan,core1)? f[i-2*oddSpan+1][core1] + 1 : 0,
                    isPalin(i,oddSpan,core1)? f[i-oddSpan-evenSpan][core2] + 1 : 0,
                    f[i-1][core1],
                    f[i-1][core2]
                });
            f[i][core2] = max({
                    isPalin(i,evenSpan,core2)? f[i-oddSpan-evenSpan+1][core1] + 1 : 0,
                    isPalin(i,evenSpan,core2)? f[i-2*evenSpan][core2] + 1 : 0,
                    f[i-1][core1],
                    f[i-1][core2]
                });
        }
        return max(dp[n-1][core1], dp[n-1][core2]);
    }
    
};

/*
string: s

select non-overlapping substrs:
    len>=k
    palin
return: max num of substr to select
================================
s:
    len: 1~2000
    val: lowerCase 26
k: 1~s.len
================================
intui:
    use the smaller palindrome substr, only with len==k or k+1
    palindrome has core, with 1 or 2 elem
    iteratively take the as left most ones as possible, no matter core is 1 or 2 elem
    dp[i][1core or 2 core] = 
        try use cur elem as core if possible
            dp[i- k/2][1core or 2core] + 1
        not use cur as core
            dp[i-1][1core or 2core]
    return max dp[n-1][1 or 2 core]
================================
dp:
    k
        odd
            odd len: k
            even len: k+1
        even
            odd len: k+1
            even len: k
    for 1core or 2core: span including core = (len+1)/2

    core: i, i+1

    f[i][1core] max= 
        f[i-2span+1][1core] + 1
        f[i-2span][2core] + 1
        f[i-1][1core]
        f[i-1][2core]
    f[i][2core] max= 
        f[i-2span+1][1core] + 1
        f[i-2span][2core] + 1
        f[i-1][1core]
        f[i-1][2core]

    return max dp[n-1][1 or 2 core]
*/
```