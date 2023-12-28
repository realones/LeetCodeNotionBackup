# Count Symmetric Integers

#: 2843
Difficult: Easy
Tags: Math
link: https://leetcode.com/problems/count-symmetric-integers/description/
Priority: Pass
Created time: September 3, 2023 10:32 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given two positive integers `low` and `high`.

An integer `x` consisting of `2 * n` digits is **symmetric** if the sum of the first `n` digits of `x` is equal to the sum of the last `n` digits of `x`. Numbers with an odd number of digits are never symmetric.

Return *the **number of symmetric** integers in the range* `[low, high]`.

**Example 1:**

```
Input: low = 1, high = 100
Output: 9
Explanation: There are 9 symmetric integers between 1 and 100: 11, 22, 33, 44, 55, 66, 77, 88, and 99.

```

**Example 2:**

```
Input: low = 1200, high = 1230
Output: 4
Explanation: There are 4 symmetric integers between 1200 and 1230: 1203, 1212, 1221, and 1230.

```

**Constraints:**

- `1 <= low <= high <= 104`

```cpp
class Solution {
public:
    int countSymmetricIntegers(int low, int high) {
        int cnt=0;
        for(int x=low; x<=high; x++){
            int t=x;
            int digitCnt=0;
            while(t){
                digitCnt++;
                t/=10;
            }
            if(digitCnt&1) continue;
            t=x;
            int lsum=0;
            for(int i=0; i<digitCnt/2; i++){
                lsum+=t%10;
                t/=10;
            }
            
            int rsum=0;
            for(int i=0; i<digitCnt/2; i++){
                rsum+=t%10;
                t/=10;
            }
            
            if(lsum==rsum) {cnt++;}
        }
        return cnt;
    }
};
```