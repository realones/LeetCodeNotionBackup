# Word Break II

#: 140
Difficult: Hard
Tags: Array, BackTracking, DP, Hash, Trie, string
link: https://leetcode.com/problems/word-break-ii/description/
Priority: Medium
Created time: June 28, 2023 12:55 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given a string `s` and a dictionary of strings `wordDict`, add spaces in `s` to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in **any order**.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

```
Input: s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
Output: ["cats and dog","cat sand dog"]

```

**Example 2:**

```
Input: s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
Output: ["pine apple pen apple","pineapple pen apple","pine applepen apple"]
Explanation: Note that you are allowed to reuse a dictionary word.

```

**Example 3:**

```
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: []

```

**Constraints:**

- `1 <= s.length <= 20`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 10`
- `s` and `wordDict[i]` consist of only lowercase English letters.
- All the strings of `wordDict` are **unique**.
- Input is generated in a way that the length of the answer doesn't exceed 1e5

```cpp
class Solution {
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(),wordDict.end());
        unordered_map<string,vector<string>> mp;//dp
        //can't dfs with back tracing, since use memorization
        helper(dict,mp,s);
        
        return mp[s];
    }
    vector<string> helper(unordered_set<string>& dict, unordered_map<string,vector<string>>& mp, string s){
        if(mp.count(s)){return mp[s];}
        if(s=="") return vector<string>{""};
        
        vector<string> res;
        for(int sz=1;sz<=s.size();sz++){
            string prefix=s.substr(0,sz),postfix=s.substr(sz);
            if(dict.count(prefix)){
                vector<string> nexts=helper(dict,mp,postfix);
                if(nexts.empty()) continue;
                for(auto next:nexts){
                    string t = (next!="")? prefix+" "+next : prefix;
                    res.push_back(t);
                }
            }
        }
        
        mp[s]=res;
        return mp[s];
    }
};
```