# Minimum String Length After Removing Substrings

#: 2696
Difficult: Easy
Tags: stack, string
link: https://leetcode.com/problems/minimum-string-length-after-removing-substrings/
Priority: Medium
Status: No Idea At First
Created time: May 21, 2023 10:53 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a string `s` consisting only of **uppercase** English letters.

You can apply some operations to this string where, in one operation, you can remove **any** occurrence of one of the substrings `"AB"` or `"CD"` from `s`.

Return *the **minimum** possible length of the resulting string that you can obtain*.

**Note** that the string concatenates after removing the substring and could produce new `"AB"` or `"CD"` substrings.

**Example 1:**

```
Input: s = "ABFCACDB"
Output: 2
Explanation: We can do the following operations:
- Remove the substring "ABFCACDB", so s = "FCACDB".
- Remove the substring "FCACDB", so s = "FCAB".
- Remove the substring "FCAB", so s = "FC".
So the resulting length of the string is 2.
It can be shown that it is the minimum length that we can obtain.
```

**Example 2:**

```
Input: s = "ACBBD"
Output: 5
Explanation: We cannot do any operations on the string so the length remains the same.

```

**Constraints:**

- `1 <= s.length <= 100`
- `s` consists only of uppercase English letters.

Solution:

stack

```cpp
class Solution {
public:
    int minLength(string s) {
        stack<char> stk;
        for(auto c: s){
            if(c=='B' && !stk.empty() && stk.top()=='A') stk.pop();
            else if(c=='D' && !stk.empty() && stk.top()=='C') stk.pop();
            else stk.push(c);
        }
        return stk.size();
    }
};
```

string 

```cpp
class Solution {
public:
    int minLength(string s) {
        int n=s.size(); if(n==0) {return 0;}
        int lastSz=0;
        while(lastSz!=s.size()){
            lastSz=s.size();
            
            auto itr1=s.find("CD");
            if(itr1!=string::npos) {s.erase(itr1,2);}
            
            auto itr2=s.find("AB");
            if(itr2!=string::npos) {s.erase(itr2,2);}
        }
        return s.size();
    }
};

/*
s
one op -> remove any occur of substr

min leno
AB
CD
*/
```