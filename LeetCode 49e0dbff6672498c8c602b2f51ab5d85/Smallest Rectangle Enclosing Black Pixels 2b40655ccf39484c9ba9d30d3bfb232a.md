# Smallest Rectangle Enclosing Black Pixels

#: 302
Difficult: Hard
Tags: BFS, BS_Idx, Matrix
link: https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/
Priority: Low
Created time: July 8, 2022 12:32 PM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: jiuzhang, 算初BFS, 算初BS

[http://www.lintcode.com/problem/smallest-rectangle-enclosing-black-pixels/](http://www.lintcode.com/problem/smallest-rectangle-enclosing-black-pixels/)

You are given an `m x n` binary matrix `image` where `0` represents a white pixel and `1` represents a black pixel.

The black pixels are connected (i.e., there is only one black region). Pixels are connected horizontally and vertically.

Given two integers `x` and `y` that represents the location of one of the black pixels, return *the area of the smallest (axis-aligned) rectangle that encloses all black pixels*.

You must write an algorithm with less than `O(mn)` runtime complexity

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/14/pixel-grid.jpg](https://assets.leetcode.com/uploads/2021/03/14/pixel-grid.jpg)

```
Input: image = [["0","0","1","0"],["0","1","1","0"],["0","1","0","0"]], x = 0, y = 2
Output: 6

```

**Example 2:**

```
Input: image = [["1"]], x = 0, y = 0
Output: 1

```

**Constraints:**

- `m == image.length`
- `n == image[i].length`
- `1 <= m, n <= 100`
- `image[i][j]` is either `'0'` or `'1'`.
- `0 <= x < m`
- `0 <= y < n`
- `image[x][y] == '1'.`
- The black pixels in the `image` only form **one component**.

600 BS/(BFS无法AC但是可以作为BFS的练习题)

```cpp
class Solution {
public:
    /*
     * @param image: a binary matrix with '0' and '1'
     * @param x: the location of one of the black pixels
     * @param y: the location of one of the black pixels
     * @return: an integer
     */
    int minArea(vector<vector<char>> &image, int x, int y) {
        int m=image.size(); if(m==0) return -1;
        int n=image[0].size(); if(n==0) return -1;

        int left,right,top,bottom;
        int a,b;
        //left
        a=0,b=y;
        while(a<=b){
            int mid=a+(b-a)/2;
            if(isWhiteCol(image,mid)) a=mid+1;
            else b=mid-1;
        }
        left=a;

        //right
        a=y,b=n-1;
        while(a<=b){
            int mid=a+(b-a)/2;
            if(isWhiteCol(image,mid)) b=mid-1;
            else a=mid+1;
        }
        right=b;

        //top
        a=0,b=x;
        while(a<=b){
            int mid=a+(b-a)/2;
            if(isWhiteRow(image,mid)) a=mid+1;
            else b=mid-1;
        }
        top=a;

        //bottom
        a=x,b=m-1;
        while(a<=b){
            int mid=a+(b-a)/2;
            if(isWhiteRow(image,mid)) b=mid-1;
            else a=mid+1;
        }
        bottom=b;

        return (right-left+1)*(bottom-top+1);
    }

    bool isWhiteRow(vector<vector<char>> &image, int r){
        for(int i=0;i<image[0].size();i++){
            if(image[r][i]=='1') return false;
        }
        return true;
    }
    bool isWhiteCol(vector<vector<char>> &image, int c){
        for(int i=0;i<image.size();i++){
            if(image[i][c]=='1') return false;
        }
        return true;
    }
};
```