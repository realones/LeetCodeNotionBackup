# Shortest Path to Get All Keys

#: 864
Difficult: Hard
Tags: Array, BFS, Bit Manipulation, Graph, Math, visiting_all_nodes_with_dup
link: https://leetcode.com/problems/shortest-path-to-get-all-keys/description/
Priority: High
Status: HardToImplement, toSummarize
Created time: June 28, 2023 8:05 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily
related: Shortest Path Visiting All Nodes (Shortest%20Path%20Visiting%20All%20Nodes%206756cebd582341eca7d2fd9d8da8c95c.md)

You are given an `m x n` grid `grid` where:

- `'.'` is an empty cell.
- `'#'` is a wall.
- `'@'` is the starting point.
- Lowercase letters represent keys.
- Uppercase letters represent locks.

You start at the starting point and one move consists of walking one space in one of the four cardinal directions. You cannot walk outside the grid, or walk into a wall.

If you walk over a key, you can pick it up and you cannot walk over a lock unless you have its corresponding key.

For some `1 <= k <= 6`, there is exactly one lowercase and one uppercase letter of the first `k` letters of the English alphabet in the grid. This means that there is exactly one key for each lock, and one lock for each key; and also that the letters used to represent the keys and locks were chosen in the same order as the English alphabet.

Return *the lowest number of moves to acquire all keys*. If it is impossible, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/07/23/lc-keys2.jpg](https://assets.leetcode.com/uploads/2021/07/23/lc-keys2.jpg)

```
Input: grid = ["@.a..","###.#","b.A.B"]
Output: 8
Explanation: Note that the goal is to obtain all the keys not to open all the locks.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/07/23/lc-key2.jpg](https://assets.leetcode.com/uploads/2021/07/23/lc-key2.jpg)

```
Input: grid = ["@..aA","..B#.","....b"]
Output: 6

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/07/23/lc-keys3.jpg](https://assets.leetcode.com/uploads/2021/07/23/lc-keys3.jpg)

```
Input: grid = ["@Aa"]
Output: -1

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 30`
- `grid[i][j]` is either an English letter, `'.'`, `'#'`, or `'@'`.
- The number of keys in the grid is in the range `[1, 6]`.
- Each key in the grid is **unique**.
- Each key in the grid has a matching lock.

comment: see similar question - “****Shortest Path to Get All Keys****”

Solution BFS:

see “**Shortest Path to Get All Keys**” for algorithm explanation

```cpp
using ii=pair<int,int>;
using iii=tuple<int,int,int>;
vector<int> d{-1,0,1,0,-1};

class Solution {
public:
    int shortestPathAllKeys(vector<string>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        ii src={-1,-1};
        int keyCnt=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(islower(matrix[i][j])) keyCnt++;
                else if('@'==matrix[i][j]) src={i,j};
            }
        }
        //bfs visited[pos][keys], once get all key return, otherwise -1
        queue<iii> q;//pos(ii),int
        vector<vector<vector<int>>> visited(m,vector<vector<int>>(n,vector<int>(1<<keyCnt,0)));

        q.emplace(src.first, src.second, 0);
        visited[src.first][src.second][0]=1;

        int step=0;
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                auto [x,y,state]=q.front(); q.pop();
                if(state==(1<<keyCnt)-1) return step;
                for(int t=0; t<4; t++){
                    int nx=x+d[t];
                    int ny=y+d[t+1];
                    if(nx<0 || nx>m-1 ||ny<0 ||ny>n-1) continue;
                    if(matrix[nx][ny]=='#') continue;
                    if(isupper(matrix[nx][ny]) && !(state & (1<<(matrix[nx][ny]-'A')))) continue;
                    int newState=state;
                    if(islower(matrix[nx][ny])) newState |= 1<<(matrix[nx][ny]-'a');
                    if(visited[nx][ny][newState]) continue;
                    visited[nx][ny][newState]=1;
                    q.emplace(nx,ny,newState);
                }
            }
            step++;
        }
        return -1;
    }
};

```

Solution:

permutation + bfs, brute force find all solutions, hard to implement, 

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int shortestPathAllKeys(vector<string>& matrix) {
        int m=matrix.size();
        int n=matrix[0].size();
        vector<ii> keys(6,{-1,-1});
        vector<ii> locks(6,{-1,-1});
        ii start={-1,-1};
        int sz=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(islower(matrix[i][j])){//key
                    keys[matrix[i][j]-'a']={i,j};
                    sz++;
                }
                else if(isupper(matrix[i][j])){//lock
                    locks[matrix[i][j]-'A']={i,j};
                }
                else if(matrix[i][j]=='@') start={i,j};
            }
        }
        keys.resize(sz);
        locks.resize(sz);

        vector<int> v(sz,1);
        for(int i=0; i<sz; i++){v[i]=i;}
        vector<vector<int>> ways=permute(v);

        int res=INT_MAX;
        for(auto& w: ways){//for each permutation
            ii cur=start;
            int tmp=0;
            for(int i: w){//for each key
                ii& key=keys[i];
                ii& lock=locks[i];
                int step=bfs(matrix,cur,key);
                if(step==-1) {tmp=INT_MAX; break;}
                tmp+=step;
                cur=key;
                matrix[lock.first][lock.second]='.';
            }
            res=min(res,tmp);
            //fix lock
            for(int i=0;i<sz;i++){
                matrix[locks[i].first][locks[i].second]='A'+i;
            }
        }
        return res==INT_MAX?-1:res;
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tmp;
        vector<bool> visited(nums.size(),false);
        helper(nums,res,tmp,visited);
        return res;
    }
    
    void helper(vector<int>& nums, vector<vector<int>>& res, vector<int>& tmp, vector<bool>& visited){
        if(tmp.size()==nums.size()){
            res.push_back(tmp);
            return;
        }
            
        for(int i=0; i<nums.size(); i++){
            if(visited[i]) continue;
            if(i>0 && !visited[i-1] && nums[i]==nums[i-1]) continue;
            tmp.push_back(nums[i]); visited[i]=true;
            helper(nums,res,tmp,visited);
            tmp.pop_back(); visited[i]=false;
        }
    }

    vector<int> d={-1,0,1,0,-1};
    int bfs(vector<string>& matrix, ii start, ii end){
        int m=matrix.size();
        int n=matrix[0].size();
        auto [sx,sy]=start; 
        auto [ex,ey]=end;
        queue<ii> q;
        vector<vector<bool>> visited(m,vector<bool>(n,0));
        q.push(start);
        visited[sx][sy]=1;
        int step=0;
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                auto [x,y] = q.front(); q.pop();
                if(x==ex && y==ey) return step;
                for(int k=0; k<4; k++){
                    int nx=x+d[k];
                    int ny=y+d[k+1];
                    if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                    if(visited[nx][ny]) continue;
                    if(matrix[nx][ny]=='#') continue;
                    if(isupper(matrix[nx][ny])) continue;
                    visited[nx][ny]=1;
                    q.push({nx,ny});
                }
            }
            step++;
        }
        return -1;
    }
};

/*
. empty cell
# wall
@ start
lower keys
upper locks

k: 1~6

if just wall, from start to target: mbfs
if with one key, just from start to the key

bfs reach one keys: worst: (m*n)
*
permutation of fetch key: worst:(6!)
=6e5

*/
```