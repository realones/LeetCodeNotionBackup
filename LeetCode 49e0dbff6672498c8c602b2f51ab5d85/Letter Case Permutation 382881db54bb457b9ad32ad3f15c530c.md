# Letter Case Permutation

#: 784
Difficult: Medium
Tags: BackTracking, Bit Manipulation, recursion, string
link: https://leetcode.com/problems/letter-case-permutation/
Priority: High
Status: HardToImplement
Created time: June 20, 2023 8:55 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given a string `s`, you can transform every letter individually to be lowercase or uppercase to create another string.

Return *a list of all possible strings we could create*. Return the output in **any order**.

**Example 1:**

```
Input: s = "a1b2"
Output: ["a1b2","a1B2","A1b2","A1B2"]

```

**Example 2:**

```
Input: s = "3z4"
Output: ["3z4","3Z4"]

```

**Constraints:**

- `1 <= s.length <= 12`
- `s` consists of lowercase English letters, uppercase English letters, and digits.

```cpp
class Solution {
public:
    vector<string> letterCasePermutation(string s) {
        for(char& c: s){
            if('A'<=c && c<='Z') c-='A'-'a';
        }
        vector<string> res;
        helper(res,s,0);
        return res;
    }

    void helper(vector<string>& res, string& s, int start){
        res.push_back(s);

        for(int i=start;i<s.size();i++){
            if(isdigit(s[i])) continue;
            s[i]+='A'-'a';
            helper(res,s,i+1);
            s[i]-='A'-'a';
        }
    }
}
```