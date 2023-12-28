# Plus One

#: 66
Difficult: Easy
Tags: Math
link: https://leetcode.com/problems/plus-one/
Priority: Pass
Created time: June 6, 2023 10:55 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

You are given a **large integer** represented as an integer array `digits`, where each `digits[i]` is the `ith` digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading `0`'s.

Increment the large integer by one and return *the resulting array of digits*.

**Example 1:**

```
Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].

```

**Example 2:**

```
Input: digits = [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
Incrementing by one gives 4321 + 1 = 4322.
Thus, the result should be [4,3,2,2].

```

**Example 3:**

```
Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].

```

**Constraints:**

- `1 <= digits.length <= 100`
- `0 <= digits[i] <= 9`
- `digits` does not contain any leading `0`'s.

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int n=digits.size();
        int inc=1;
        for(int i=n-1;i>=0;i--){
            digits[i]+=inc;
            inc=(digits[i]/10);
            digits[i]=digits[i]%10;
        }
        if(inc==1) digits.insert(digits.begin(),1);
        return digits;
    }
};
```