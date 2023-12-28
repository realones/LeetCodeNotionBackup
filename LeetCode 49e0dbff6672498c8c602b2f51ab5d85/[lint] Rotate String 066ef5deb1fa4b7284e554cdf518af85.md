# [lint] Rotate String

#: 8
Difficult: Easy
Tags: 3 Step Rotate
link: https://www.lintcode.com/problem/rotate-string
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 19, 2023 9:29 PM
practicedTimes: 1
source: jiuzhang, 算初BS, 算初TwoPointers

Given a string(Given in the way of char array) and an offset, rotate the string by offset `in place`. (rotate from left to right).

Example:
**Example 1:**

```
Input: str="abcdefg", offset = 3
Output: str = "efgabcd"
Explanation: Note that it is rotated in place, that is, after str is rotated, it becomes "efgabcd".

```

**Example 2:**

```
Input: str="abcdefg", offset = 0
Output: str = "abcdefg"
Explanation: Note that it is rotated in place, that is, after str is rotated, it becomes "abcdefg".

```

**Example 3:**

```
Input: str="abcdefg", offset = 1
Output: str = "gabcdef"
Explanation: Note that it is rotated in place, that is, after str is rotated, it becomes "gabcdef".

```

**Example 4:**

```
Input: str="abcdefg", offset =2
Output: str = "fgabcde"
Explanation: Note that it is rotated in place, that is, after str is rotated, it becomes "fgabcde".

```

**Example 5:**

```
Input: str="abcdefg", offset = 10
Output: str = "efgabcd"
Explanation: Note that it is rotated in place, that is, after str is rotated, it becomes "efgabcd".

```

Challenge
Rotate in-place with O(1) extra memory.

Related Questions:
rotate-string-ii,groups-of-special-equivalent-strings,rotate-words,substring-rotation,rotate-list,rotate-image,recover-rotated-sorted-array

```cpp
class Solution {
public:
    /*
     * @param str: An array of char
     * @param offset: An integer
     * @return: nothing
     */
    void rotateString(string &str, int offset) {
        if(str.size()<=1) return;
        offset=offset%str.size();

        reverse(str.begin(),str.end());
        reverse(str.begin(),str.begin()+offset);
        reverse(str.begin()+offset,str.end());
    }
};
```