# Minimum Remove to Make Valid Parentheses

#: 1249
Difficult: Medium
Tags: stack, string
link: https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/description/
Priority: Low
Status: checkBetterSolution
Created time: November 23, 2023 3:17 AM
Last edited time: November 23, 2023 3:18 AM
source: facebookFreq

Given a string s of `'('` , `')'` and lowercase English characters.

Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting *parentheses string* is valid and return **any** valid string.

Formally, a *parentheses string* is valid if and only if:

- It is the empty string, contains only lowercase characters, or
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.

**Example 1:**

```
Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.

```

**Example 2:**

```
Input: s = "a)b(c)d"
Output: "ab(c)d"

```

**Example 3:**

```
Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.

```

**Constraints:**

- `1 <= s.length <= 105`
- `s[i]` is either`'('` , `')'`, or lowercase English letter`.`

```cpp
using ic=pair<int,char>;

class Solution {
public:
    string minRemoveToMakeValid(string s) {
        int n=s.size();// if(n==0) {return 0;}
        vector<ic> stk; //idx, 0l/1r
        for(int i=0;i<n;i++){
            char c=s[i];
            // (:enq(0,idx)
            if(c=='('){
                stk.emplace_back(i,'(');
            }
            // ):deq top (, if any, else enq
            else if(c==')'){
                if(!stk.empty() && stk.back().second=='(') stk.pop_back();
                else stk.emplace_back(i,')');
            }
        }
        string res;
        int idx=0;
        for(int i=0; i<n; i++){
            if(idx<stk.size() && i==stk[idx].first) {idx++; continue;}
            res+=s[i];
        }
        return res;
    }
};
/*
'(' : must pair with right ')', 
')' : must pair with left '(', so if cntL==0, must remove the ')'
================================
maintain a list of invalid (, ), in stk
(:enq(0,idx)
):deq top (, if any, else enq
*/
```