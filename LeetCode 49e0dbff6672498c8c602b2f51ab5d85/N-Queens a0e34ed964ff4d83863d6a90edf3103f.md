# N-Queens

#: 51
Difficult: Hard
Tags: BackTracking, Permutation, search
link: https://leetcode.com/problems/n-queens/
Priority: Low
Created time: July 29, 2022 4:01 PM
Last edited time: October 16, 2023 4:55 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初DFS

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *all distinct solutions to the **n-queens puzzle***. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/11/13/queens.jpg](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above

```

**Example 2:**

```
Input: n = 1
Output: [["Q"]]

```

**Constraints:**

- `1 <= n <= 9`

```cpp
class Solution {
public:
    /*
     * @param n: The number of queens
     * @return: All distinct solutions
     */
    vector<vector<string>> solveNQueens(int n) {
        // write your code here
        if(n<=0) return vector<vector<string>>();

        vector<vector<string>> res;
        vector<int> cols;//index row, value col
        helper(n,res,cols);
        return res;
    }
    void helper(int n, vector<vector<string>>& res, vector<int> &cols){
        if(cols.size()==n){
            res.push_back(drawBoard(cols,n));
            return;
        }

        for(int col=0;col<n;col++){
            if(!isValid(cols,col)) {continue;}
            cols.push_back(col);
            helper(n,res,cols);
            cols.pop_back();
        }

    }

    bool isValid(vector<int>& cols, int newCol){
        int newRow=cols.size();
        for(int row=0;row<cols.size();row++){
            int col=cols[row];
            if(col==newCol) return false;
            else if(col-row==newCol-newRow) return false;
            else if(col+row==newCol+newRow) return false;
            else continue;
        }
        return true;
    }
    //[0,1,2]=>
    //Q..
    //.Q.
    //..Q
    vector<string> drawBoard(vector<int> &cols,int n){
        vector<string> res;
        for(auto col:cols){
            string tmp(n,'.');
            tmp[col]='Q';
            res.push_back(tmp);
        }
        return res;
    }
};
```