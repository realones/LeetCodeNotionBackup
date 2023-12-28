# Generate Parentheses

#: 22
Difficult: Medium
Tags: BackTracking, DFS, DP
link: https://leetcode.com/problems/generate-parentheses/
Priority: Low
Status: More Solution
Created time: June 7, 2023 2:05 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

**Example 1:**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]

```

**Example 2:**

```
Input: n = 1
Output: ["()"]

```

**Constraints:**

- `1 <= n <= 8`

Solution w/o backtracking:

```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> res;
        string tmp="";
        
        helper(res,tmp,n,0,0);
            
        return res;
    }
    
    void helper(vector<string>& res,string tmp, int n, int lcnt, int rcnt){
        if(lcnt==n && rcnt==n) {res.push_back(tmp); return;}
        
        if(lcnt==n){
            helper(res,tmp+")",n,lcnt,rcnt+1);
        }
        else if(lcnt>rcnt){
            helper(res,tmp+"(",n,lcnt+1,rcnt);
            helper(res,tmp+")",n,lcnt,rcnt+1);
        }
        else{//==
            helper(res,tmp+"(",n,lcnt+1,rcnt);
        }
    }
};
```

Solution with backtracking:

```cpp
class Solution {
public:
    vector<string> res;
    string tmp;

    vector<string> generateParenthesis(int n) {
        helper(0,0,n);
        return res;
    }

    //return possible ways
    void helper(int l, int r, int n){
        if(l==n&&r==n) {
            res.push_back(tmp);
            return;
        }
        if(l==r){
            tmp+="(";
            helper(l+1,r,n);
            tmp.pop_back();
        }
        else if(l>r){
            if(l==n){
                tmp+=")";
                helper(l,r+1,n);
                tmp.pop_back();
            }
            else{
                tmp+="(";
                helper(l+1,r,n);
                tmp.pop_back();

                tmp+=")";
                helper(l,r+1,n);
                tmp.pop_back();
            }
        }
    }
};
```