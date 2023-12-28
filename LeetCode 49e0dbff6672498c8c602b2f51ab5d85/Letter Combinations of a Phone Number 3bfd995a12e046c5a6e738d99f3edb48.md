# Letter Combinations of a Phone Number

#: 17
Difficult: Medium
Tags: BackTracking, search
link: https://leetcode.com/problems/letter-combinations-of-a-phone-number/
Priority: Low
Created time: October 23, 2022 11:02 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75, LC_Top_Interview_150, googleFreq, leetcode_daily

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]

```

**Example 2:**

```
Input: digits = ""
Output: []

```

**Example 3:**

```
Input: digits = "2"
Output: ["a","b","c"]

```

**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.

Solution 1

```cpp
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if(digits.empty()) return vector<string>();
        
        vector<string> charmap{"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> res;
        string tmp="";
        
        helper(charmap,res,tmp,digits,0);
        
        return res;
    }
    
    void helper(vector<string>& charmap, vector<string>& res, string tmp, string digits, int idx){
        if(idx==digits.size()){res.push_back(tmp);return;}
        
        int num=digits[idx]-'0';
        string letters=charmap[num];
        for(auto c:letters){
            tmp+=c;
            helper(charmap,res,tmp,digits,idx+1);
            tmp.pop_back();
        }
    }
};
```

Solution 2

```cpp
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> res;
        if(digits=="") return res;
        string charmap[10] = {"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        res.push_back("");
        for (int i = 0; i < digits.size(); i++)
        {
            vector<string> tempres;
            string chars = charmap[digits[i] - '0'];
            for (int c = 0; c < chars.size();c++)
                for (int j = 0; j < res.size();j++)
                    tempres.push_back(res[j]+chars[c]);
            res = tempres;
        }
        return res;
    }
};
```