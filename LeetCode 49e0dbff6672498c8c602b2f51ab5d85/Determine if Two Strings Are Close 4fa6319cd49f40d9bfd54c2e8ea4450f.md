# Determine if Two Strings Are Close

#: 1657
Difficult: Medium
Tags: Hash, Sort, string
link: https://leetcode.com/problems/determine-if-two-strings-are-close
Priority: Low
Status: More Solution
Created time: July 7, 2023 4:25 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75
related: Check If String Is Transformable With Substring Sort Operations (Check%20If%20String%20Is%20Transformable%20With%20Substring%20So%205cc2f9960e3540f98ed1648756dd9c73.md)

Two strings are considered **close** if you can attain one from the other using the following operations:

- Operation 1: Swap any two **existing** characters.
    - For example, `abcde -> aecdb`
- Operation 2: Transform **every** occurrence of one **existing** character into another **existing** character, and do the same with the other character.
    - For example, `aacabb -> bbcbaa` (all `a`'s turn into `b`'s, and all `b`'s turn into `a`'s)

You can use the operations on either string as many times as necessary.

Given two strings, `word1` and `word2`, return `true` *if* `word1` *and* `word2` *are **close**, and* `false` *otherwise.*

**Example 1:**

```
Input: word1 = "abc", word2 = "bca"
Output: true
Explanation: You can attain word2 from word1 in 2 operations.
Apply Operation 1: "abc" -> "acb"
Apply Operation 1: "acb" -> "bca"

```

**Example 2:**

```
Input: word1 = "a", word2 = "aa"
Output: false
Explanation:It is impossible to attain word2 from word1, or vice versa, in any number of operations.

```

**Example 3:**

```
Input: word1 = "cabbba", word2 = "abbccc"
Output: true
Explanation: You can attain word2 from word1 in 3 operations.
Apply Operation 1: "cabbba" -> "caabbb"
Apply Operation 2: "caabbb" -> "baaccc"
Apply Operation 2: "baaccc" -> "abbccc"

```

**Constraints:**

- `1 <= word1.length, word2.length <= 105`
- `word1` and `word2` contain only lowercase English letters.

```cpp
class Solution {
public:
    bool closeStrings(string word1, string word2) {
        if(word1.size()!=word2.size()) return false;
        //check char set
        unordered_set<char> st;
        for(auto c: word1){st.insert(c);}
        for(auto c: word2){st.erase(c);}
        if(!st.empty()) return false;

        //check freq
        unordered_map<char,int> mp1;//word1 - {char, freq}
        unordered_map<char,int> mp2;//word2 - {char, freq}
        unordered_map<int,int> mp; //{freq, cnt}, word1 freq++, word2 freq--
        for(auto c: word1){ mp1[c]++; }
        for(auto c: word2){ mp2[c]++; }
        for(auto [_,f]: mp1){ mp[f]++; }
        for(auto [_,f]: mp2){ mp[f]--; }
        for(auto [_,cnt]: mp){
            if(cnt!=0) return false;
        }
        return true;
    }
};

/*
seems as long as two strings has following pattern
str1:
    c1 - freq1
    c2 - freq2

str2: has same group of chars with same freqs, then it is true

str1 and str2 should have same set of chars
*/
```