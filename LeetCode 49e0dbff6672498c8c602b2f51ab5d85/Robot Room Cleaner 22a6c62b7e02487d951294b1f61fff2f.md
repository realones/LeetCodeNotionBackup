# Robot Room Cleaner

#: 489
Difficult: Hard
Tags: BackTracking, DFS, Matrix
link: https://leetcode.com/problems/robot-room-cleaner/
Priority: High
Status: No Idea At First, toSummarize
Created time: October 7, 2022 12:32 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

You are controlling a robot that is located somewhere in a room. The room is modeled as an `m x n` binary grid where `0` represents a wall and `1` represents an empty slot.

The robot starts at an unknown location in the room that is guaranteed to be empty, and you do not have access to the grid, but you can move the robot using the given API `Robot`.

You are tasked to use the robot to clean the entire room (i.e., clean every empty cell in the room). The robot with the four given APIs can move forward, turn left, or turn right. Each turn is `90` degrees.

When the robot tries to move into a wall cell, its bumper sensor detects the obstacle, and it stays on the current cell.

Design an algorithm to clean the entire room using the following APIs:

```
interface Robot {
  // returns true if next cell is open and robot moves into the cell.
  // returns false if next cell is obstacle and robot stays on the current cell.
  boolean move();

  // Robot will stay on the same cell after calling turnLeft/turnRight.
  // Each turn will be 90 degrees.
  void turnLeft();
  void turnRight();

  // Clean the current cell.
  void clean();
}

```

**Note** that the initial direction of the robot will be facing up. You can assume all four edges of the grid are all surrounded by a wall.

**Custom testing:**

