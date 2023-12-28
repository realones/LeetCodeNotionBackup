# Length of Last Word

#: 58
Difficult: Easy
Tags: string
link: https://leetcode.com/problems/length-of-last-word/
Priority: Pass
Created time: May 31, 2023 1:53 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given a string `s` consisting of words and spaces, return *the length of the **last** word in the string.*

A **word** is a maximal substring consisting of non-space characters only.

**Example 1:**

```
Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.

```

**Example 2:**

```
Input: s = "   fly me   to   the moon  "
Output: 4
Explanation: The last word is "moon" with length 4.

```

**Example 3:**

```
Input: s = "luffy is still joyboy"
Output: 6
Explanation: The last word is "joyboy" with length 6.

```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of only English letters and spaces `' '`.
- There will be at least one word in `s`.

```cpp
class Solution {
public:
    int lengthOfLastWord(string str) {
		int r = str.find_last_not_of(' ');
		if(string::npos==r){return 0;}
		int ll = str.find_last_of(' ', r);
		if(string::npos==ll){return r+1;}
		else{return r-ll;}

    }
};
```