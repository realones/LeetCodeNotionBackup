# Unique Length-3 Palindromic Subsequences

#: 1930
Difficult: Medium
Tags: Hash, Prefix, string
link: https://leetcode.com/problems/unique-length-3-palindromic-subsequences
Priority: High
Status: No Idea At First
Created time: November 13, 2023 4:42 PM
Last edited time: November 13, 2023 4:43 PM
source: leetcode_daily

Given a string `s`, return *the number of **unique palindromes of length three** that are a **subsequence** of* `s`.

Note that even if there are multiple ways to obtain the same subsequence, it is still only counted **once**.

A **palindrome** is a string that reads the same forwards and backwards.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

**Example 1:**

```
Input: s = "aabca"
Output: 3
Explanation: The 3 palindromic subsequences of length 3 are:
- "aba" (subsequence of "aabca")
- "aaa" (subsequence of "aabca")
- "aca" (subsequence of "aabca")

```

**Example 2:**

```
Input: s = "adc"
Output: 0
Explanation: There are no palindromic subsequences of length 3 in "adc".

```

**Example 3:**

```
Input: s = "bbcbaba"
Output: 4
Explanation: The 4 palindromic subsequences of length 3 are:
- "bbb" (subsequence of "bbcbaba")
- "bcb" (subsequence of "bbcbaba")
- "bab" (subsequence of "bbcbaba")
- "aba" (subsequence of "bbcbaba")

```

**Constraints:**

- `3 <= s.length <= 105`
- `s` consists of only lowercase English letters.

```cpp
class Solution {
public:
    int countPalindromicSubsequence(string s) {
        int n=s.size();//3~1e5
        //preprocess first occur and last occur for each
        vector<int> first(256,-1);
        vector<int> last(256,-1);
        for(int i=0; i<n; i++){
            char c=s[i];
            if(first[c]==-1) first[c]=i;
            last[c]=i;
        }
        //process each letter
        int res=0;
        for(char c='a'; c<='z'; c++){
            if(first[c]==-1 || last[c]-first[c]<=1) continue;
            int l=first[c]+1, r=last[c]-1;
            unordered_set<char> st;
            for(int i=l;i<=r;i++){
                st.insert(s[i]);
            }
            res+=st.size();
        }
        return res;
    }
};

/*
lowerCase only -> 26
*/
```