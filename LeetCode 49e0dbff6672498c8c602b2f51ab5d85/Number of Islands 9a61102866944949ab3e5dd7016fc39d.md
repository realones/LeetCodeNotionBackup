# Number of Islands

#: 200
Difficult: Medium
Tags: BFS, Connected_Component, DFS, Graph, Matrix, Union Find, mbfs_preChkVisited_woSZ, search
link: https://leetcode.com/problems/number-of-islands
Priority: Medium
Created time: July 26, 2022 5:02 PM
Last edited time: November 12, 2023 10:41 PM
practicedTimes: 1
source: AmazonFreq, HuaHua, LC_Top_Interview_150, jiuzhang, 算初BFS, 算高UnionFind&Trie

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1

```

**Example 2:**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

Union find/BFS/DFS 染色

bfs done

```cpp
//BFS
class Solution {
public:
    vector<int> d{-1,0,1,0,-1};

    typedef pair<int, int> Block;
    
    int numIslands(vector<vector<char>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        int cnt=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]=='1'){
                    cnt++;
                    bfs(matrix,{i,j});
                }
            }
        }
        return cnt;
    }
    
    void bfs(vector<vector<char>>& matrix, Block root){
        int m=matrix.size(); 
        int n=matrix[0].size();
        queue<Block> q;
        matrix[root.first][root.second] = '2';
        q.push(root);
        while(!q.empty()){
            Block cur = q.front(); q.pop();
            int x=cur.first, y=cur.second;
            for(int i=0; i<4; i++){
                int nx = x + d[i];
                int ny = y + d[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1){
                    continue;
                }
                if(matrix[nx][ny]=='1') {
                    matrix[nx][ny]='2';
                    q.push({nx,ny});
                }
            }
        }
    }
};
```

```cpp
//DFS
class Solution {
public:
    vector<int> d{-1,0,1,0,-1};
    typedef pair<int, int> Block;
    
    int numIslands(vector<vector<char>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        int cnt=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]!='1') continue;
                cnt++;
                dfs(matrix,Block(i,j));
            }
        }
        return cnt;
    }

    void dfs(vector<vector<char>>& matrix, Block root){
        int m=matrix.size(); 
        int n=matrix[0].size();

        matrix[root.first][root.second] = '2';//mark as visited

        int x=root.first, y=root.second;
        for(int i=0; i<4; i++){
            int nx = x + d[i];
            int ny = y + d[i+1];
            if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
            if(matrix[nx][ny]=='1') {
                dfs(matrix,{nx,ny});
            }
        }
    }
};
```

```cpp
// version 2: Union Find
class UnionFind { 

    private int[] father = null;
    private int count;

    private int find(int x) {
        if (father[x] == x) {
            return x;
        }
        return father[x] = find(father[x]);
    }

    public UnionFind(int n) {
        // initialize your data structure here.
        father = new int[n];
        for (int i = 0; i < n; ++i) {
            father[i] = i;
        }
    }

    public void connect(int a, int b) {
        // Write your code here
        int root_a = find(a);
        int root_b = find(b);
        if (root_a != root_b) {
            father[root_a] = root_b;
            count --;
        }
    }
        
    public int query() {
        // Write your code here
        return count;
    }
    
    public void set_count(int total) {
        count = total;
    }
}

public class Solution {
    public int numIslands(boolean[][] grid) {
        int count = 0;
        int n = grid.length;
        if (n == 0)
            return 0;
        int m = grid[0].length;
        if (m == 0)
            return 0;
        UnionFind union_find = new UnionFind(n * m);
        
        int total = 0;
        for(int i = 0;i < grid.length; ++i)
            for(int j = 0;j < grid[0].length; ++j)
            if (grid[i][j])
                total ++;
    
        union_find.set_count(total);
        for(int i = 0;i < grid.length; ++i)
            for(int j = 0;j < grid[0].length; ++j)
            if (grid[i][j]) {
                if (i > 0 && grid[i - 1][j]) {
                    union_find.connect(i * m + j, (i - 1) * m + j);
                }
                if (j > 0 && grid[i][j - 1]) {
                    union_find.connect(i * m + j, i * m + j - 1);
                }
            }
        return union_find.query();
    }
}
```