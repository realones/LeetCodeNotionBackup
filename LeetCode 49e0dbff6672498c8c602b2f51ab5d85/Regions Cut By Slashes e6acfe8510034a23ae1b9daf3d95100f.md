# Regions Cut By Slashes

#: 959
Difficult: Medium
Tags: Array, BFS, DFS, Graph, Hash, Matrix, Union Find, string
link: https://leetcode.com/problems/regions-cut-by-slashes/
Priority: Medium
Status: checkBetterSolution
Created time: June 28, 2023 12:24 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

An `n x n` grid is composed of `1 x 1` squares where each `1 x 1` square consists of a `'/'`, `'\'`, or blank space `' '`. These characters divide the square into contiguous regions.

Given the grid `grid` represented as a string array, return *the number of regions*.

Note that backslash characters are escaped, so a `'\'` is represented as `'\\'`.

**Example 1:**

![https://assets.leetcode.com/uploads/2018/12/15/1.png](https://assets.leetcode.com/uploads/2018/12/15/1.png)

```
Input: grid = [" /","/ "]
Output: 2

```

**Example 2:**

![https://assets.leetcode.com/uploads/2018/12/15/2.png](https://assets.leetcode.com/uploads/2018/12/15/2.png)

```
Input: grid = [" /","  "]
Output: 1

```

**Example 3:**

![https://assets.leetcode.com/uploads/2018/12/15/4.png](https://assets.leetcode.com/uploads/2018/12/15/4.png)

```
Input: grid = ["/\\","\\/"]
Output: 5
Explanation:Recall that because \ characters are escaped, "\\/" refers to \/, and "/\\" refers to /\.

```

**Constraints:**

- `n == grid.length == grid[i].length`
- `1 <= n <= 30`
- `grid[i][j]` is either `'/'`, `'\'`, or `' '`.

comment:

there are other solution as bfs and dfs, and my solution only beat 5% for both, so check better solutions.

Solution: union find

```cpp
using iii=tuple<int,int,int>;
int u=0, l=1, d=2, r=3;

class Solution {
public:
    unordered_map<string,string> father;
    int n;
    int sz;

    //with pass compression
    // O(1)
    string find(string a){
        if(!father.count(a)) father[a]=a;
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    bool Union(iii a, iii b) {
        auto[x,y,_] = b;
        if(x<0 || x>n-1 || y<0 || y>n-1) return false;
        return Union2(iii2str(a), iii2str(b));
    }
    //union - union roots, tree like
    // O(1)
    bool Union2(string a, string b) {
        string root_a = find(a);
        string root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            sz--;
            return true;
        }
        return false;
    }

    string iii2str(iii a){
        auto[x,y,k] = a;
        return to_string(x)+"_"+to_string(y)+"_"+to_string(k);
    }

    int regionsBySlashes(vector<string>& grid) {
        this->n=grid.size();
        this->sz=n*n*4;
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==' '){
                    Union({i,j,u},{i,j,d});
                    Union({i,j,d},{i,j,l});
                    Union({i,j,l},{i,j,r});
                }
                else if(grid[i][j]=='/'){
                    Union({i,j,u},{i,j,l});
                    Union({i,j,d},{i,j,r});
                }
                else if(grid[i][j]=='\\'){
                    Union({i,j,u},{i,j,r});
                    Union({i,j,d},{i,j,l});
                }
            }
        }

        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==' '){
                    Union({i,j,u},{i-1,j,d});
                    Union({i,j,u},{i,j-1,r});
                    Union({i,j,u},{i,j+1,l});
                    Union({i,j,u},{i+1,j,u});
                }
                else if(grid[i][j]=='/'){
                    Union({i,j,u},{i,j-1,r});
                    Union({i,j,u},{i-1,j,d});
                    Union({i,j,d},{i,j+1,l});
                    Union({i,j,d},{i+1,j,u});
                }
                else if(grid[i][j]=='\\'){
                    Union({i,j,u},{i,j+1,l});
                    Union({i,j,u},{i-1,j,d});
                    Union({i,j,d},{i,j-1,r});
                    Union({i,j,d},{i+1,j,u});
                }
            }
        }
        
        return sz;
    }
};

/*
connected component: union find
for each cell
    divide to 4 pieces, up down left right
*/
```