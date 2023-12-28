# Reverse Vowels of a String

#: 345
Difficult: Easy
Tags: 2pt_DiffDir, Partition, string
link: https://leetcode.com/problems/reverse-vowels-of-a-string
Priority: Medium
Created time: July 4, 2023 6:43 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

Given a string `s`, reverse only all the vowels in the string and return it.

The vowels are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`, and they can appear in both lower and upper cases, more than once.

**Example 1:**

```
Input: s = "hello"
Output: "holle"

```

**Example 2:**

```
Input: s = "leetcode"
Output: "leotcede"

```

**Constraints:**

- `1 <= s.length <= 3 * 105`
- `s` consist of **printable ASCII** characters.

```cpp
class Solution {
public:
    string reverseVowels(string s) {
        int l=0,r=s.size()-1;
        while(l<=r){
            while(l<=r && !isVowel(s[l])) l++;
            while(l<=r && !isVowel(s[r])) r--;
            if(l<=r){
                swap(s[l],s[r]);
                l++,r--;
            }
        }
        return s;
    }
    
    bool isVowel(char c){
        return c=='a'||c=='e'||c=='i'||c=='o'||c=='u'||c=='A'||c=='E'||c=='I'||c=='O'||c=='U';
    }
};
```