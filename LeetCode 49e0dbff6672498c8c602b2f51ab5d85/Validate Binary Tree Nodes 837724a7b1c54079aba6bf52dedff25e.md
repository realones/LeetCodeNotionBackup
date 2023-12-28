# Validate Binary Tree Nodes

#: 1361
Difficult: Medium
Tags: BFS, DFS, Graph, Tree, Union Find
link: https://leetcode.com/problems/validate-binary-tree-nodes/
Priority: Medium
Created time: October 16, 2023 5:26 PM
Last edited time: October 16, 2023 5:27 PM
source: leetcode_daily

You have `n` binary tree nodes numbered from `0` to `n - 1` where node `i` has two children `leftChild[i]` and `rightChild[i]`, return `true` if and only if **all** the given nodes form **exactly one** valid binary tree.

If node `i` has no left child then `leftChild[i]` will equal `-1`, similarly for the right child.

Note that the nodes have no values and that we only use the node numbers in this problem.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/08/23/1503_ex1.png](https://assets.leetcode.com/uploads/2019/08/23/1503_ex1.png)

```
Input: n = 4, leftChild = [1,-1,3,-1], rightChild = [2,-1,-1,-1]
Output: true

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/08/23/1503_ex2.png](https://assets.leetcode.com/uploads/2019/08/23/1503_ex2.png)

```
Input: n = 4, leftChild = [1,-1,3,-1], rightChild = [2,3,-1,-1]
Output: false

```

**Example 3:**

![https://assets.leetcode.com/uploads/2019/08/23/1503_ex3.png](https://assets.leetcode.com/uploads/2019/08/23/1503_ex3.png)

```
Input: n = 2, leftChild = [1,0], rightChild = [-1,-1]
Output: false

```

**Constraints:**

- `n == leftChild.length == rightChild.length`
- `1 <= n <= 104`
- `1 <= leftChild[i], rightChild[i] <= n - 1`

找全充分条件

```cpp
class Solution {
public:
    //init
    vector<int> father;//use hash_map<type,type> if not using idx as identifier
    void init(int n) {//not required if using hash_map, init in begining of find()
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
    
    bool validateBinaryTreeNodes(int n, vector<int>& leftChild, vector<int>& rightChild) {
        init(n);
        int cnt=0;//cnt of edge
        unordered_map<int,int> ind;
        for(int i=0;i<n;i++){
            if(leftChild[i]==-1) continue;
            if(++ind[leftChild[i]]>1) return false;
            if(!Union(leftChild[i],i)) return false;
            cnt++;
        }
        for(int i=0;i<n;i++){
            if(rightChild[i]==-1) continue;
            if(++ind[rightChild[i]]>1) return false;
            if(!Union(rightChild[i],i)) return false; 
            cnt++;
        }
        return cnt==n-1;
    }
};

/*
tree: 
    1.all connected
    2.ind: root:0, other:1
    3.outd: leaves:0, other: 1~2
    4.cntOfEdge: n-1
*/
```