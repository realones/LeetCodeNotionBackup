# Find the Index of the First Occurrence in a String

#: 28
Difficult: Easy
Tags: 2pt, string
link: https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/
Priority: Low
Status: More Solution
Created time: May 31, 2023 3:53 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

**Example 1:**

```
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.

```

**Example 2:**

```
Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.

```

**Constraints:**

- `1 <= haystack.length, needle.length <= 104`
- `haystack` and `needle` consist of only lowercase English characters.

Solution :

```cpp
class Solution {
public:
    int strStr(string source, string  target) {
        int i, j, lenh = source.length(), lenn =  target.length();
        if (lenn == 0)  return 0;
        for (i = 0; i <= lenh - lenn; i++) {
            for (j = 0; j < lenn; j++) 
                if (source[i + j] !=  target[j]) break;
            // 匹配成功
            if (j == lenn)  return i;
        }
        return -1;
    }
};
```

Solution 2; KMP

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.length(), n = needle.length();
        if (!n) return 0;
        vector<int> lps = kmpProcess(needle);
        for (int i = 0, j = 0; i < m; ) {
            if (haystack[i] == needle[j]) { 
                i++;
                j++;
            }
            else{
                if (j>0) j = lps[j - 1];
                else i++;//take care
            }
            
            if (j == n) return i - j;
        }
        return -1;
    }
private:
    vector<int> kmpProcess(string& needle) {
        int n = needle.length();
        vector<int> lps(n, 0);
        for (int i = 1, len = 0; i < n; ) {
            int nextCompare=len-1+1;
            if (needle[i] == needle[nextCompare]){
                len++;
                lps[i] = len;
                i++;
            }
            else if (len>0){
                int lastSameNumForPre=len-1;
                len = lps[lastSameNumForPre];
            }
            else{
                lps[i] = 0;//take care
                i++;
            }
        }
        return lps;
    }
};
```