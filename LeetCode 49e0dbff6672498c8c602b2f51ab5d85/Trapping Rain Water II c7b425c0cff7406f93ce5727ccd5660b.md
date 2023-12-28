# Trapping Rain Water II

#: 407
Difficult: Hard
Tags: PQ, mbfs_preChkVisited_woSZ
link: https://leetcode.com/problems/trapping-rain-water-ii/
Priority: Medium
Created time: September 1, 2022 9:52 PM
Last edited time: October 28, 2023 1:28 PM
practicedTimes: 1
source: jiuzhang, 算高Heap&Stack

similar to mbfs_preChkVisited_woSZ, but use pq not q.

Given an `m x n` integer matrix `heightMap` representing the height of each unit cell in a 2D elevation map, return *the volume of water it can trap after raining*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/08/trap1-3d.jpg](https://assets.leetcode.com/uploads/2021/04/08/trap1-3d.jpg)

```
Input: heightMap = [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]
Output: 4
Explanation: After the rain, water is trapped between the blocks.
We have two small ponds 1 and 3 units trapped.
The total volume of water trapped is 4.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/04/08/trap2-3d.jpg](https://assets.leetcode.com/uploads/2021/04/08/trap2-3d.jpg)

```
Input: heightMap = [[3,3,3,3,3],[3,2,2,2,3],[3,2,1,2,3],[3,2,2,2,3],[3,3,3,3,3]]
Output: 10

```

**Constraints:**

- `m == heightMap.length`
- `n == heightMap[i].length`
- `1 <= m, n <= 200`
- `0 <= heightMap[i][j] <= 2 * 104`

```cpp
class Cell{
public:
  int x,y,h;
  Cell(){}
  Cell(int xx,int yy, int hh){
    x = xx;
    y = yy;
    h = hh;
  }
};

class MyCompare{
public:
  int operator()(Cell x, Cell y){
    return (x.h > y.h);
  }
};

class Solution {
public:
    /*
     * @param heights: a matrix of integers
     * @return: an integer
     */
    int trapRainWater(vector<vector<int>> &heights) {
        // write your code here
        int res=0;
        vector<int> dx = {1,-1,0,0};
        vector<int> dy = {0,0,1,-1}; 
        priority_queue<Cell,vector<Cell>,MyCompare> q;
        int n = heights.size();
        int m = heights[0].size();
        
        vector<vector<int>> visit(n,vector<int>(m,0));
        //put the outside walls into queue
        for(int i=0;i<n;i++){
            q.push(Cell(i,0,heights[i][0]));
            visit[i][0] = 1;
            q.push(Cell(i,m-1,heights[i][m-1]));
            visit[i][m-1] = 1;
        }
        for(int i=0;i<m;i++){
            q.push(Cell(0,i,heights[0][i]));
            visit[0][i] = 1;
            q.push(Cell(n-1,i,heights[n-1][i]));
            visit[n-1][i] = 1;
        }
        
        //main logic
        while(!q.empty()){
            Cell now=q.top();
            q.pop();
            
            for(int i=0;i<4;i++){
                //方向数组
                int nx=now.x+dx[i];
                int ny=now.y+dy[i];
                
                if(0<=nx && nx < n && 0 <= ny && ny < m && visit[nx][ny] == 0){
                    visit[nx][ny]=1;
                    if(heights[nx][ny]<now.h){
                        res+=now.h-heights[nx][ny];
                        //care: push the height of cell+water
                        q.push(Cell(nx,ny,now.h));
                    }
                    else{
                        q.push(Cell(nx,ny,heights[nx][ny]));
                    }
                }
            }
        }
        
        return res;
    }
};
```