The input is only given to initialize the room and the robot's position internally. You must solve this problem "blindfolded". In other words, you must control the robot using only the four mentioned APIs without knowing the room layout and the initial robot's position.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/07/17/lc-grid.jpg](https://assets.leetcode.com/uploads/2021/07/17/lc-grid.jpg)

```
Input: room = [[1,1,1,1,1,0,1,1],[1,1,1,1,1,0,1,1],[1,0,1,1,1,1,1,1],[0,0,0,1,0,0,0,0],[1,1,1,1,1,1,1,1]], row = 1, col = 3
Output: Robot cleaned all rooms.
Explanation: All grids in the room are marked by either 0 or 1.
0 means the cell is blocked, while 1 means the cell is accessible.
The robot initially starts at the position of row=1, col=3.
From the top left corner, its position is one row below and three columns right.

```

**Example 2:**

```
Input: room = [[1]], row = 0, col = 0
Output: Robot cleaned all rooms.

```

**Constraints:**

- `m == room.length`
- `n == room[i].length`
- `1 <= m <= 100`
- `1 <= n <= 200`
- `room[i][j]` is either `0` or `1`.
- `0 <= row < m`
- `0 <= col < n`
- `room[row][col] == 1`
- All the empty cells can be visited from the starting position.

Solution - back tracking

![Untitled](Robot%20Room%20Cleaner%2022a6c62b7e02487d951294b1f61fff2f/Untitled.png)

image you know the map and you can be anywhere without moving

- use forij to clean each cell

image you don’t know the map and you can be anywhere without moving

- use BFS or DFS to clean from starting point to each reachable cell
    - dfs: remember a chain of previous steps in call stack
    - bfs: remember a list of next cells in queue

image you don’t know the map and you can only remember a chain of previous steps

- DFS

 image you don’t know the map and you need to move cell by cell

- use DFS with backtracking
    - dfs: function call stack to remember cell status, which direction you have tried
    - backtracking: you need to actually move you ass back.

```cpp
/**
 * // This is the robot's control interface.
 * // You should not implement it, or speculate about its implementation
 * class Robot {
 *   public:
 *     // Returns true if the cell in front is open and robot moves into the cell.
 *     // Returns false if the cell in front is blocked and robot stays in the current cell.
 *     bool move();
 *
 *     // Robot will stay in the same cell after calling turnLeft/turnRight.
 *     // Each turn will be 90 degrees.
 *     void turnLeft();
 *     void turnRight();
 *
 *     // Clean the current cell.
 *     void clean();
 * };
 */

/*
Spiral Backtracking
    constrained programming: marked visited cell as virtual obstacles
    backtracking: go back to the cell offering an alternative path
    
right-hand rule - maze solving algorithm:
    go forward till obstacle, turn right, etc
    
    if obstacle again, turn right again
    
    how to explore the althernative path:
        go back to cell, turn right from last explored direction
            
stop:
    all 4 directions for each cell 
    
backtrack(x,y,d){
    mark cell as visited and clean up
    explore 4 directions from cur to turn right
        nx cell: if not visited yet and no obstacles
            move forward
            explore next cell
            back track
        turn right
}

*/

class Solution {
public:
    //up right down left
    vector<int> dx{-1,0,1,0};
    vector<int> dy{0,1,0,-1};
    
    unordered_set<string> cleaned; //x_y
    
    
    //go straight still obstacles or cleaned cells, turn right
    void cleanRoom(Robot& robot) {
        backtrack(robot,0,0,0);// pos: 0 0, direction: 0 - up
    }
    
    //recursive with backtracking and adding visited
    //each cell operation should gurantee all 4 direction tried, remember current direction in current cell operation(current recur function), and restore same diretion before go back to previous cell.
    //in the end, the robot go back to start point
    void backtrack(Robot& robot, int x, int y, int d){
        if (cleaned.count(getKey(x,y))) return;
        //mark as visited and clean
        robot.clean();
        cleaned.insert(getKey(x,y));
        
        //for 4 directions start from current direction
        //matter go or not go, will turn right anyway, after 4 operations, still facing the same direction, so backtracking can guarantee same direction
        for(int k=0; k<4; k++){
            //next cell coordination if go current direction
            int nx=x+dx[d];
            int ny=y+dy[d];
            
            if (!cleaned.count(getKey(nx,ny)) && robot.move()) {//if can go
                backtrack(robot, nx, ny, d);//go
                goBack(robot);//back to current cell from next one
            }

            //current direction is all clean, go to next diretion in current cell
            // turn the robot following chosen direction : clockwise
            d=(d+1)%4;
            robot.turnRight();
        }
    }
    
    //go back operation is just for go back, no cleaning, used for backtracking only
    void goBack(Robot& robot){
        //turn back
        robot.turnRight();
        robot.turnRight();
        //go back cell
        robot.move();
        //restore direction
        robot.turnRight();
        robot.turnRight();
    }
    
    string getKey(int x, int y){
        return to_string(x)+"_"+to_string(y);
    }
};
```

Solution - brute force - Time Limit Exceeded

```cpp
/**
 * // This is the robot's control interface.
 * // You should not implement it, or speculate about its implementation
 * class Robot {
 *   public:
 *     // Returns true if the cell in front is open and robot moves into the cell.
 *     // Returns false if the cell in front is blocked and robot stays in the current cell.
 *     bool move();
 *
 *     // Robot will stay in the same cell after calling turnLeft/turnRight.
 *     // Each turn will be 90 degrees.
 *     void turnLeft();
 *     void turnRight();
 *
 *     // Clean the current cell.
 *     void clean();
 * };
 */

//sigh, it turns out to be a brute force and always timesout, lot of useless stepsw

/*
spinning circle (always turn left and go)
+
remember each direction robot departed from each cell
visited[x,y,d]
*/
//d: 0up 1left 2down 3right
//   1.  10.   100   1000
//always turn left
class Solution {
public:
    //x y - 1101 - no down
    // total: 1+2+4+8=15
    map<string, int> visited; //key is x_y
    int k=0;//up
    //up left down right
    vector<int> dx{-1,0,1,0};
    vector<int> dy{0,-1,0,1};
    
    void cleanRoom(Robot& robot) {
        robot=robot;
        clean(robot,0,0);
    }
    
    void clean(Robot& robot, int x, int y) {
        if(visited[getKey(x,y)]==0) robot.clean();
        
        bool moved=false;
        int nx;
        int ny;
        while(moved==false){
            //turn left
            k=(k+1)%4;
            nx=x+dx[k];
            ny=y+dy[k];
            robot.turnLeft();

            //remember your attempted choice of direction for this cell
            if(visited[getKey(x,y)]&(1<<k)){continue;}
            visited[getKey(x,y)]|=(1<<k);
            // std::cout << "x y v: " << x << "-"<< y << "-"<< visited[getKey(x,y)] << std::endl;

            //check if can go
            if(visited[getKey(nx,ny)]!=15){
                moved=robot.move();
            } //not all direction full
            // std::cout << canMove << std::endl;
        }
        
        std::cout << moved << std::endl;
        if(moved) {
            if(k==0) std::cout << "Up" << std::endl;
                if(k==1) std::cout << "Left" << std::endl;
                if(k==2) std::cout << "Down" << std::endl;
                if(k==3) std::cout << "Right" << std::endl;
            clean(robot, nx, ny);
        }
    }
    
    string getKey(int x, int y){
        return to_string(x)+"_"+to_string(y);
    }
};
```