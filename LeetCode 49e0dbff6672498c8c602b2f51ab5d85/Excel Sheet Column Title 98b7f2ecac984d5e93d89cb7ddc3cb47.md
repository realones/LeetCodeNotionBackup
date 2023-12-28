# Excel Sheet Column Title

#: 168
Difficult: Easy
Tags: Math
link: https://leetcode.com/problems/excel-sheet-column-title/description/
Priority: High
Status: Dig More, No Idea At First
Created time: August 21, 2023 10:18 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an integer `columnNumber`, return *its corresponding column title as it appears in an Excel sheet*.

For example:

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28
...

```

**Example 1:**

```
Input: columnNumber = 1
Output: "A"

```

**Example 2:**

```
Input: columnNumber = 28
Output: "AB"

```

**Example 3:**

```
Input: columnNumber = 701
Output: "ZY"

```

**Constraints:**

- `1 <= columnNumber <= 231 - 1`

Solution 1:

26based, 

issue: there is no zero

solve: 

map 0 to @==A-1

map 1 to A

map 2 to B

treat Z = A@

after generate string, then for str[idx] == @

str[idx]=Z, above char --, recursive handle all above chars

```cpp
class Solution {
public:
    string convertToTitle(int n) {
        string tmp="";
        for(;n;n/=26){
            int t=n%26;
            char c='@'+t;
            tmp=c+tmp;
        }
        
        string res;
        for(auto c: tmp){
            res.push_back(c);
            for(int i=res.size()-1; i>=0; i--){
                if(res[i]=='@'){
                    if(i==0) res.erase(res.begin(),res.begin()+1);
                    else{
                        res[i]='Z';
                        res[i-1]--;
                    }
                }
                else break;
            }
        }
        return res;
    }
};

/*
0      @ = A-1
1      A
2      B
...
25     Y
26     A#
26+1   AA
26+25  AY
26+26  B#
*/
```

Solution 2:

from Editorial, not quite understand

```cpp
class Solution {
public:
    string convertToTitle(int n) {
        string res="";
        for(;n;n/=26){
            int t=(--n)%26;
            res=char('A'+t)+res;
        }
        return res;
    }
};
```