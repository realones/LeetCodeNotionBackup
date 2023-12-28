# Length of the Longest Valid Substring

#: 2781
Difficult: Hard
Tags: Array, Hash, Sliding Window, string
link: https://leetcode.com/problems/length-of-the-longest-valid-substring/
Priority: High
Created time: October 10, 2023 4:17 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given a string `word` and an array of strings `forbidden`.

A string is called **valid** if none of its substrings are present in `forbidden`.

Return *the length of the **longest valid substring** of the string* `word`.

A **substring** is a contiguous sequence of characters in a string, possibly empty.

**Example 1:**

```
Input: word = "cbaaaabc", forbidden = ["aaa","cb"]
Output: 4
Explanation: There are 11 valid substrings in word: "c", "b", "a", "ba", "aa", "bc", "baa", "aab", "ab", "abc" and "aabc". The length of the longest valid substring is 4.
It can be shown that all other substrings contain either "aaa" or "cb" as a substring.
```

**Example 2:**

```
Input: word = "leetcode", forbidden = ["de","le","e"]
Output: 4
Explanation: There are 11 valid substrings in word: "l", "t", "c", "o", "d", "tc", "co", "od", "tco", "cod", and "tcod". The length of the longest valid substring is 4.
It can be shown that all other substrings contain either "de", "le", or "e" as a substring.

```

**Constraints:**

- `1 <= word.length <= 105`
- `word` consists only of lowercase English letters.
- `1 <= forbidden.length <= 105`
- `1 <= forbidden[i].length <= 10`
- `forbidden[i]` consists only of lowercase English letters.

Solution TLE:

```cpp
class Solution {
public:
    int longestValidSubstring(string word, vector<string>& forbidden) {
        int n=word.size();
        
        function valid = [&](int l, int r)->bool {
            for(auto& w: forbidden){
                if(w.size()> r-l+1) continue;
                if(word.substr(r-w.size()+1, w.size()) == w) return false;
            }
            return true;
        };
        
        int res=0;
        for(int l=0,r=0;r<n;r++){
            while(!valid(l,r)) l++;
            res=max(res,r-l+1);
        }
        return res;
    }
};
```

Solution good:

```cpp
class Solution {
public:
    int longestValidSubstring(string word, vector<string>& forbidden) {
        int n=word.size();
        unordered_set<string> st(forbidden.begin(), forbidden.end());
        
        function valid = [&](int l, int r)->bool {
            for(int sz=1;sz<=min(10,r-l+1);sz++){
                if(st.count(word.substr(r-sz+1, sz))) return false;
            }
            return true;
        };
        
        int res=0;
        for(int l=0,r=0;r<n;r++){
            while(!valid(l,r)) l++;
            res=max(res,r-l+1);
        }
        return res;
    }
};
```