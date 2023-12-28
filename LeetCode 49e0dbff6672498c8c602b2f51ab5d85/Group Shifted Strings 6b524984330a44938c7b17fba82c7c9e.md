# Group Shifted Strings

#: 249
Difficult: Medium
Tags: Array, Hash, string
link: https://leetcode.com/problems/group-shifted-strings/
Priority: Low
Created time: December 2, 2023 4:20 PM
Last edited time: December 2, 2023 4:21 PM
source: facebookFreq

We can shift a string by shifting each of its letters to its successive letter.

- For example, `"abc"` can be shifted to be `"bcd"`.

We can keep shifting the string to form a sequence.

- For example, we can keep shifting `"abc"` to form the sequence: `"abc" -> "bcd" -> ... -> "xyz"`.

Given an array of strings `strings`, group all `strings[i]` that belong to the same shifting sequence. You may return the answer in **any order**.

**Example 1:**

```
Input: strings = ["abc","bcd","acef","xyz","az","ba","a","z"]
Output: [["acef"],["a","z"],["abc","bcd","xyz"],["az","ba"]]

```

**Example 2:**

```
Input: strings = ["a"]
Output: [["a"]]

```

**Constraints:**

- `1 <= strings.length <= 200`
- `1 <= strings[i].length <= 50`
- `strings[i]` consists of lowercase English letters.

```cpp
class Solution {
public:
    vector<vector<string>> groupStrings(vector<string>& strings) {
        vector<vector<string>> res;
        auto getFeature = [&](string& s)->string{
            string f="#";
            for(int i=1;i<s.size();i++){
                f+=to_string((s[i]-s[i-1]+26)%26)+"#";
            }
            return f;
        };
        unordered_map<string, vector<string>> mp;
        for(auto& s: strings){
            string feature = getFeature(s);
            mp[feature].push_back(s);
        }
        for(auto& [_,v]: mp){
            res.push_back(v);
        }
        return res;
    }
};

/*
strings:
    len: 1~200
    [i].len: 1~50
lowcase only: 26
================================
for each s:
    find: diff between each continuous two chars
        init with "#" for sz==1 handling
    mp[] += s
*/
```