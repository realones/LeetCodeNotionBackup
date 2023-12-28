# Minimum Moves to Reach Target with Rotations

#: 1210
Difficult: Hard
Tags: BFS, Matrix
link: https://leetcode.com/problems/minimum-moves-to-reach-target-with-rotations/
Priority: Medium
Created time: September 23, 2023 10:20 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

In an `n*n` grid, there is a snake that spans 2 cells and starts moving from the top left corner at `(0, 0)` and `(0, 1)`. The grid has empty cells represented by zeros and blocked cells represented by ones. The snake wants to reach the lower right corner at `(n-1, n-2)` and `(n-1, n-1)`.

In one move the snake can:

- Move one cell to the right if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Move down one cell if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Rotate clockwise if it's in a horizontal position and the two cells under it are both empty. In that case the snake moves from `(r, c)` and `(r, c+1)` to `(r, c)` and `(r+1, c)`.
    
    ![https://assets.leetcode.com/uploads/2019/09/24/image-2.png](https://assets.leetcode.com/uploads/2019/09/24/image-2.png)
    
- Rotate counterclockwise if it's in a vertical position and the two cells to its right are both empty. In that case the snake moves from `(r, c)` and `(r+1, c)` to `(r, c)` and `(r, c+1)`.
    
    ![https://assets.leetcode.com/uploads/2019/09/24/image-1.png](https://assets.leetcode.com/uploads/2019/09/24/image-1.png)
    

Return the minimum number of moves to reach the target.

If there is no way to reach the target, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/09/24/image.png](https://assets.leetcode.com/uploads/2019/09/24/image.png)

```
Input: grid = [[0,0,0,0,0,1],
               [1,1,0,0,1,0],
               [0,0,0,0,1,1],
               [0,0,1,0,1,0],
               [0,1,1,0,0,0],
               [0,1,1,0,0,0]]
Output: 11
Explanation:
One possible solution is [right, right, rotate clockwise, right, down, down, down, down, rotate counterclockwise, right, down].

```

**Example 2:**

```
Input: grid = [[0,0,1,1,1,1],
               [0,0,0,0,1,1],
               [1,1,0,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,0]]
Output: 9

```

**Constraints:**

- `2 <= n <= 100`
- `0 <= grid[i][j] <= 1`
- It is guaranteed that the snake starts at empty cells.

```cpp
using iii=tuple<int,int,int>;

class Solution {
public:
    int minimumMoves(vector<vector<int>>& matrix) {
        //0 lr, 1 ud
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        //x,y,0lr/1ud
        vector<vector<vector<int>>> visited(m,vector<vector<int>>(n,vector<int>(2,0)));
        queue<iii> q;
        q.emplace(0,0,0);
        visited[0][0][0]=1;
        function valid = [&](int x, int y)->bool{
            return x<m && y<n && matrix[x][y]==0;
        };
        function push = [&](int x, int y, int t)->void{
            if(visited[x][y][t]) return;
            q.emplace(x,y,t);
            visited[x][y][t]=true;
        };
        int step=0;
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                auto [x,y,t] = q.front(); q.pop();
                if (iii{x,y,t} == iii{m-1,n-2,0}) return step;
                if(t==0){
                    if(valid(x+1,y) && valid(x+1,y+1)) push(x,y,1);//rotate
                    if(valid(x,y+2)) push(x,y+1,0);//right
                    if(valid(x+1,y) && valid(x+1,y+1)) push(x+1,y,0);//down
                }
                else{
                    if(valid(x,y+1) && valid(x+1,y+1)) push(x,y,0);//rotate
                    if(valid(x,y+1) && valid(x+1,y+1)) push(x,y+1,1);//right
                    if(valid(x+2,y)) push(x+1,y,1);//down
                }
            }
            step++;
        }
        return -1;
    }
};

/*

*/
```