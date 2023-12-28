# Construct Quad Tree

#: 427
Difficult: Medium
Tags: Array, Divide and Conquer, Matrix, Tree
link: https://leetcode.com/problems/construct-quad-tree/
Priority: High
Created time: June 6, 2023 12:59 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given a `n * n` matrix `grid` of `0's` and `1's` only. We want to represent `grid` with a Quad-Tree.

Return *the root of the Quad-Tree representing* `grid`.

A Quad-Tree is a tree data structure in which each internal node has exactly four children. Besides, each node has two attributes:

- `val`: True if the node represents a grid of 1's or False if the node represents a grid of 0's. Notice that you can assign the `val` to True or False when `isLeaf` is False, and both are accepted in the answer.
- `isLeaf`: True if the node is a leaf node on the tree or False if the node has four children.

```
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
```

We can construct a Quad-Tree from a two-dimensional area using the following steps:

1. If the current grid has the same value (i.e all `1's` or all `0's`) set `isLeaf` True and set `val` to the value of the grid and set the four children to Null and stop.
2. If the current grid has different values, set `isLeaf` to False and set `val` to any value and divide the current grid into four sub-grids as shown in the photo.
3. Recurse for each of the children with the proper sub-grid.

![https://assets.leetcode.com/uploads/2020/02/11/new_top.png](https://assets.leetcode.com/uploads/2020/02/11/new_top.png)

If you want to know more about the Quad-Tree, you can refer to the [wiki](https://en.wikipedia.org/wiki/Quadtree).

**Quad-Tree format:**

You don't need to read this section for solving the problem. This is only if you want to understand the output format here. The output represents the serialized format of a Quad-Tree using level order traversal, where `null` signifies a path terminator where no node exists below.

It is very similar to the serialization of the binary tree. The only difference is that the node is represented as a list `[isLeaf, val]`.

If the value of `isLeaf` or `val` is True we represent it as **1** in the list `[isLeaf, val]` and if the value of `isLeaf` or `val` is False we represent it as **0**.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/02/11/grid1.png](https://assets.leetcode.com/uploads/2020/02/11/grid1.png)

```
Input: grid = [[0,1],[1,0]]
Output: [[0,1],[1,0],[1,1],[1,1],[1,0]]
Explanation: The explanation of this example is shown below:
Notice that 0 represnts False and 1 represents True in the photo representing the Quad-Tree.

```

![https://assets.leetcode.com/uploads/2020/02/12/e1tree.png](https://assets.leetcode.com/uploads/2020/02/12/e1tree.png)

**Example 2:**

![https://assets.leetcode.com/uploads/2020/02/12/e2mat.png](https://assets.leetcode.com/uploads/2020/02/12/e2mat.png)

```
Input: grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
Output: [[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
Explanation: All values in the grid are not the same. We divide the grid into four sub-grids.
The topLeft, bottomLeft and bottomRight each has the same value.
The topRight have different values so we divide it into 4 sub-grids where each has the same value.
Explanation is shown in the photo below:

```

![https://assets.leetcode.com/uploads/2020/02/12/e2tree.png](https://assets.leetcode.com/uploads/2020/02/12/e2tree.png)

**Constraints:**

- `n == grid.length == grid[i].length`
- `n == 2x` where `0 <= x <= 6`

```cpp
/*
// Definition for a QuadTree node.
class Node {
public:
    bool val;
    bool isLeaf;
    Node* topLeft;
    Node* topRight;
    Node* bottomLeft;
    Node* bottomRight;
    
    Node() {
        val = false;
        isLeaf = false;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }
    
    Node(bool _val, bool _isLeaf) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }
    
    Node(bool _val, bool _isLeaf, Node* _topLeft, Node* _topRight, Node* _bottomLeft, Node* _bottomRight) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = _topLeft;
        topRight = _topRight;
        bottomLeft = _bottomLeft;
        bottomRight = _bottomRight;
    }
};
*/

class Solution {
public:
    vector<vector<int>> grid;
        
    Node* construct(vector<vector<int>>& grid) {
        this->grid=grid;
        int n=grid.size();
        //build prefix sum
        /*
        vector<vector<int>> prefixSum(grid);
        for(int j=1; j<n; j++){
            prefixSum[0][j]+=prefixSum[0][j-1];
        }
        for(int i=1; i<n; i++){
            prefixSum[i][0]+=prefixSum[i-1][0];
        }
        for(int i=1;i<n;i++){
            for(int j=1; j<n; j++){
                prefixSum[i][j]+=prefixSum[i][j-1]+prefixSum[i-1][j]-prefixSum[i-1][j-1];
            }
        }
        */
        
        return construct2(0,0,n-1,n-1);
    }
    Node* construct2(int x1, int y1, int x2, int y2) {
        if(isAll0(x1,y1,x2,y2)){ return new Node(0,1); }//val, isLeaf
        if(isAll1(x1,y1,x2,y2)){ return new Node(1,1); }
        vector<vector<int>> nxt=divide(x1,y1,x2,y2);
        vector<int> tl=nxt[0], tr=nxt[1], bl=nxt[2], br=nxt[3];
        return new Node(1,0,
                       construct2(tl[0],tl[1],tl[2],tl[3]),
                       construct2(tr[0],tr[1],tr[2],tr[3]),
                       construct2(bl[0],bl[1],bl[2],bl[3]),
                       construct2(br[0],br[1],br[2],br[3])
                       );
    }
    
    bool isAll0(int x1, int y1, int x2, int y2){
        for(int i=x1;i<=x2;i++){
            for(int j=y1; j<=y2; j++){
                if(grid[i][j]!=0) return false;
            }
        }
        return true;
    }
    
    bool isAll1(int x1, int y1, int x2, int y2){
        for(int i=x1;i<=x2;i++){
            for(int j=y1; j<=y2; j++){
                if(grid[i][j]!=1) return false;
            }
        }
        return true;
    }
    
    //same seq with Node
    vector<vector<int>> divide(int x1,int y1,int x2,int y2){
        return {
            {x1, y1, (x2+x1)/2, (y2+y1)/2},
            {x1, (y2+y1)/2+1, (x2+x1)/2, y2},
            {(x2+x1)/2+1, y1, x2, (y2+y1)/2},
            {(x2+x1)/2+1, (y2+y1)/2+1, x2, y2}
        };
    }
};
```