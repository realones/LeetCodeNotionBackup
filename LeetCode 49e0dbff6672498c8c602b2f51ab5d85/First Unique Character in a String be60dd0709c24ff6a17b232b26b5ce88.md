# First Unique Character in a String

#: 387
Difficult: Easy
Tags: Hash
link: https://leetcode.com/problems/first-unique-character-in-a-string/
Priority: Pass
Created time: October 16, 2022 5:48 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

Given a string `s`, *find the first non-repeating character in it and return its index*. If it does not exist, return `-1`.

**Example 1:**

```
Input: s = "leetcode"
Output: 0

```

**Example 2:**

```
Input: s = "loveleetcode"
Output: 2

```

**Example 3:**

```
Input: s = "aabb"
Output: -1

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of only lowercase English letters.

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char, int> mp;

        for(int i=0; i<s.length(); i++)
            mp[s[i]]++;

        for(int i=0; i<s.length(); i++){
            if(mp[s[i]]==1)
                return i;
        }

        return -1;
    }
};
```