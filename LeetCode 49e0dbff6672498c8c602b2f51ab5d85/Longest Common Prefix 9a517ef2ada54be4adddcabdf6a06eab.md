# Longest Common Prefix

#: 14
Difficult: Easy
Tags: string
link: https://leetcode.com/problems/longest-common-prefix/
Priority: Low
Created time: May 31, 2023 2:08 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: strs = ["flower","flow","flight"]
Output: "fl"

```

**Example 2:**

```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.

```

**Constraints:**

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` consists of only lowercase English letters.

Solution 1:

```cpp
class Solution {
    public String longestCommonPrefix(String[] strs) {
    if (strs.length == 0) return "";
    String prefix = strs[0];
    for (int i = 1; i < strs.length; i++)
        while (strs[i].indexOf(prefix) != 0) {
            prefix = prefix.substring(0, prefix.length() - 1);
            if (prefix.isEmpty()) return "";
        }        
    return prefix;
}
}
```

Solution 2:

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string prefix="";
        for(int i=0; 1; i++){
            char c=0;
            for(auto s: strs){
                if(i>(int)s.size()-1) return prefix;
                if(c==0) c=s[i];
                else{
                    if(c!=s[i]) return prefix;
                }
            }
            prefix+=c;
        }
        return prefix;
    }
};
```