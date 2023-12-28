# Palindrome Partitioning

#: 131
Difficult: Medium
Tags: BackTracking, Combination, DFS, DP
link: https://leetcode.com/problems/palindrome-partitioning/
Priority: Medium
Status: More Solution
Created time: July 28, 2022 12:37 AM
Last edited time: October 16, 2023 4:56 PM
practicedTimes: 1
source: jiuzhang, 算初DFS, 算初DP

Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of `s`.

A **palindrome** string is a string that reads the same backward as forward.

**Example 1:**

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]

```

**Example 2:**

```
Input: s = "a"
Output: [["a"]]

```

**Constraints:**

- `1 <= s.length <= 16`
- `s` contains only lowercase English letters.

back tracing, still can use DP, go dp solution

```cpp
class Solution {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> res;
        vector<string> tmp;
        int n=s.size();

        //backtracking
        function<void(string s)> backTracking = [&](string s){
            if(s=="") {res.push_back(tmp); return;}
            for(int sz=1;sz<=s.size();sz++){
                string l=s.substr(0,sz);
                string r=s.substr(sz);
                if(isPalidrome(l)){
                    tmp.push_back(l);
                    backTracking(r);
                    tmp.pop_back();
                }
            }
        };
        backTracking(s);

        return res;
    }

    bool isPalidrome(string& s){
        for(int l=0, r=s.size()-1; l<r; l++,r--){
            if(s[l]!=s[r]) return false;
        }
        return true;
    }
};

/*
backTracking:

0~i  ,  i+1~n-1

*/
```