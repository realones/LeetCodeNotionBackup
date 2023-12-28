# Word Search

#: 79
Difficult: Medium
Tags: BackTracking, DFS, Matrix, search
link: https://leetcode.com/problems/word-search/
Priority: Medium
Status: HardToImplement
Created time: June 7, 2023 2:07 PM
Last edited time: November 17, 2023 1:31 AM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, ticktokFreq

Given an `m x n` grid of characters `board` and a string `word`, return `true` *if* `word` *exists in the grid*.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/11/04/word2.jpg](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true

```

**Example 3:**

![https://assets.leetcode.com/uploads/2020/10/15/word3.jpg](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false

```

**Constraints:**

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` and `word` consists of only lowercase and uppercase English letters.

**Follow up:** Could you use search pruning to make your solution faster with a larger `board`?

```cpp
class Solution {
public:
    vector<int> dx{0,1,0,-1};
    vector<int> dy{1,0,-1,0};
    
    bool exist(vector<vector<char>>& board, string word) {
        int m=board.size(); if(m==0) return false;
        int n=board[0].size(); if(n==0) return false;
        if(word.empty()) return true;
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(dfs(board,i,j,word,0)) return true;
            }
        }
        return false;
    }
    //dfs w/ visited
    bool dfs(vector<vector<char>>& board, int x, int y, string& word, int start){
        if(start==word.size()) return true;
        if(x<0 || x>=board.size() || y<0 || y>=board[0].size() || word[start]!=board[x][y]) return false;
        char t=board[x][y]; board[x][y]='v';
        for(int i=0;i<4;i++){
            int nx=x+dx[i], ny=y+dy[i];
            if(dfs(board,nx,ny,word,start+1)) return true;
        }
        board[x][y]=t;
        
        return false;
    }
};
```