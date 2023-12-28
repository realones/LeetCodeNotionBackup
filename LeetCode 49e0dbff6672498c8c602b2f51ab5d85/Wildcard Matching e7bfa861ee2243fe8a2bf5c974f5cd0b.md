# Wildcard Matching

#: 44
Difficult: Hard
Tags: DP, Greedy, recursion, string
link: https://leetcode.com/problems/wildcard-matching/description/
Priority: High
Status: HardToImplement, More Solution, toRevisit
Created time: August 22, 2023 12:39 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'` where:

- `'?'` Matches any single character.
- `'*'` Matches any sequence of characters (including the empty sequence).

The matching should cover the **entire** input string (not partial).

**Example 1:**

```
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".

```

**Example 2:**

```
Input: s = "aa", p = "*"
Output: true
Explanation: '*' matches any sequence.

```

**Example 3:**

```
Input: s = "cb", p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.

```

**Constraints:**

- `0 <= s.length, p.length <= 2000`
- `s` contains only lowercase English letters.
- `p` contains only lowercase English letters, `'?'` or `'*'`.

Comment:

check greedy solution, leetcode has much shorter solution

Solutions:

solution recursion with memo:

```cpp
class Solution {
public:
    int sn, pn;
    vector<vector<int>> dp;

    bool isMatch(string s, string p) {
        sn=s.size(), pn=p.size();
        dp=vector<vector<int>> (sn,vector<int>(pn,-1));

        return helper(s,0,p,0);
    }

    bool helper(string& s, int si, string &p, int pi){
        if(si==sn && pi==pn) return true; 
        if(si==sn) return p.find_first_not_of('*',pi)==-1;
        if(pi==pn) return false;
        if(dp[si][pi]!=-1) return dp[si][pi];

        if(p[pi]=='*'){
            for(int k=0;si+k<=sn;k++){
                if(helper(s,si+k,p,pi+1)) {dp[si][pi]=true; return true;};
            }
            dp[si][pi]=false; return false;
        }
        else if(p[pi]=='?'){
            dp[si][pi]=helper(s,si+1,p,pi+1); return dp[si][pi];
        }
        else{
            if(s[si]==p[pi]){
                dp[si][pi]=helper(s,si+1,p,pi+1); return dp[si][pi];
            }
            else {dp[si][pi]=false; return false;} 
        }
    }
};

/*
input string: s
pattern: p
    ? match any single char
    * match [0~k] chars
*/
```

Solution 2:

```cpp
//from the ans

class Solution {
public:
    bool isMatch(const char *s, const char *p) {
        int last_star = -1, last_s = -1, i, j;
        for(i = j = 0; s[i]; )
        {
            if(s[i] == p[j] || p[j] == '?') i ++, j ++;
            else if(p[j] == '*') last_star = ++ j, last_s = i;
            else if(last_star != -1) i = ++ last_s, j = last_star;
            else return false;
        }
        while(p[j] == '*') j ++;
        return !s[i] && !p[j];
    }
};
```