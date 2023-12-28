# Reorganize String

#: 767
Difficult: Medium
Tags: Greedy, Hash, PQ, Sort, string
link: https://leetcode.com/problems/reorganize-string/description/
Priority: Medium
Status: More Solution
Created time: August 24, 2023 6:46 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily
related: Rearrange String k Distance Apart (Rearrange%20String%20k%20Distance%20Apart%20bb10923334944eea8d6b31d7d6ce6566.md)

Given a string `s`, rearrange the characters of `s` so that any two adjacent characters are not the same.

Return *any possible rearrangement of* `s` *or return* `""` *if not possible*.

**Example 1:**

```
Input: s = "aab"
Output: "aba"

```

**Example 2:**

```
Input: s = "aaab"
Output: ""

```

**Constraints:**

- `1 <= s.length <= 500`
- `s` consists of lowercase English letters.

comment:

直接想出了greedy solution，应该有别的solution，没去想

Solution Greedy

intui, 只考虑最多的char能不能放的开就行

```cpp
class Solution {
public:
    string reorganizeString(string s) {
        int n=s.size();
        unordered_map<char,int> freq;
        for(char c: s){ freq[c]++; }
        int maxCnt=0;
        for(auto [c,cnt]: freq){
            maxCnt=max(maxCnt,cnt);
        }
        if(maxCnt>(n/2+n%2)) return "";

        map<int,unordered_set<char>> cnt_char;
        for(auto [c,cnt]: freq){
            cnt_char[cnt].insert(c);
        }
        string res(s);
        int idx=0;
        for(auto itr=cnt_char.rbegin(); itr!=cnt_char.rend(); itr++){
            auto& [cnt, chars]=*itr;
            for(char c: chars){
                for(int i=0; i<cnt; i++){
                    res[idx]=c;
                    idx+=2;
                    if(idx>=n) idx=1;
                }
            }
        }
        return res;
    }
};
```