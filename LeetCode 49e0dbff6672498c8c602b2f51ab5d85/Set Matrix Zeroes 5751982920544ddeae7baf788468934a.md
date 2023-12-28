# Set Matrix Zeroes

#: 73
Difficult: Medium
Tags: Matrix
link: https://leetcode.com/problems/set-matrix-zeroes/
Priority: Medium
Status: No Idea At First
Created time: June 2, 2023 5:42 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

```
Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

```
Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]

```

**Constraints:**

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `231 <= matrix[i][j] <= 231 - 1`

**Follow up:**

- A straightforward solution using `O(mn)` space is probably a bad idea.
- A simple improvement uses `O(m + n)` space, but still not the best solution.
- Could you devise a constant space solution?

Solution:

把row0，col0当作坐标轴用，每个点投影上去

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) return;
        int n=matrix[0].size(); if(n==0) return;
        
        bool row1Zero=false, col1Zero=false;
        for(int i=0;i<m;i++) {if(matrix[i][0]==0){col1Zero=true;break;}}
        for(int j=0;j<n;j++) {if(matrix[0][j]==0){row1Zero=true;break;}}
        
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(matrix[i][j]!=0) continue;
                matrix[i][0]=0;
                matrix[0][j]=0;
            }
        }
        
        for(int i=1;i<m;i++){
            if(matrix[i][0]==0){markRowZeros(matrix,i);} 
        }
        
        for(int j=1;j<n;j++){
            if(matrix[0][j]==0){markColZeros(matrix,j);} 
        }
        
        if(row1Zero){markRowZeros(matrix,0);} 
        if(col1Zero){markColZeros(matrix,0);}
    }
    
    void markRowZeros(vector<vector<int>>& matrix, int r){
        for(int j=0;j<matrix[0].size();j++){
            matrix[r][j]=0;
        }
    }
    
    void markColZeros(vector<vector<int>>& matrix, int c){
        for(int i=0;i<matrix.size();i++){
            matrix[i][c]=0;
        }
    }
    
};
```