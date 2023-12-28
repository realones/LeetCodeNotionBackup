# Flower Planting With No Adjacent

#: 1042
Difficult: Medium
Tags: Bipartition_graphColoring, Graph
link: https://leetcode.com/problems/flower-planting-with-no-adjacent/description/
Priority: Medium
Status: HardToImplement, More Solution
Created time: August 24, 2023 5:21 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Possible Bipartition (Possible%20Bipartition%2091625744774f4c66a944f881af8611a8.md), Is Graph Bipartite? (Is%20Graph%20Bipartite%2045c1135455a441f6b56570aee649ac62.md)

You have `n` gardens, labeled from `1` to `n`, and an array `paths` where `paths[i] = [xi, yi]` describes a bidirectional path between garden `xi` to garden `yi`. In each garden, you want to plant one of 4 types of flowers.

All gardens have **at most 3** paths coming into or leaving it.

Your task is to choose a flower type for each garden such that, for any two gardens connected by a path, they have different types of flowers.

Return ***any** such a choice as an array* `answer`*, where* `answer[i]` *is the type of flower planted in the* `(i+1)th` *garden. The flower types are denoted* `1`*,* `2`*,* `3`*, or* `4`*. It is guaranteed an answer exists.*

**Example 1:**

```
Input: n = 3, paths = [[1,2],[2,3],[3,1]]
Output: [1,2,3]
Explanation:
Gardens 1 and 2 have different types.
Gardens 2 and 3 have different types.
Gardens 3 and 1 have different types.
Hence, [1,2,3] is a valid answer. Other valid answers include [1,2,4], [1,4,2], and [3,2,1].

```

**Example 2:**

```
Input: n = 4, paths = [[1,2],[3,4]]
Output: [1,2,1,2]

```

**Example 3:**

```
Input: n = 4, paths = [[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]
Output: [1,2,3,4]

```

**Constraints:**

- `1 <= n <= 104`
- `0 <= paths.length <= 2 * 104`
- `paths[i].length == 2`
- `1 <= xi, yi <= n`
- `xi != yi`
- Every garden has **at most 3** paths coming into or leaving it.

comment: 弱化版本的bipartition, 但是花了不少时间debug

原因： if(availableType & 1<<(mp[nei])) 这段没有加，看来bit op不熟悉，而且还是写成funciton比较容易unit test，或者用笨办法先实现，例如用个set（1，2，3，4）😭

```cpp
class Solution {
public:
    vector<int> res;
    vector<int> gardenNoAdj(int n, vector<vector<int>>& paths) {
        res.resize(n,-1);
        //build graph
        unordered_map<int, unordered_set<int>> g;//garden_id - [garden_id]
        for(auto& p: paths){
            int x=p[0]-1, y=p[1]-1;//change to 0 based
            g[x].insert(y);
            g[y].insert(x);
        }

        //bfs
        unordered_map<int,int> mp;//garden_id - flower_type
        vector<int> visited(n,0);//mp is not enough since flower type decided when popped from queue during processing, but not when insert into q
        //for loop to handle each connected component
        for(int i=0; i<n; i++){
            if(visited[i]) continue;
            bfs(g,mp,visited,i);
        }
        return res;
    }

    //seems dfs is easier
    void bfs(unordered_map<int,unordered_set<int>>& g, unordered_map<int,int>& mp, vector<int>& visited, int start){
        queue<int> q;
        visited[start]=1;
        q.push(start);

        while(!q.empty()){
            int cur=q.front(); q.pop();
            //process
            int availableType=(1<<4)-1;//type 0,1,2,3
            for(auto nei: g[cur]){
                if(visited[nei]) {
                    if(mp.count(nei)) {
                        if(availableType & 1<<(mp[nei]))
                            availableType -= 1<<(mp[nei]);
                    }
                    //since guaranteed exist, no need to check conflict
                    continue;
                }
                visited[nei]=1;
                q.push(nei);
            }
            mp[cur]=nxtAvailable(availableType);
            res[cur]=mp[cur]+1;
        }
    }

    int nxtAvailable(int state){
        for(int i=0; i<4; i++){
            if(state & (1<<i)) return i;
        }
        std::cout << "error" << std::endl;
        return -1;
    }
};

/*
gardens: 1,2,..,n
paths : [{x,y}], bidirection

flower type: 1,2,3,4
ind/outd: at most 3

return a solution(guaranteed to exist at least one) that two same adjacent flowers
================================
bfs - shared visited mp<garden_id, flower_type>
*/
```