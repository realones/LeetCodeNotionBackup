# Number of Submatrices That Sum to Target

#: 1074
Difficult: Hard
Tags: Hash, Matrix, Prefix
link: https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/description/
Priority: Medium
Created time: December 19, 2023 9:40 PM
Last edited time: December 19, 2023 9:41 PM
source: googleFreq

Given a `matrix` and a `target`, return the number of non-empty submatrices that sum to target.

A submatrix `x1, y1, x2, y2` is the set of all cells `matrix[x][y]` with `x1 <= x <= x2` and `y1 <= y <= y2`.

Two submatrices `(x1, y1, x2, y2)` and `(x1', y1', x2', y2')` are different if they have some coordinate that is different: for example, if `x1 != x1'`.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/02/mate1.jpg](https://assets.leetcode.com/uploads/2020/09/02/mate1.jpg)

```
Input: matrix = [[0,1,0],[1,1,1],[0,1,0]], target = 0
Output: 4
Explanation: The four 1x1 submatrices that only contain 0.

```

**Example 2:**

```
Input: matrix = [[1,-1],[-1,1]], target = 0
Output: 5
Explanation: The two 1x2 submatrices, plus the two 2x1 submatrices, plus the 2x2 submatrix.

```

**Example 3:**

```
Input: matrix = [[904]], target = 0
Output: 0

```

**Constraints:**

- `1 <= matrix.length <= 100`
- `1 <= matrix[0].length <= 100`
- `1000 <= matrix[i] <= 1000`
- `10^8 <= target <= 10^8`

```cpp
using ll=long long;

class Solution {
public:
    int m,n;
    vector<vector<int>> prefixSum;

    int numSubmatrixSumTarget(vector<vector<int>>& matrix, int target) {
        m=matrix.size(), n=matrix[0].size();//check case ==0
        //build prefix matrix
        buildPrefixMatrix(matrix);
        //for each r1 r2 O(m^2)
        int res=0;
        for(int i1=0;i1<m;i1++){
            for(int i2=i1;i2<m;i2++){
                //prefix arr for each rol, checking how many cur-target exist in prefixMap O(n)
                unordered_map<int,int> mp;
                mp[0]=1;//init
                for(int j=0;j<n;j++){
                    int cur=sumRegion(i1,0, i2,j);
                    res+=mp[cur-target];
                    mp[cur]++;
                }
            }
        }
        return res;
    }

    void buildPrefixMatrix(vector<vector<int>>& matrix) {
        prefixSum=matrix;
        prefixSum[0][0]=matrix[0][0];
        for(int i=1; i<m; i++) prefixSum[i][0]+=prefixSum[i-1][0];
        for(int j=1; j<n; j++) prefixSum[0][j]+=prefixSum[0][j-1];
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                prefixSum[i][j]= prefixSum[i-1][j]+ prefixSum[i][j-1]- prefixSum[i-1][j-1] + matrix[i][j];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        return prefixSum[row2][col2] 
                - ((col1-1>=0)?prefixSum[row2][col1-1]:0)
                - ((row1-1>=0)?prefixSum[row1-1][col2]:0)
                + ((row1-1>=0 && col1-1>=0)?prefixSum[row1-1][col1-1]:0);
    }
};
```