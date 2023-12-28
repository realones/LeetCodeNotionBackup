# Group Anagrams

#: 49
Difficult: Medium
Tags: Anagram, Hash
link: https://leetcode.com/problems/group-anagrams/
Priority: Medium
Created time: August 11, 2022 7:19 PM
Last edited time: October 21, 2023 1:02 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算初Hash&Heap

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]

```

**Example 2:**

```
Input: strs = [""]
Output: [[""]]

```

**Example 3:**

```
Input: strs = ["a"]
Output: [["a"]]

```

**Constraints:**

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters.

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> res;
        if(strs.empty()) return res;
        unordered_map<string, vector<string>> mp;//key_strs_map
        for(auto s: strs){
            string key=getKey(s);
            mp[key].push_back(s);
        }
        
        for(auto p: mp){
            res.push_back(p.second);
        }
        return res;
    }
    
    string getKey(string s){
        string key(26,'0');
        for(auto c:s){key[c-'a']++;}
        return key;
    }
};
```