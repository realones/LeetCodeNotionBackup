# Minimum Domino Rotations For Equal Row

#: 1007
Difficult: Medium
Tags: Array, Greedy
link: https://leetcode.com/problems/minimum-domino-rotations-for-equal-row/description/
Priority: Medium
Status: More Solution, checkBetterSolution
Created time: December 24, 2023 12:35 AM
Last edited time: December 24, 2023 12:36 AM
source: googleFreq

In a row of dominoes, `tops[i]` and `bottoms[i]` represent the top and bottom halves of the `ith` domino. (A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.)

We may rotate the `ith` domino, so that `tops[i]` and `bottoms[i]` swap values.

Return the minimum number of rotations so that all the values in `tops` are the same, or all the values in `bottoms` are the same.

If it cannot be done, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/05/14/domino.png](https://assets.leetcode.com/uploads/2021/05/14/domino.png)

```
Input: tops = [2,1,2,4,2,2], bottoms = [5,2,6,2,3,2]
Output: 2
Explanation:
The first figure represents the dominoes as given by tops and bottoms: before we do any rotations.
If we rotate the second and fourth dominoes, we can make every value in the top row equal to 2, as indicated by the second figure.

```

**Example 2:**

```
Input: tops = [3,5,1,2,3], bottoms = [3,6,3,3,4]
Output: -1
Explanation:
In this case, it is not possible to rotate the dominoes to make one row of values equal.

```

**Constraints:**

- `2 <= tops.length <= 2 * 104`
- `bottoms.length == tops.length`
- `1 <= tops[i], bottoms[i] <= 6`

todo: check lee215 solution and editorial solution

```cpp
class Solution {
public:
    int minDominoRotations(vector<int>& tops, vector<int>& bottoms) {
        int n=tops.size();
        vector<bool> avail(7,true); avail[0]=false;
        for(int i=1; i<n; i++){
            int a=tops[i], b=bottoms[i];
            for(int x=1;x<=6;x++){
                if(x!=a && x!=b) avail[x]=false;
            }
        }
        if(accumulate(avail.begin(), avail.end(),0) == 0) return -1;
        int res=INT_MAX;
        for(int x=1;x<=6;x++){
            if(avail[x]){
                int toTop=0;
                int toBottom=0;
                for(int i=0; i<n; i++){
                    int a=tops[i], b=bottoms[i];
                    if(a!=x) toTop++;
                    if(b!=x) toBottom++;
                }
                res=min({res,toTop,toBottom});
            }
        }
        return res;
    }
};
/*
[(a,b)]

len: 2~2e4
val: 1~6

to make one side same
================================
val 1~6
for all, find available val to all (at most 2) O(n)

for each val (2)
top side:
    find steps
bot side
    find steps
*/
```