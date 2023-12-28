# Equal Row and Column Pairs

#: 2352
Difficult: Medium
Tags: Hash
link: https://leetcode.com/problems/equal-row-and-column-pairs/
Priority: Medium
Status: No Idea At First
Created time: June 13, 2023 11:38 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, leetcode_daily

Given a **0-indexed** `n x n` integer matrix `grid`, *return the number of pairs* `(ri, cj)` *such that row* `ri` *and column* `cj` *are equal*.

A row and column pair is considered equal if they contain the same elements in the same order (i.e., an equal array).

**Example 1:**

![https://assets.leetcode.com/uploads/2022/06/01/ex1.jpg](https://assets.leetcode.com/uploads/2022/06/01/ex1.jpg)

```
Input: grid = [[3,2,1],[1,7,6],[2,7,7]]
Output: 1
Explanation: There is 1 equal row and column pair:
- (Row 2, Column 1): [2,7,7]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/06/01/ex2.jpg](https://assets.leetcode.com/uploads/2022/06/01/ex2.jpg)

```
Input: grid = [[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]
Output: 3
Explanation: There are 3 equal row and column pairs:
- (Row 0, Column 0): [3,1,2,2]
- (Row 2, Column 2): [2,4,2,2]
- (Row 3, Column 2): [2,4,2,2]

```

**Constraints:**

- `n == grid.length == grid[i].length`
- `1 <= n <= 200`
- `1 <= grid[i][j] <= 105`

```cpp
class Solution {
public:
    int equalPairs(vector<vector<int>>& grid) {
        int n=grid.size();
        unordered_multiset<string> st;
        for(int i=0; i<n; i++){
            st.insert(getRowStr(grid,i));
        }
        int res=0;
        for(int j=0; j<n; j++){
            res+=st.count(getColStr(grid,j));
        }
        return res;
    }
    
    string getRowStr(vector<vector<int>>& grid, int i){
        string res;
        for(int j=0; j<grid[0].size(); j++){
            res+=to_string(grid[i][j])+"_";
        }
        return res;
    }
    
    string getColStr(vector<vector<int>>& grid, int j){
        string res;
        for(int i=0; i<grid.size(); i++){
            res+=to_string(grid[i][j])+"_";
        }
        return res;
    }
};
```