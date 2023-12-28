# Reconstruct Itinerary

#: 332
Difficult: Hard
Tags: DFS, EulerianPath, Graph
link: https://leetcode.com/problems/reconstruct-itinerary/
Priority: High
Status: Dig More, No Idea At First, todo
Created time: June 27, 2023 11:53 PM
Last edited time: October 17, 2023 6:28 PM
source: HuaHua

You are given a list of airline `tickets` where `tickets[i] = [fromi, toi]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

- For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg](https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg)

```
Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg](https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg)

```
Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.

```

**Constraints:**

- `1 <= tickets.length <= 300`
- `tickets[i].length == 2`
- `fromi.length == 3`
- `toi.length == 3`
- `fromi` and `toi` consist of uppercase English letters.
- `fromi != toi`

Comment: 

new algorithm: Eulerian Path

关于lexi order的部分有点晕

Solution 1:

```cpp
class Solution {
public:
    unordered_map<string, queue<string>> g;//queue small->large, pop small one

    vector<string> findItinerary(vector<vector<string>>& tickets) {
        sort(tickets.begin(), tickets.end());//same from, sort tos -> finally keep lexical order in output
        for(auto& t: tickets){
            g[t[0]].push(t[1]);
        }
        return dfs("JFK");
    }

    vector<string> dfs(string start){
        vector<vector<string>> paths;
        while(!g[start].empty()){//at most two paths: B->path2->path1
            string nxt=g[start].front(); g[start].pop();//pick the smallest lexical order nxt
            paths.push_back(dfs(nxt));
        }
        vector<string> res({start});//since start is not included in next dfs
        for(int i=paths.size()-1; i>=0; i--){
            for(auto p: paths[i]){
                res.push_back(p);
            }
        }
        return res;
    }
};

//Eulerian Path
```

Solution improved from above:

```cpp
class Solution {
public:
    unordered_map<string, queue<string>> g;//queue small->large, pop small one
    vector<string> res;

    vector<string> findItinerary(vector<vector<string>>& tickets) {
        sort(tickets.begin(), tickets.end());//same from, sort tos -> finally keep lexical order in output
        for(auto& t: tickets){
            g[t[0]].push(t[1]);
        }
        dfs("JFK");
        reverse(res.begin(), res.end());
        return res;
    }

    void dfs(string start){
        while(!g[start].empty()){//at most two paths: path1 -> path2 -> B
            string nxt=g[start].front(); g[start].pop();
            dfs(nxt);
        }
        res.push_back(start);
    }
};

//Eulerian Path
```