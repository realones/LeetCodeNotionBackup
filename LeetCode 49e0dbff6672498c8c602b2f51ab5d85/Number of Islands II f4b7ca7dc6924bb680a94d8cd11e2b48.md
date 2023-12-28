# Number of Islands II

#: 305
Difficult: Hard
Tags: Union Find
link: https://leetcode.com/problems/number-of-islands-ii
Priority: Medium
Created time: August 27, 2022 4:12 PM
Last edited time: October 22, 2023 3:12 PM
practicedTimes: 1
source: jiuzhang, 算高UnionFind&Trie

You are given an empty 2D binary grid `grid` of size `m x n`. The grid represents a map where `0`'s represent water and `1`'s represent land. Initially, all the cells of `grid` are water cells (i.e., all the cells are `0`'s).

We may perform an add land operation which turns the water at position into a land. You are given an array `positions` where `positions[i] = [ri, ci]` is the position `(ri, ci)` at which we should operate the `ith` operation.

Return *an array of integers* `answer` *where* `answer[i]` *is the number of islands after turning the cell* `(ri, ci)` *into a land*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/10/tmp-grid.jpg](https://assets.leetcode.com/uploads/2021/03/10/tmp-grid.jpg)

```
Input: m = 3, n = 3, positions = [[0,0],[0,1],[1,2],[2,1]]
Output: [1,1,2,3]
Explanation:
Initially, the 2d grid is filled with water.
- Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land. We have 1 island.
- Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land. We still have 1 island.
- Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land. We have 2 islands.
- Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land. We have 3 islands.

```

**Example 2:**

```
Input: m = 1, n = 1, positions = [[0,0]]
Output: [1]

```

**Constraints:**

- `1 <= m, n, positions.length <= 104`
- `1 <= m * n <= 104`
- `positions[i].length == 2`
- `0 <= ri < m`
- `0 <= ci < n`

**Follow up:** Could you solve it in time complexity `O(k log(mn))`, where `k == positions.length`?

动态加入union

```cpp
class UnionFind {
public:
    vector<int> father;
    
    UnionFind(int n){
        father.resize(n);
        for(int i=0; i<n; i++){
            father[i]=i;
        }
    }
    
    int find(int a){
        if(a==father[a]) return a;
        father[a] = find(father[a]);
        return father[a];
    }
    
    bool connect(int a, int b){
        int rootA=find(a);
        int rootB=find(b);
        if(rootA==rootB) return false;
        father[rootB]=rootA;
        return true;
    }
};

class Solution {
public:
    vector<int> numIslands2(int m, int n, vector<vector<int>>& positions) {
        vector<int> res;
        int cnt=0;
        vector<vector<int>> matrix(m,vector<int>(n,0));
        UnionFind uf(m*n);
        for(auto p: positions){
            int x=p[0], y=p[1], a=x*n+y;
            if(matrix[x][y]){
                res.push_back(cnt);
                continue;
            }
            matrix[x][y]=1;
            cnt++;
            //for each neighbor
            vector<int> d{0,1,0,-1,0};
            for(int k=0;k<4;k++){
                int nx=x+d[k];
                int ny=y+d[k+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(matrix[nx][ny]==1){
                    //union, if true cnt--;
                    int b=nx*n+ny;
                    if(uf.connect(a,b)) cnt--;
                }
            }
            res.push_back(cnt);
        }
        return res;
    }
};
```