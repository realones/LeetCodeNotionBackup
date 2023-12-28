# Triangle

#: 120
Difficult: Medium
Tags: BFS, Bottom_Up, DP, Triangle
link: https://leetcode.com/problems/triangle
Priority: High
Created time: August 16, 2022 12:06 PM
Last edited time: October 22, 2023 12:12 AM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初DP

Given a `triangle` array, return *the minimum path sum from top to bottom*.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example 1:**

```
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
23 4
 65 7
41 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).

```

**Example 2:**

```
Input: triangle = [[-10]]
Output: -10

```

**Constraints:**

- `1 <= triangle.length <= 200`
- `triangle[0].length == 1`
- `triangle[i].length == triangle[i - 1].length + 1`
- `104 <= triangle[i][j] <= 104`

**Follow up:**

Could you do this using only

```
O(n)
```

extra space, where

```
n
```

is the total number of rows in the triangle?

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        // i -> i and i+1
        int m=triangle.size(); if(m==0) {return 0;}
        for(int i=m-2;i>=0;i--){
            for(int j=0;j<triangle[i].size();j++){
                triangle[i][j]+=min(triangle[i+1][j],triangle[i+1][j+1]);
            }
        }
        return triangle.front().front();
    }
};
```

解决方法:

- DFS: Traverse
- DFS: Divide Conquer
- Divide Conquer + Memorization
- Dynamic Programming
    - recursively
    - iteratively
        - top down
        - bottom up

DFS: Traverse

```cpp
T: O(2^m) m is the height of triangle
S: O(m) in stack, function call stack; O(1) in heap

void traverse(int x, int y, int sum){
    sum+=A[x][y];    

    if(x==bottom){
        //found a whole path from top to bottom
        if(sum < best){
            best = sum;
        }
    }

    traverse(x+1, y   ,sum);
    traverse(x+1, y+1 ,sum);
}

best = INT_MAX;
traverse(0,0,0);
// best is the result
```

DFS: Divide Conquer

```cpp
T: O(2^m) m is the height of triangle
S: O(m) in stack, function call stack; O(1) in heap

// return minimun path from (x, y) to bottom
int divideConquer(int x, int y){
    if(x==bottom) return 0;

    return A[x][y] + min(
                        divideConquer(x+1, y)
                        divideConquer(x+1, y+1));
}

return divideConquer(0, 0);
```

DFS: Divide Conquer + Memorization

```cpp
T: O(k) k is the number of nodes, (k^2 == m)
S: O(m) in stack, function call stack; O(k) in heap

// return minimun path from (x, y) to bottom
int divideConquer(int x, int y){
    // row index from 0 to n-1
    if(x==n) return 0;

    // if we already got the minimum path from (x,y) to bottom, just use it
    if(hash[x][y] != INT_MAX) return hash[x][y];
    
    //set before return
    hash[x][y] = A[x][y] + min(
                        divideConquer(x+1, y)
                        divideConquer(x+1, y+1));
    
    return hash[x][y];
}

//init: hash[*][*] = INT_MAX;
divideConquer(0, 0);
```

DP - interation: top down - bfs

```cpp
T: O(k) k is the number of nodes, (k^2 == m)
S: O(1) in stack, function call stack; O(k) in heap, if can change on original input, then O(1)

//状态定义
f[i][j]: 从0,0出发到i,j的最小路径长度

//init: 起点先有值
f[0][0] = A[0][0], //optional 初始化三角形的左边和右边来优化

//interatively to find the result
for(int i=1; i<n; i++){
    for(int j=0; j<=i ;j++){
        f[i][j] = min(f[i-1][j-1], f[i-1][j]) + A[i][j]; //处理一下越界
    }
}

//result：bottom中的最小值
min(f[n-1][0], f[n-1][1], …, f[n-1][n-1])
```

DP - interation - bottom up - bfs

```cpp
T: O(k) k is the number of nodes, (k^2 == m)
S: O(1) in stack, function call stack; O(k) in heap, if can change on original input, then O(1)

//状态定义
f[i][j]: 从i,j出发到bottom的最小路径长度

//init: 终点先有值
for(int i=0; i<n; i++){
    f[n-1][i] = A[n-1][i];
}

//interatively to find the result
for(int i=n-2; i>=0; i--){
    for(int j=0; j<=i ;j++){
        f[i][j] = min(f[i+1][j], f[i+1][j+1]) + A[i][j];
    }
}

//result：起点
f[0][0]
```