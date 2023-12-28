# Last Day Where You Can Still Cross

#: 1970
Difficult: Hard
Tags: Array, BFS, BS_Idx, Matrix, Union Find
link: https://leetcode.com/problems/last-day-where-you-can-still-cross/description/
Priority: High
Status: No Idea At First
Created time: June 29, 2023 9:53 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

There is a **1-based** binary matrix where `0` represents land and `1` represents water. You are given integers `row` and `col` representing the number of rows and columns in the matrix, respectively.

Initially on day `0`, the **entire** matrix is **land**. However, each day a new cell becomes flooded with **water**. You are given a **1-based** 2D array `cells`, where `cells[i] = [ri, ci]` represents that on the `ith` day, the cell on the `rith` row and `cith` column (**1-based** coordinates) will be covered with **water** (i.e., changed to `1`).

You want to find the **last** day that it is possible to walk from the **top** to the **bottom** by only walking on land cells. You can start from **any** cell in the top row and end at **any** cell in the bottom row. You can only travel in the **four** cardinal directions (left, right, up, and down).

Return *the **last** day where it is possible to walk from the **top** to the **bottom** by only walking on land cells*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/07/27/1.png](https://assets.leetcode.com/uploads/2021/07/27/1.png)

```
Input: row = 2, col = 2, cells = [[1,1],[2,1],[1,2],[2,2]]
Output: 2
Explanation: The above image depicts how the matrix changes each day starting from day 0.
The last day where it is possible to cross from top to bottom is on day 2.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/07/27/2.png](https://assets.leetcode.com/uploads/2021/07/27/2.png)

```
Input: row = 2, col = 2, cells = [[1,1],[1,2],[2,1],[2,2]]
Output: 1
Explanation: The above image depicts how the matrix changes each day starting from day 0.
The last day where it is possible to cross from top to bottom is on day 1.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/07/27/3.png](https://assets.leetcode.com/uploads/2021/07/27/3.png)

```
Input: row = 3, col = 3, cells = [[1,2],[2,1],[3,3],[2,2],[1,1],[1,3],[2,3],[3,2],[3,1]]
Output: 3
Explanation: The above image depicts how the matrix changes each day starting from day 0.
The last day where it is possible to cross from top to bottom is on day 3.

```

**Constraints:**

- `2 <= row, col <= 2 * 104`
- `4 <= row * col <= 2 * 104`
- `cells.length == row * col`
- `1 <= ri <= row`
- `1 <= ci <= col`
- All the values of `cells` are **unique**.

Solution BS:

```cpp
class Solution {
public:
    int m,n;
    vector<vector<int>> matrix;
    vector<vector<int>> cells;

    int latestDayToCross(int row, int col, vector<vector<int>>& cells) {
        m=row, n=col;
        matrix=vector<vector<int>>(m,vector<int>(n,0));
        this->cells=cells;

        int l=0,r=cells.size()-1;
        while(l<r){//YYY[Y]NNNN
            int m=l+((long long)r-l+1)/2;
            if(!valid(m)) r=m-1;
            else l=m;
        }
        return r;
    }

    bool valid(int c){
        //build matrix;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                matrix[i][j]=0;
            }
        }
        for(int i=0;i<c;i++){
            matrix[cells[i][0]-1][cells[i][1]-1]=1;
        }
        int res= bfs();
        return res;
    }

    //outside of function
    vector<int> d{-1,0,1,0,-1};
    typedef pair<int, int> ii;

    bool bfs(){
        queue<ii> q;

        //first row, if land put into
        for(int j=0; j<n; j++){
            if(matrix[0][j]==0){//land
                matrix[0][j] = 2;//mark as visited
                q.emplace(0,j);
            }
        }

        while(!q.empty()){
            auto [x,y] = q.front(); q.pop();
            if(x==m-1) return true;
            for(int i=0; i<4; i++){
                int nx = x + d[i];
                int ny = y + d[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(matrix[nx][ny]==1) continue;
                if(matrix[nx][ny]==0) {
                    matrix[nx][ny]=2;
                    q.push({nx,ny});
                }
            }
        }
        return false;
    }
};
```

Solution 2:

倒序+unionfind

```cpp
class Solution {
public:
    vector<int> d{-1,0,1,0,-1};

    int latestDayToCross(int row, int col, vector<vector<int>>& cells) {
        int m=row, n=col;
        vector<vector<int>> matrix(m,vector<int>(n,1));

        init(m*n+2);
        int top=m*n;
        int bot=m*n+1;

        for(int i=cells.size()-1;i>=0;i--){
            int x=cells[i][0]-1, y=cells[i][1]-1;
            matrix[x][y]=0;
            if(x==0) Union(x*n+y,top);
            if(x==m-1) Union(x*n+y,bot);
            //for each neighbor
            for(int k=0;k<4;k++){
                int nx=x+d[k];
                int ny=y+d[k+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(matrix[nx][ny]==0) Union(x*n+y,nx*n+ny);
            }
            if(find(top)==find(bot)) return i;
        }
        return -1;
    }

    //init
    vector<int> father;
    void init(int n) {
        father.resize(n,0);
        for(int i=0;i<n;i++){
            father[i]=i;//parent is itself
        }
    }

    //with pass compression
    // O(1)
    int find(int a){
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    //union - union roots, tree like
    // O(1)
    bool Union(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            return true;
        }
        return false;
    }
};
```