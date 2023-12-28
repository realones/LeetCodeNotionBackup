# Regular Expression Matching

#: 10
Difficult: Hard
Tags: DP, recursion, string
link: https://leetcode.com/problems/regular-expression-matching/description/
Priority: Low
Created time: September 23, 2023 12:55 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

- `'.'` Matches any single character.
- `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

**Example 1:**

```
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".

```

**Example 2:**

```
Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

```

**Example 3:**

```
Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".

```

**Constraints:**

- `1 <= s.length <= 20`
- `1 <= p.length <= 20`
- `s` contains only lowercase English letters.
- `p` contains only lowercase English letters, `'.'`, and `'*'`.
- It is guaranteed for each appearance of the character `'*'`, there will be a previous valid character to match.

Solution new:

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m=s.size(), n=p.size();
        vector<vector<int>> dp(m,vector<int>(n,-1));
        function<bool(int,int)> f = [&](int i, int j){
            if(i==m && j==n) return true;
            if(i==m) {
                while(j+1<n && p[j+1]=='*') j+=2;
                return j==n;
            }
            if(j==n) return false;
            if(dp[i][j]!=-1) return (bool)dp[i][j];
            if(j==n-1 || p[j+1]!='*'){//regular
                if(s[i]==p[j] || p[j]=='.') dp[i][j]=f(i+1,j+1);
                else dp[i][j]=false;//not match
            }
            else{//c*
                if(f(i,j+2)) {dp[i][j]=true; return true;}//match 0 char
                for(int k=0; i+k<m && (s[i+k]==p[j] || p[j]=='.') ;k++)
                    if(f(i+k+1,j+2)) {dp[i][j]=true; return true;}//nextI from i to last consequtive elements from i+1 that == p[j], j jump over *
                dp[i][j]=false;
            }
            return (bool)dp[i][j];
        };
        return f(0,0);
    }
};
```

Solution old1:

```cpp
class Solution {
public:
	bool isMatch(string s, string p) {
		if (p == "") return s == "";
		vector<vector<int>> dp(s.size()+1, vector<int>(p.size()+1, -1));
		return helper(s, 0, p, 0, dp);
	}

	bool helper(string s, int si, string p, int pi, vector<vector<int>>& dp){
		if (pi == p.size()) return (si == s.size());
		if (dp[si][pi] != -1) return dp[si][pi];
		bool res = false;
		if (pi == p.size() - 1 || p[pi + 1] != '*'){
			if (si<s.size() && (p[pi] == '.' || p[pi] == s[si])){
				res = helper(s, si + 1, p, pi + 1, dp);
			}
			//else{res=false;}
		}
		else{//p[pi+1] == *
			//0th p[pi]
			if (helper(s, si, p, pi + 2, dp)) { res = true; }
			//1~kth p[pi]
			if (res != true){
				for (; si<s.size() && (p[pi] == '.' || p[pi] == s[si]); si++){
					if (helper(s, si + 1, p, pi + 2, dp)){ res = true; break; }
				}
			}
		}
		dp[si][pi] = res;
		return dp[si][pi];
	}
};
```

Solution old2:

```cpp
class Solution {
public:
	bool isMatch(string s, string p) {
		if (p.empty()) return s.empty();

		return helper(s.c_str(),p.c_str());
	}

	bool helper(const char* s, const char *p){
		if (*p == 0) return *s == 0;

		if (*(p + 1) != '*')//next not *
		{
			if (*s && (*s == *p || *p == '.'))
				return isMatch(s + 1, p + 1);
			return false;
		}
		else//*
		{
			if (isMatch(s, p + 2)) return true;//* means 0 pre element
			for (; *s && (*s == *p || *p == '.'); s++)//* means at least 1 pre element
				if (isMatch(s+1, p+2)) return true;
			return false;
		}
	}
};
//=====
```