# Optimal Partition of String

#: 2405
Difficult: Medium
Tags: Greedy, Hash, string
link: https://leetcode.com/problems/optimal-partition-of-string/
Priority: Low
Created time: June 27, 2023 11:14 PM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq, HuaHua

Given a string `s`, partition the string into one or more **substrings** such that the characters in each substring are **unique**. That is, no letter appears in a single substring more than **once**.

Return *the **minimum** number of substrings in such a partition.*

Note that each character should belong to exactly one substring in a partition.

**Example 1:**

```
Input: s = "abacaba"
Output: 4
Explanation:
Two possible partitions are ("a","ba","cab","a") and ("ab","a","ca","ba").
It can be shown that 4 is the minimum number of substrings needed.

```

**Example 2:**

```
Input: s = "ssssss"
Output: 6
Explanation:
The only valid partition is ("s","s","s","s","s","s").

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of only English lowercase letters.

```cpp
class Solution {
public:
    int partitionString(string s) {
        unordered_set<char> st;
        int cnt=1;
        int n=s.size();
        for(int i=0; i<n; i++){
            if(st.count(s[i])){
                cnt++;
                st.clear();
            }
            st.insert(s[i]);
        }
        return cnt;
    }
};
```

Solution 2pt:

```cpp
class Solution {
public:
    int partitionString(string s) {
        int n=s.size();//1~1e5
        int cnt=0;
        unordered_set<char> st;
        for(int l=0,r=0;r<n;r++){
            if(st.count(s[r])){
                cnt++;
                l=r;
                st={};
            }
            st.insert(s[r]);
        }
        return cnt+1;
    }
};
```