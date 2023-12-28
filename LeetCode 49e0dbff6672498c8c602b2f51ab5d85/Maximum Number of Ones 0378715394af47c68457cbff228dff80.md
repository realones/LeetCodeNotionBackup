# Maximum Number of Ones

#: 1183
Difficult: Hard
Tags: Greedy, Math, Matrix, PQ
link: https://leetcode.com/problems/maximum-number-of-ones/
Priority: High
Status: No Idea At First
Created time: August 29, 2023 7:09 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Consider a matrix `M` with dimensions `width * height`, such that every cell has value `0` or `1`, and any **square** sub-matrix of `M` of size `sideLength * sideLength` has at most `maxOnes` ones.

Return the maximum possible number of ones that the matrix `M` can have.

**Example 1:**

```
Input: width = 3, height = 3, sideLength = 2, maxOnes = 1
Output: 4
Explanation:
In a 3*3 matrix, no 2*2 sub-matrix can have more than 1 one.
The best solution that has 4 ones is:
[1,0,1]
[0,0,0]
[1,0,1]

```

**Example 2:**

```
Input: width = 3, height = 3, sideLength = 2, maxOnes = 2
Output: 6
Explanation:
[1,0,1]
[1,0,1]
[1,0,1]

```

**Constraints:**

- `1 <= width, height <= 100`
- `1 <= sideLength <= width, height`
- `0 <= maxOnes <= sideLength * sideLength`

comment: like a math-themed brain teaser

```cpp
class Solution {
public:
    int maximumNumberOfOnes(int m, int n, int len, int maxOnes) {
        vector<int> cnts;
        for(int i=0;i<len;i++){
            for(int j=0;j<len;j++){
                int cnt = (1+(m-i-1)/len) * (1+(n-j-1)/len);
                cnts.push_back(cnt);
            }
        }
        sort(cnts.begin(), cnts.end(), greater<>());
        int res=0;
        for(int i=0; i<maxOnes; i++){
            res+=cnts[i];
        }
        return res;
    }
};

/*
1<=m,n<=100
1<=len<=m,n
0<=maxOnes<=len*len
================================
could't figure out - checked Editorial

best strategy is to tiling first(topLeft) square to all the grid - hmmm

you can put all maxOnes 1 in all squares, no need to consider < maxOnes
you can put maxOnes 1 whatever ways you want, they all valid, only need to think how to max the profit

int the first square, check each cell how many its relative position after tiling
    e.g. for cell (x,y)
        1+(m−x−1)/len
        *
        1+(n−y−1)/len
then greedy put 1 in the cell with maximum relative postions
*/
```