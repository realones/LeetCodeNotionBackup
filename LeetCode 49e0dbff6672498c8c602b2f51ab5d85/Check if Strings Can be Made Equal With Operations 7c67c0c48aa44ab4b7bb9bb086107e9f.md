# Check if Strings Can be Made Equal With Operations I

#: 2839
Difficult: Easy
link: https://leetcode.com/problems/check-if-strings-can-be-made-equal-with-operations-i/description/
Priority: Pass
Created time: September 8, 2023 9:23 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests
related: Check if Strings Can be Made Equal With Operations II (Check%20if%20Strings%20Can%20be%20Made%20Equal%20With%20Operations%20a38e0e695f764e20939c877d28b0b756.md)

You are given two strings `s1` and `s2`, both of length `4`, consisting of **lowercase** English letters.

You can apply the following operation on any of the two strings **any** number of times:

- Choose any two indices `i` and `j` such that `j - i = 2`, then **swap** the two characters at those indices in the string.

Return `true` *if you can make the strings* `s1` *and* `s2` *equal, and* `false` *otherwise*.

**Example 1:**

```
Input: s1 = "abcd", s2 = "cdab"
Output: true
Explanation: We can do the following operations on s1:
- Choose the indices i = 0, j = 2. The resulting string is s1 = "cbad".
- Choose the indices i = 1, j = 3. The resulting string is s1 = "cdab" = s2.

```

**Example 2:**

```
Input: s1 = "abcd", s2 = "dacb"
Output: false
Explanation: It is not possible to make the two strings equal.
```

```cpp
class Solution {
public:
    bool canBeEqual(string s1, string s2) {
        unordered_map<char,int> freq;
        for(int i=0; i<4; i+=2){
            freq[s1[i]]++;
            freq[s2[i]]--;
        }
        for(auto [_,f]: freq){
            if(f!=0) return false;
        }
        
        for(int i=1; i<4; i+=2){
            freq[s1[i]]++;
            freq[s2[i]]--;
        }
        for(auto [_,f]: freq){
            if(f!=0) return false;
        }
        
        return true;
    }
};

/*
==2

*/
```