# Strobogrammatic Number II

#: 247
Difficult: Medium
Tags: Array, BackTracking, recursion, string
link: https://leetcode.com/problems/strobogrammatic-number-ii/description/
Priority: Medium
Status: toSummarize
Created time: December 24, 2023 1:17 AM
Last edited time: December 24, 2023 1:19 AM
source: googleFreq

Given an integer `n`, return all the **strobogrammatic numbers** that are of length `n`. You may return the answer in **any order**.

A **strobogrammatic number** is a number that looks the same when rotated `180` degrees (looked at upside down).

**Example 1:**

```
Input: n = 2
Output: ["11","69","88","96"]

```

**Example 2:**

```
Input: n = 1
Output: ["0","1","8"]

```

**Constraints:**

- `1 <= n <= 14`

toSummarize:

the recursion way to find combination and permutation, actually it is my first thought for combination and permutation instead of using backtracking, need to analyze the complexity between them, this one I always think worse than backtracking because the Space complexity is hight.

```cpp
//cp from Editorial - solution 1
class Solution {
public:
    vector<vector<char>> reversiblePairs = {
        {'0', '0'},
        {'1', '1'}, 
        {'6', '9'},
        {'8', '8'},
        {'9', '6'}
    };
    
    vector<string> findStrobogrammatic(int n) {
        return generateStroboNumbers(n, n);
    }
    
    vector<string> generateStroboNumbers(int n, int finalLength) {
        // 0-digit strobogrammatic number is an empty string.
        if (n == 0) return { "" };
        // 1-digit strobogrammatic numbers.
        if (n == 1)  return { "0", "1", "8" };
        
        vector<string> prevStroboNums = generateStroboNumbers(n - 2, finalLength);
        vector<string> currStroboNums;
        
        for (string& prevStroboNum : prevStroboNums) {
            for (vector<char>& pair : reversiblePairs) {
                // We can only append 0's if it is not first digit.
                if (pair[0] != '0' || n != finalLength) {
                    currStroboNums.push_back(pair[0] + prevStroboNum + pair[1]);
                }
            }
        }
        return currStroboNums;
    }
};

/*
0 - 0
1 - 1
6 - 9
8 - 8
9 - 6

n:1~14
find all valid
================================

X........X
.X......X.
..X....X..
...
....XX....
or
....X....

find len: even/2, (odd+1)/2
===>
one side len:(sz+1)/2

in one side, find all permutation
the other side, reverse

*/
```