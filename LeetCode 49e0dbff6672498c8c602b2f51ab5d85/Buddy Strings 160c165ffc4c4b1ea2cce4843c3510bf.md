# Buddy Strings

#: 859
Difficult: Easy
Tags: Hash, string
link: https://leetcode.com/problems/buddy-strings/
Priority: Pass
Created time: June 15, 2023 7:05 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given two strings `s` and `goal`, return `true` *if you can swap two letters in* `s` *so the result is equal to* `goal`*, otherwise, return* `false`*.*

Swapping letters is defined as taking two indices `i` and `j` (0-indexed) such that `i != j` and swapping the characters at `s[i]` and `s[j]`.

- For example, swapping at indices `0` and `2` in `"abcd"` results in `"cbad"`.

**Example 1:**

```
Input: s = "ab", goal = "ba"
Output: true
Explanation: You can swap s[0] = 'a' and s[1] = 'b' to get "ba", which is equal to goal.

```

**Example 2:**

```
Input: s = "ab", goal = "ab"
Output: false
Explanation: The only letters you can swap are s[0] = 'a' and s[1] = 'b', which results in "ba" != goal.

```

**Example 3:**

```
Input: s = "aa", goal = "aa"
Output: true
Explanation: You can swap s[0] = 'a' and s[1] = 'a' to get "aa", which is equal to goal.

```

**Constraints:**

- `1 <= s.length, goal.length <= 2 * 104`
- `s` and `goal` consist of lowercase letters.

```cpp
class Solution {
public:
    bool buddyStrings(string s, string goal) {
        if(s==goal){
            unordered_set<char> st(s.begin(),s.end());
            if(st.size()==s.size()) return false;
            return true;
        }
        if(s.size()!=goal.size()) return false;
        vector<int> v;
        for(int i=0;i<s.size();i++){
            if(s[i]!=goal[i]){
                v.push_back(i);
                if(v.size()>2) return false;
            }
        }
        if(v.size()!=2) return false;
        swap(s[v[0]], s[v[1]]);
        return s==goal;
    }
};
```