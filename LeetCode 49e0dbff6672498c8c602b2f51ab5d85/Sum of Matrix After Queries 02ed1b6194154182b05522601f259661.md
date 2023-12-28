# Sum of Matrix After Queries

#: 2718
Difficult: Medium
Tags: Matrix
link: https://leetcode.com/problems/sum-of-matrix-after-queries/
Priority: Medium
Status: No Idea At First
Created time: June 3, 2023 9:19 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given an integer `n` and a **0-indexed** **2D array** `queries` where `queries[i] = [typei, indexi, vali]`.

Initially, there is a **0-indexed** `n x n` matrix filled with `0`'s. For each query, you must apply one of the following changes:

- if `typei == 0`, set the values in the row with `indexi` to `vali`, overwriting any previous values.
- if `typei == 1`, set the values in the column with `indexi` to `vali`, overwriting any previous values.

Return *the sum of integers in the matrix after all queries are applied*.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/05/11/exm1.png](https://assets.leetcode.com/uploads/2023/05/11/exm1.png)

```
Input: n = 3, queries = [[0,0,1],[1,2,2],[0,2,3],[1,0,4]]
Output: 23
Explanation: The image above describes the matrix after each query. The sum of the matrix after all queries are applied is 23.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/05/11/exm2.png](https://assets.leetcode.com/uploads/2023/05/11/exm2.png)

```
Input: n = 3, queries = [[0,0,4],[0,1,2],[1,0,1],[0,2,3],[1,2,1]]
Output: 17
Explanation: The image above describes the matrix after each query. The sum of the matrix after all queries are applied is 17.

```

**Constraints:**

- `1 <= n <= 104`
- `1 <= queries.length <= 5 * 104`
- `queries[i].length == 3`
- `0 <= typei <= 1`
- `0 <= indexi < n`
- `0 <= vali <= 105`

Solution: 倒序

```cpp
typedef long long ll;

class Solution {
public:
    long long matrixSumQueries(int n, vector<vector<int>>& queries) {
        ll res=0;
        unordered_set<int> rst;//which row has used
        unordered_set<int> cst;
        for(int i=queries.size()-1;i>=0;i--){
            int type=queries[i][0],index=queries[i][1],val=queries[i][2];
            if(type==0){//row
                if(rst.count(index)) continue;
                rst.insert(index);
                res+=(n-cst.size()) * val;
            }
            else{//col
                if(cst.count(index)) continue;
                cst.insert(index);
                res+=(n-rst.size()) * val;
            }
        }
        return res;
    }
};
```