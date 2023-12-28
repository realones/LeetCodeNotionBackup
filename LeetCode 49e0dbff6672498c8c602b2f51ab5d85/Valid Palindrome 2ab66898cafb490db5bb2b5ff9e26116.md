# Valid Palindrome

#: 125
Difficult: Easy
Tags: 2pt_DiffDir
link: https://leetcode.com/problems/valid-palindrome/
Priority: Pass
Created time: August 3, 2022 1:37 PM
Last edited time: October 19, 2023 5:53 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初TwoPointers

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` *if it is a **palindrome**, or* `false` *otherwise*.

**Example 1:**

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.

```

**Example 2:**

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.

```

**Example 3:**

```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.

```

**Constraints:**

- `1 <= s.length <= 2 * 105`
- `s` consists only of printable ASCII characters.

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int l=0,r=s.size()-1;
        while(l<r){
            if(!isalnum(s[l])) {l++; continue;}
            if(!isalnum(s[r])) {r--; continue;}
            
            if(toupper(s[l])!=toupper(s[r])){return false;}
            l++,r--;
        }
        return true;
    }
};
```