# Strobogrammatic Number

#: 246
Difficult: Easy
Tags: 2pt_DiffDir
link: https://leetcode.com/problems/strobogrammatic-number/
Priority: Pass
Created time: October 12, 2022 7:05 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

Given a string `num` which represents an integer, return `true` *if* `num` *is a **strobogrammatic number***.

A **strobogrammatic number** is a number that looks the same when rotated `180` degrees (looked at upside down).

**Example 1:**

```
Input: num = "69"
Output: true

```

**Example 2:**

```
Input: num = "88"
Output: true

```

**Example 3:**

```
Input: num = "962"
Output: false

```

**Constraints:**

- `1 <= num.length <= 50`
- `num` consists of only digits.
- `num` does not contain any leading zeros except for zero itself.

```cpp
class Solution {
public:
    bool isStrobogrammatic(string num) {
        vector<int> valid(10,0);
        valid[0]=0;
        valid[1]=1;
        valid[8]=8;
        valid[6]=9;
        valid[9]=6;
        //0 1 8
        //6 9
        for(int i=0,j=num.size()-1;i<=j;i++,j--){
            int l=num[i]-'0', r=num[j]-'0';
            if(valid[l]==-1 || valid[r]==-1) return false;
            if(valid[l]!=r) return false;
        }
        return true;
    }
};
```