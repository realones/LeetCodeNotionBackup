# Largest 1-Bordered Square

#: 1139
Difficult: Medium
Tags: DP, Matrix
link: https://leetcode.com/problems/largest-1-bordered-square/description/
Priority: Medium
Status: checkBetterSolution
Created time: September 21, 2023 10:10 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given a 2D `grid` of `0`s and `1`s, return the number of elements in the largest **square** subgrid that has all `1`s on its **border**, or `0` if such a subgrid doesn't exist in the `grid`.

**Example 1:**

```
Input: grid = [[1,1,1],[1,0,1],[1,1,1]]
Output: 9

```

**Example 2:**

```
Input: grid = [[1,1,0,0]]
Output: 1

```

**Constraints:**

- `1 <= grid.length <= 100`
- `1 <= grid[0].length <= 100`
- `grid[i][j]` is `0` or `1`

```cpp
class Solution {
public:
    int largest1BorderedSquare(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        //calcu L R U D
        vector<vector<int>> left(matrix), right(matrix), up(matrix), down(matrix);
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1){
                    up[i][j]+=i-1<0?0:up[i-1][j];
                    left[i][j]+=j-1<0?0:left[i][j-1];
                }
            }
        }
        for(int i=m-1;i>=0;i--){
            for(int j=n-1;j>=0;j--){
                if(matrix[i][j]==1){
                    down[i][j]+=i+1>m-1?0:down[i+1][j];
                    right[i][j]+=j+1>n-1?0:right[i][j+1];
                }
            }
        }
        //for each cell, check each h
        int res=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                //i,j is the bottom right cell
                for(int hi=i,hj=j; hi>=0&&hj>=0; hi--,hj--){
                    //hi,hj is the up left cell
                    int len=i-hi+1;
                    if( down[hi][hj]>=len &&
                        right[hi][hj]>=len &&
                        up[i][j]>=len &&
                        left[i][j]>=len ){ res=max(res,len); }
                }
            }
        }
        return pow(res,2);
    }
};

/*
largest square area:
    1 on border

build L,U,D,R, for how many consequtive 1s from cur cell - O(n), n ~100

for each node, go h till not possible, check largest square
O(n^2 * sqrt(2)*n)
*/
```