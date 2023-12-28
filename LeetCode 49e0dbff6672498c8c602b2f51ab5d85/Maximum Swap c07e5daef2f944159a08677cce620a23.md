# Maximum Swap

#: 670
Difficult: Medium
Tags: Greedy, Math, string
link: https://leetcode.com/problems/maximum-swap/description/
Priority: Medium
Created time: December 3, 2023 1:42 AM
Last edited time: December 3, 2023 1:43 AM
source: facebookFreq

You are given an integer `num`. You can swap two digits at most once to get the maximum valued number.

Return *the maximum valued number you can get*.

**Example 1:**

```
Input: num = 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.

```

**Example 2:**

```
Input: num = 9973
Output: 9973
Explanation: No swap.

```

**Constraints:**

- `0 <= num <= 108`

```cpp
class Solution {
public:
    int maximumSwap(int num) {
        string s=to_string(num);
        int maxVal=-1, maxIdx=-1;
        string res=s;
        for(int i=s.size()-1;i>=0;i--){
            if(s[i]<maxVal){
                swap(s[i], s[maxIdx]);
                res=s;
                swap(s[i], s[maxIdx]);
            }
            if(s[i]>maxVal){
                maxVal=s[i];
                maxIdx=i;
            }
        }
        return stoi(res);
    }
};
```