# Making A Large Island

#: 827
Difficult: Hard
Tags: BFS, Connected_Component, DFS, Graph, Matrix, Union Find
link: https://leetcode.com/problems/making-a-large-island/description/
Priority: Medium
Status: No Idea At First
Created time: June 28, 2023 12:25 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Max Area of Island (Max%20Area%20of%20Island%2023b2df13e6a949bcbaaaf6a435d51854.md)

You are given an `n x n` binary matrix `grid`. You are allowed to change **at most one** `0` to be `1`.

Return *the size of the largest **island** in* `grid` *after applying this operation*.

An **island** is a 4-directionally connected group of `1`s.

**Example 1:**

```
Input: grid = [[1,0],[0,1]]
Output: 3
Explanation: Change one 0 to 1 and connect two 1s, then we get an island with area = 3.

```

**Example 2:**

```
Input: grid = [[1,1],[1,0]]
Output: 4
Explanation:Change the 0 to 1 and make the island bigger, only one island with area = 4.
```

**Example 3:**

```
Input: grid = [[1,1],[1,1]]
Output: 4
Explanation: Can't change any 0 to 1, only one island with area = 4.

```

**Constraints:**

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 500`
- `grid[i][j]` is either `0` or `1`.

comment:

一开始没想到用个uid就解决了岛屿归属

```cpp
class Solution {
public:
    //outside of function
    vector<int> d{-1,0,1,0,-1};
    using ii=pair<int,int>;

    int largestIsland(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0

        int res=0;
        int uid=2;
        unordered_map<int,int> mp;//uid_sz
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1){
                    int sz=bfs(matrix,{i,j},uid);
                    mp[uid]=sz;
                    res=max(res, sz);
                    uid++;
                }
            }
        }

        unordered_set<int> uids;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                uids.clear();
                if(matrix[i][j]==0){
                    for(int t=0; t<4; t++){
                        int nx = i + d[t];
                        int ny = j + d[t+1];
                        if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                        if(matrix[nx][ny]!=0) uids.insert(matrix[nx][ny]);
                    }
                    int tmp=1;
                    for(auto uid: uids){
                        tmp+=mp[uid];
                    }
                    res=max(res, tmp);
                }
            }
        }
        return res;
    }

    int bfs(vector<vector<int>>& matrix, ii root, int uid){
        int m=matrix.size(); 
        int n=matrix[0].size();
        int res=0;
        queue<ii> q;

        matrix[root.first][root.second] = uid;//mark as visited
        q.push(root);
        res++;

        while(!q.empty()){
            auto [x,y] = q.front(); q.pop();
            for(int i=0; i<4; i++){
                int nx = x + d[i];
                int ny = y + d[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(matrix[nx][ny]==1) {
                    matrix[nx][ny]=uid;
                    res++;
                    q.emplace(nx,ny);
                }
            }
        }
        return res;
    }
};
```