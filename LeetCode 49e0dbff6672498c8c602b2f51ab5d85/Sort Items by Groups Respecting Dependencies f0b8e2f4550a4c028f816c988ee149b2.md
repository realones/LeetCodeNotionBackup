# Sort Items by Groups Respecting Dependencies

#: 1203
Difficult: Hard
Tags: BFS, DFS, Graph, TopSorting
link: https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/description/
Priority: High
Status: HardToImplement, No Idea At First
Created time: August 20, 2023 10:49 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

There are `n` items each belonging to zero or one of `m` groups where `group[i]` is the group that the `i`-th item belongs to and it's equal to `-1` if the `i`-th item belongs to no group. The items and the groups are zero indexed. A group can have no item belonging to it.

Return a sorted list of the items such that:

- The items that belong to the same group are next to each other in the sorted list.
- There are some relations between these items where `beforeItems[i]` is a list containing all the items that should come before the `i`th item in the sorted array (to the left of the `i`th item).

Return any solution if there is more than one solution and return an **empty list** if there is no solution.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/09/11/1359_ex1.png](https://assets.leetcode.com/uploads/2019/09/11/1359_ex1.png)

```
Input: n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3,6],[],[],[]]
Output: [6,3,4,1,5,2,0,7]

```

**Example 2:**

```
Input: n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3],[],[4],[]]
Output: []
Explanation: This is the same as example 1 except that 4 needs to be before 6 in the sorted list.

```

**Constraints:**

- `1 <= m <= n <= 3 * 104`
- `group.length == beforeItems.length == n`
- `1 <= group[i] <= m - 1`
- `0 <= beforeItems[i].length <= n - 1`
- `0 <= beforeItems[i][j] <= n - 1`
- `i != beforeItems[i][j]`
- `beforeItems[i]` does not contain duplicates elements.

comment: 

no idea, checked wisdom peak, but still low performance, need to check more solution

Solution TLE:

```cpp
class Solution {
public:
    vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) {
        //build graph
        unordered_map<int,unordered_set<int>> g;
        for(int i=0; i<beforeItems.size(); i++){
            int to=i;
            for(auto from: beforeItems[i]){
                g[from].insert(to);
            }
        }
        //build groupItems
        unordered_map<int,unordered_set<int>> groupItems;//group -- items
        for(int i=0;i<group.size();i++){
            if(group[i]==-1) group[i]=m+i;//make unique id and easy to calculate itemId back
            groupItems[group[i]].insert(i);
        }
        //topSort in each group
        unordered_map<int,vector<int>> group_seqItems;
        for(auto& [groupId, items]: groupItems){
            group_seqItems[groupId]=topSort(g,items);
            if(group_seqItems[groupId].empty()) return {};
        }
        //transfer beforeItems => beforeGroups
        unordered_map<int,unordered_set<int>> groupGraph;
        for(auto& [from_item, to_items]: g){
            for(auto to_item: to_items){
                if(group[to_item]!=group[from_item])
                    groupGraph[group[from_item]].insert(group[to_item]);
            }
        }
        //between groups, topSort
        unordered_set<int> groupSt;
        for(auto& [groupId,_]: groupItems) groupSt.insert(groupId);
        vector<int> groupOrder=topSort(groupGraph,groupSt);
        if(groupGraph.empty()) return {};
        //transfer group order to item order
        vector<int> res;
        for(auto groupId: groupOrder){
            res.insert(res.end(), group_seqItems[groupId].begin(), group_seqItems[groupId].end());
        }
        return res;
    }

    vector<int> topSort(unordered_map<int,unordered_set<int>>& graph, unordered_set<int>& whiteList){
        //calculate in degree for each node
        unordered_map<int, int> ind;
        for(auto& [from, tos]: graph){
            if(!whiteList.count(from)) continue;
            for(auto to: tos){
                if(!whiteList.count(to)) continue;
                ind[to]++;
            }
        }
        //push nodes with 0 indegree into queue
        queue<int> q;
        for(auto from: whiteList){
            if(ind[from]==0){
                q.push(from);
            }
        }
        //top sort and process - bfs
        vector<int> res;
        while(!q.empty()){
            int cur = q.front(); q.pop();
            //process
            res.push_back(cur);
            //recalculate indegree of neighbors of cur
            for(auto to: graph[cur]){
                if(!whiteList.count(to)) continue;
                ind[to]--;
                if(ind[to]==0){q.push(to);}
            }
        }
        if(res.size()<whiteList.size()) return {};
        return res;
    }
};

/*
    groupId - [list of items] : must be together
         -1 - [list of items] : can seperate

intui:
    beforeItems -> topSort of all items
    groups -> topSort between groups
    
impl:
    for each group(except -1), do top sort
    for groups and -1Items, organize the dependencies based on beforeItems and groupItems
    topSort on groups

details:
    build
        graph     : {node -> st{node}}
        groupItems: {groupId -> st{node}}
            if(groupId==-1, groupId=m+node;

    in each group, topSort (graph as dependencies, groupItem as filters)
    any one false, return flase
        
    between groups, topSort
    build dependencies:
        node id = group id
        transfer beforeItems => beforeGroups
    no filter required, reuse above topSort func
*/
```

Solution Passed:

```cpp
class Solution {
public:
    vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) {
        //build group_items
        unordered_map<int,unordered_set<int>> group_items;//group -- items
        for(int i=0;i<group.size();i++){
            if(group[i]==-1) group[i]=m+i;//make unique id and easy to calculate itemId back
            group_items[group[i]].insert(i);
        }
        //build graph
        unordered_map<int,unordered_set<int>> g;
        unordered_map<int,int> ind;
        for(int to=0; to<beforeItems.size(); to++){
            for(int from: beforeItems[to]){
                if(group[from]!=group[to]) continue;
                g[from].insert(to);
                ind[to]++;
            }
        }
        //topSort in each group
        unordered_map<int,vector<int>> group_seqItems;
        for(auto& [groupId, items]: group_items){
            group_seqItems[groupId]=topSort(g,ind,items);
            if(group_seqItems[groupId].empty()) return {};
        }
        //transfer beforeItems => beforeGroups
        g.clear();
        ind.clear();
        for(int to=0; to<beforeItems.size(); to++){
            for(int from: beforeItems[to]){
                if(group[from]==group[to]) continue;
                if(!g[group[from]].count(group[to])){
                    g[group[from]].insert(group[to]);
                    ind[group[to]]++;
                }
            }
        }
        //between groups, topSort
        unordered_set<int> groupSt;
        for(auto& [groupId,_]: group_items) groupSt.insert(groupId);
        vector<int> groupOrder=topSort(g,ind,groupSt);
        if(groupOrder.empty()) return {};
        //transfer group order to item order
        vector<int> res;
        for(auto groupId: groupOrder){
            res.insert(res.end(), group_seqItems[groupId].begin(), group_seqItems[groupId].end());
        }
        return res;
    }

    vector<int> topSort(unordered_map<int,unordered_set<int>>& graph, unordered_map<int,int>& ind, unordered_set<int>& whiteList){
        //push nodes with 0 indegree into queue
        queue<int> q;
        for(auto from: whiteList){
            if(ind[from]==0){
                q.push(from);
            }
        }
        //top sort and process - bfs
        vector<int> res;
        while(!q.empty()){
            int cur = q.front(); q.pop();
            //process
            res.push_back(cur);
            //recalculate indegree of neighbors of cur
            for(auto to: graph[cur]){
                ind[to]--;
                if(ind[to]==0){q.push(to);}
            }
        }
        if(res.size()<whiteList.size()) return {};
        return res;
    }
};

/*
    groupId - [list of items] : must be together
         -1 - [list of items] : can seperate

intui:
    beforeItems -> topSort of all items
    groups -> topSort between groups
    
impl:
    for each group(except -1), do top sort
    for groups and -1Items, organize the dependencies based on beforeItems and group_items
    topSort on groups

details:
    build
        graph     : {node -> st{node}}
        group_items: {groupId -> st{node}}
            if(groupId==-1, groupId=m+node;

    in each group, topSort (graph as dependencies, groupItem as filters)
    any one false, return flase
        
    between groups, topSort
    build dependencies:
        node id = group id
        transfer beforeItems => beforeGroups
    no filter required, reuse above topSort func
*/
```