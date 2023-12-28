# Backspace String Compare

#: 844
Difficult: Easy
Tags: 2pt, simulation, stack, string
link: https://leetcode.com/problems/backspace-string-compare
Priority: High
Status: HardToImplement, More Solution, No Idea At First
Created time: October 18, 2023 10:11 PM
Last edited time: October 19, 2023 12:59 PM
source: leetcode_daily

Given two strings `s` and `t`, return `true` *if they are equal when both are typed into empty text editors*. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

**Example 1:**

```
Input: s = "ab#c", t = "ad#c"
Output: true
Explanation: Both s and t become "ac".

```

**Example 2:**

```
Input: s = "ab##", t = "c#d#"
Output: true
Explanation: Both s and t become "".

```

**Example 3:**

```
Input: s = "a#c", t = "b"
Output: false
Explanation: s becomes "c" while t becomes "b".

```

**Constraints:**

- `1 <= s.length, t.length <= 200`
- `s` and `t` only contain lowercase letters and `'#'` characters.

**Follow up:** Can you solve it in `O(n)` time and `O(1)` space?

Solution: follow up

```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        int i=s.size()-1,j=t.size()-1;
        while(i>=0 || j>=0){
            //get nxt i
            int cnt1=0;
            while(i>=0){
                if(s[i]=='#') cnt1++, i--;
                else{
                    if(cnt1>0) cnt1--, i--;
                    else break;
                }
            }
            //get nxt j
            int cnt2=0;
            while(j>=0){
                if(t[j]=='#') cnt2++ ,j--;
                else{
                    if(cnt2>0) cnt2--, j--;
                    else break;
                }
            }
            //compare
            if(i>=0 && j>=0){
                if(s[i]!=t[j]) return false;
            }
            else if(i>=0) return false;
            else if(j>=0) return false;
            i--,j--;
        }
        return true;
    }
};
```

Solution: no follow up:

```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        string ss;
        for(auto c:s){
            if(c=='#'){
                if(ss.size()>0) ss.pop_back();
            }
            else ss+=c;
        }
        string tt;
        for(auto c:t){
            if(c=='#'){
                if(tt.size()>0) tt.pop_back();
            }
            else tt+=c;
        }
        return ss==tt;
    }
};
```