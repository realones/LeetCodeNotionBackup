# Reverse Only Letters

#: 917
Difficult: Easy
Tags: 2pt_DiffDir, string
link: https://leetcode.com/problems/reverse-only-letters/description/
Priority: Pass
Created time: August 17, 2023 12:50 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given a string `s`, reverse the string according to the following rules:

- All the characters that are not English letters remain in the same position.
- All the English letters (lowercase or uppercase) should be reversed.

Return `s` *after reversing it*.

**Example 1:**

```
Input: s = "ab-cd"
Output: "dc-ba"

```

**Example 2:**

```
Input: s = "a-bC-dEf-ghIj"
Output: "j-Ih-gfE-dCba"

```

**Example 3:**

```
Input: s = "Test1ng-Leet=code-Q!"
Output: "Qedo1ct-eeLg=ntse-T!"

```

**Constraints:**

- `1 <= s.length <= 100`
- `s` consists of characters with ASCII values in the range `[33, 122]`.
- `s` does not contain `'\"'` or `'\\'`.

```cpp
class Solution {
public:
    string reverseOnlyLetters(string s) {
        int n=s.size();
        int l=0,r=n-1;
        while(l<r){
            while(l<r && !isalpha(s[l])) l++;
            while(l<r && !isalpha(s[r])) r--;
            swap(s[l++], s[r--]);
        }
        return s; 
    }
}
```