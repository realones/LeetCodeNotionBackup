# Check if Strings Can be Made Equal With Operations II

#: 2840
Difficult: Medium
link: https://leetcode.com/problems/check-if-strings-can-be-made-equal-with-operations-ii/description/
Priority: Low
Created time: September 8, 2023 9:24 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests
related: Check if Strings Can be Made Equal With Operations I (Check%20if%20Strings%20Can%20be%20Made%20Equal%20With%20Operations%207c67c0c48aa44ab4b7bb9bb086107e9f.md)

You are given two strings `s1` and `s2`, both of length `n`, consisting of **lowercase** English letters.

You can apply the following operation on **any** of the two strings **any** number of times:

- Choose any two indices `i` and `j` such that `i < j` and the difference `j - i` is **even**, then **swap** the two characters at those indices in the string.

Return `true` *if you can make the strings* `s1` *and* `s2` *equal, and* `false` *otherwise*.

**Example 1:**

```
Input: s1 = "abcdba", s2 = "cabdab"
Output: true
Explanation: We can apply the following operations on s1:
- Choose the indices i = 0, j = 2. The resulting string is s1 = "cbadba".
- Choose the indices i = 2, j = 4. The resulting string is s1 = "cbbdaa".
- Choose the indices i = 1, j = 5. The resulting string is s1 = "cabdab" = s2.

```

**Example 2:**

```
Input: s1 = "abe", s2 = "bea"
Output: false
Explanation: It is not possible to make the two strings equal.

```

**Constraints:**

- `n == s1.length == s2.length`
- `1 <= n <= 105`
- `s1` and `s2` consist only of lowercase English letters.

```cpp
class Solution {
public:
    bool checkStrings(string s1, string s2) {
        int n=s1.size();
        unordered_map<char,int> freq;
        for(int i=0; i<n; i+=2){
            freq[s1[i]]++;
            freq[s2[i]]--;
        }
        for(auto [_,f]: freq){
            if(f!=0) return false;
        }
        
        for(int i=1; i<n; i+=2){
            freq[s1[i]]++;
            freq[s2[i]]--;
        }
        for(auto [_,f]: freq){
            if(f!=0) return false;
        }
        
        return true;
    }
};
```