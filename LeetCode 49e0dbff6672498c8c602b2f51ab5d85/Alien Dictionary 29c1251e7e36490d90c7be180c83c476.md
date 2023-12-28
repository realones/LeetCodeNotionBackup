# Alien Dictionary

#: 269
Difficult: Hard
Tags: Array, BFS, DFS, Graph, TopSorting, string
link: https://leetcode.com/problems/alien-dictionary/description/
Priority: High
Status: HardToImplement
Created time: December 4, 2023 11:05 PM
Last edited time: December 4, 2023 11:06 PM
source: facebookFreq

There is a new alien language that uses the English alphabet. However, the order of the letters is unknown to you.

You are given a list of strings `words` from the alien language's dictionary. Now it is claimed that the strings in `words` are

**sorted lexicographically**

by the rules of this new language.

If this claim is incorrect, and the given arrangement of string in `words` cannot correspond to any order of letters, return `"".`

Otherwise, return *a string of the unique letters in the new alien language sorted in **lexicographically increasing order** by the new language's rules.* If there are multiple solutions, return ***any of them***.

**Example 1:**

```
Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"

```

**Example 2:**

```
Input: words = ["z","x"]
Output: "zx"

```

**Example 3:**

```
Input: words = ["z","x","z"]
Output: ""
Explanation: The order is invalid, so return"".

```

**Constraints:**

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` consists of only lowercase English letters.

comment: 挺简单的一道题，却花了额外的时间。

```cpp
class Solution {
public:
    string alienOrder(vector<string>& words) {
        unordered_map<char,unordered_set<char>> g;//todo: use vv instead

        //return if possible
        auto getDependencies = [&](string& s1, string& s2)->bool{
            int len=min(s1.size(), s2.size());
            for(int i=0; i<len; i++){
                if(s1[i]==s2[i]) continue;
                else {
                    g[s1[i]].insert(s2[i]);
                    return true;
                }
            }
            if(s1.size()>s2.size()) return false;
            return true;
        };

        for(int i=1;i<words.size();i++){
            string& w1=words[i-1];
            string& w2=words[i];
            if(!getDependencies(w1,w2)) return "";
        }
        
        unordered_map<char, int> ind;
        for(auto& [u, st]: g){
            for(auto v: st){
                ind[v]++;
            }
        }

        unordered_set<char> dict;
        for(auto& w: words){
            for(auto c: w){
                dict.insert(c);
            }
        }
        queue<char> q;
        for(auto c: dict){
            if(ind[c]==0) q.push(c);
        }

        string res;
        while(!q.empty()){
            char u=q.front(); q.pop();
            res+=u;
            for(auto v: g[u]){
                ind[v]--;
                if(ind[v]==0) q.push(v);
            }
        }
        if(res.size()<dict.size()) return "";
        return res;
    }
};

/*
topSort - try to find 
1. if valid or not
2. give any correct seq
*/
```