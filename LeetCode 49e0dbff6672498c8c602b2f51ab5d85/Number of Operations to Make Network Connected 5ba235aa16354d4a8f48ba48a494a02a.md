# Number of Operations to Make Network Connected

#: 1319
Difficult: Medium
Tags: BFS, DFS, Graph, Union Find
link: https://leetcode.com/problems/number-of-operations-to-make-network-connected/description/
Priority: Low
Created time: June 28, 2023 12:22 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

There are `n` computers numbered from `0` to `n - 1` connected by ethernet cables `connections` forming a network where `connections[i] = [ai, bi]` represents a connection between computers `ai` and `bi`. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network `connections`. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return *the minimum number of times you need to do this in order to make all the computers connected*. If it is not possible, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/02/sample_1_1677.png](https://assets.leetcode.com/uploads/2020/01/02/sample_1_1677.png)

```
Input: n = 4, connections = [[0,1],[0,2],[1,2]]
Output: 1
Explanation: Remove cable between computer 1 and 2 and place between computers 1 and 3.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/01/02/sample_2_1677.png](https://assets.leetcode.com/uploads/2020/01/02/sample_2_1677.png)

```
Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2],[1,3]]
Output: 2

```

**Example 3:**

```
Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2]]
Output: -1
Explanation: There are not enough cables.

```

**Constraints:**

- `1 <= n <= 105`
- `1 <= connections.length <= min(n * (n - 1) / 2, 105)`
- `connections[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- There are no repeated connections.
- No two computers are connected by more than one cable.

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

    int makeConnected(int n, vector<vector<int>>& connections) {
        int additionCnt=0, componentCnt=n;
        init(n);
        for(auto& c: connections){
            int a=c[0],b=c[1];
            if(Union(a,b)) componentCnt--;
            else additionCnt++;
        }
        if(additionCnt < componentCnt-1) return -1;
        return componentCnt-1;
    }
};

/*
if union fail, means additional edges -> get cnt of addtional
number of component -1 means edges needed to connect all

true when cnt_addtional >= cnt_component-1
*/
```