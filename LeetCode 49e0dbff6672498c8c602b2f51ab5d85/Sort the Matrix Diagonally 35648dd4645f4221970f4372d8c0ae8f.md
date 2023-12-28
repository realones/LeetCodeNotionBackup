# Sort the Matrix Diagonally

#: 1329
Difficult: Medium
Tags: Array, Matrix, Sort
link: https://leetcode.com/problems/sort-the-matrix-diagonally/
Priority: Medium
Created time: June 15, 2023 8:05 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

A **matrix diagonal** is a diagonal line of cells starting from some cell in either the topmost row or leftmost column and going in the bottom-right direction until reaching the matrix's end. For example, the **matrix diagonal** starting from `mat[2][0]`, where `mat` is a `6 x 3` matrix, includes cells `mat[2][0]`, `mat[3][1]`, and `mat[4][2]`.

Given an `m x n` matrix `mat` of integers, sort each **matrix diagonal** in ascending order and return *the resulting matrix*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/21/1482_example_1_2.png](https://assets.leetcode.com/uploads/2020/01/21/1482_example_1_2.png)

```
Input: mat = [[3,3,1,1],[2,2,1,2],[1,1,1,2]]
Output: [[1,1,1,1],[1,2,2,2],[1,2,3,3]]

```

**Example 2:**

```
Input: mat = [[11,25,66,1,69,7],[23,55,17,45,15,52],[75,31,36,44,58,8],[22,27,33,25,68,4],[84,28,14,11,5,50]]
Output: [[5,17,4,1,52,7],[11,11,25,45,8,69],[14,23,25,44,58,15],[22,27,31,36,50,66],[84,28,75,33,55,68]]

```

**Constraints:**

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 100`
- `1 <= mat[i][j] <= 100`

```cpp
class Solution {
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& matrix) {
        int m=matrix.size();
        int n=matrix[0].size();
        
        for(int i=0; i<m; i++){
            vector<int> nums;
            int x=i,y=0;
            while(x<m && y<n){
                nums.push_back(matrix[x][y]);
                x++,y++;
            }
            
            sort(nums.begin(), nums.end());
            
            x=i,y=0;
            int k=0;
            while(x<m && y<n){
                matrix[x][y]=nums[k++];
                x++,y++;
            }
        }
        
        for(int j=1; j<n; j++){
            vector<int> nums;
            int x=0,y=j;
            while(x<m && y<n){
                nums.push_back(matrix[x][y]);
                x++,y++;
            }
            
            sort(nums.begin(), nums.end());
            
            x=0,y=j;
            int k=0;
            while(x<m && y<n){
                matrix[x][y]=nums[k++];
                x++,y++;
            }
        }
        
        return matrix;
    }
};
```