# Rank Transform of a Matrix

#: 1632
Difficult: Hard
Tags: Greedy, Matrix, Sort, TopSorting, Union Find
link: https://leetcode.com/problems/rank-transform-of-a-matrix/
Priority: High
Status: HardToImplement, No Idea At First, toRevisit, toSummarize
Created time: September 19, 2023 12:07 AM
Last edited time: October 17, 2023 6:24 PM
source: leetcode_daily

Given an `m x n` `matrix`, return *a new matrix* `answer` *where* `answer[row][col]` *is the **rank** of* `matrix[row][col]`.

The **rank** is an **integer** that represents how large an element is compared to other elements. It is calculated using the following rules:

- The rank is an integer starting from `1`.
- If two elements `p` and `q` are in the **same row or column**, then:
    - If `p < q` then `rank(p) < rank(q)`
    - If `p == q` then `rank(p) == rank(q)`
    - If `p > q` then `rank(p) > rank(q)`
- The **rank** should be as **small** as possible.

The test cases are generated so that `answer` is unique under the given rules.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/18/rank1.jpg](https://assets.leetcode.com/uploads/2020/10/18/rank1.jpg)

```
Input: matrix = [[1,2],[3,4]]
Output: [[1,2],[2,3]]
Explanation:
The rank of matrix[0][0] is 1 because it is the smallest integer in its row and column.
The rank of matrix[0][1] is 2 because matrix[0][1] > matrix[0][0] and matrix[0][0] is rank 1.
The rank of matrix[1][0] is 2 because matrix[1][0] > matrix[0][0] and matrix[0][0] is rank 1.
The rank of matrix[1][1] is 3 because matrix[1][1] > matrix[0][1], matrix[1][1] > matrix[1][0], and both matrix[0][1] and matrix[1][0] are rank 2.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/10/18/rank2.jpg](https://assets.leetcode.com/uploads/2020/10/18/rank2.jpg)

```
Input: matrix = [[7,7],[7,7]]
Output: [[1,1],[1,1]]

```

**Example 3:**

![https://assets.leetcode.com/uploads/2020/10/18/rank3.jpg](https://assets.leetcode.com/uploads/2020/10/18/rank3.jpg)

```
Input: matrix = [[20,-21,14],[-19,4,19],[22,-47,24],[-19,4,19]]
Output: [[4,2,3],[1,3,4],[5,1,6],[1,3,4]]

```

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 500`
- `109 <= matrix[row][col] <= 109`

**comment**:

1. very hard to implement, takes me lot of time, then checked wisdompeak
2. to resolve pain point
    1. how to handle the unioned group, it expanded to multi row/ col, how to solve the group ind and how it affect other cells
    2. e.g. "1 1 1 2" has only last 1->2 build graph and inc 2's ind
3. check greedy solution
4. check lee215, which has a very shorter solution

Solutions:

[https://github.com/wisdompeak/LeetCode/tree/master/Union_Find/1632.Rank-Transform-of-a-Matrix](https://github.com/wisdompeak/LeetCode/tree/master/Union_Find/1632.Rank-Transform-of-a-Matrix)

Solution: topSort + unionFind

```cpp
/*
todo:
handle pain point: e.g. "1 1 1 2" has only last 1->2 build graph and inc 2's ind
*/
class Solution {
public:
    using ii=pair<int,int>;
    int m,n;

    ii i2ii(int k){ return{k/n, k%n}; }
    int ii2i(ii c){ return c.first*n+c.second; }
    
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

    //====main================================
    vector<vector<int>> matrixRankTransform(vector<vector<int>>& matrix) {
        m=matrix.size(), n=matrix[0].size();//1~500
        //unionfind
        init(m*n);

        unordered_map<int, unordered_set<int>> g;//to be improved to vector<vector<int>> g(m*n)
        unordered_map<int,int> ind;//to be improved to vector<int> ind(m*n,0);

        //build graph&ind&union for each row
        for(int i=0; i<m; i++){//for each row
            vector<ii> tmp;//[(val,cell)]
            for(int j=0; j<n; j++){
                tmp.emplace_back(matrix[i][j], ii2i({i,j}));//i*n+j
            }
            sort(tmp.begin(), tmp.end());
            for(int j=0;j<n-1;j++){
                auto [curVal, curCell]=tmp[j]; auto [nxtVal, nxtCell]=tmp[j+1];
                if(curVal < nxtVal){
                    g[curCell].insert(nxtCell);
                    ind[nxtCell]++;
                }
                else{//==
                    Union(curCell,nxtCell);
                }
            }
        }

        //build graph&ind&union for each col
        for(int j=0; j<n; j++){//for each col
            vector<ii> tmp;//[(val,cell)]
            for(int i=0; i<m; i++){
                tmp.emplace_back(matrix[i][j], ii2i({i,j}));//i*n+j
            }
            sort(tmp.begin(), tmp.end());
            for(int i=0;i<m-1;i++){
                auto [curVal, curCell]=tmp[i]; auto [nxtVal, nxtCell]=tmp[i+1];
                if(curVal < nxtVal){
                    g[curCell].insert(nxtCell);
                    ind[nxtCell]++;
                }
                else{//==
                    Union(curCell,nxtCell);
                }
            }
        }

        //build group: root_nodes
        //aggregate ind
        unordered_map<int, unordered_set<int>> group;//to use vector<vector<int>> group(m*n)
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                int cell=ii2i({i,j});
                int root=find(cell);
                group[root].insert(cell);
                if(root!=cell){
                    ind[root]+=ind[cell];
                }
            }
        }

        //topSort on root
        //for root with ind==0, in q
        queue<int> q;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                int cell=ii2i({i,j});
                int root=find(cell);
                if(cell==root && ind[root]==0) q.push(root);
            }
        }
        //top sort process - bfs
        vector<vector<int>> res(m,vector<int>(n,-1));
        int rank=1;//layer by layer to set the rank
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                int root = q.front(); q.pop();
                //process
                for(int cell: group[root]){
                    auto [x,y]=i2ii(cell);
                    res[x][y]=rank;
                }
                //recalculate indegree of neighbors of cells
                for(int cell: group[root]){
                    for(int nxtCell: g[cell]){
                        ind[find(nxtCell)]--;
                        if(ind[find(nxtCell)]==0){q.push(find(nxtCell));}
                    }
                }
            }
            rank++;
        }
        return res;
    }
};

/*
topSort + unionFind

topSort:
    for each cell
        comparing to other cells on its row and col
            there are dependency relationship: cell_A < cell_B, that rate_A<rate_B
    so topSort to put smallest into q, for each layer of the bfs, rank_X = layer_step
unionFind:
    if cell_A == cell_B, then rank_A == rank_B
    so all elements that
        share same row / col, and val is same, then group to one big node
        maintain
            root - cells

how to handle big node? since it expand on multi row and col
**intui**:
    for a big node, we need to aggregate the ind of each node of it, e.i.: ind[node]= sum(ind[cell] of the node)
    how to find the node -> use root by find(cell)
e.g.
    3 X 3 4
    X X X X
X 3 3 3 X 4
1         4

ind of big node:
- treat all linked 3 as 1 node, aggregate the ind of them together

how to deduct ind of big node:
- when a cell is pop out from queue, for all depended 3, reduce ind by 1, aggregate the deduction for the node
- when the big node's ind==0, pop out all linked 3 together

when node is poped out, how affect other cells in graph:
- each cell in the node affect others independently, means in total the big node of 4 deducted ind by 5 times

pseudo code:
    step1:
        build dependency graph for each row and col, if x<y then x->y
            g: cur->nxts
        build ind
        union nodes on same row/col with same val

            vector<vector<vector<ii>>> g(m,vector<vector<ii>>(n,vector<ii>()));
            vector<vector<int>> ind(m,vector<int>(n,0));
    step2:        
        for each row
            sort
            for each (cur, nxt) element pair
                if cur==nxt: Union(cur,nxt)
                if cur<nxt: g[cur]->nxt

        for each col, similar with above
    step3:
        build group: root_nodes
            reason: need to fetch all cells of a root to set rank
    step4:
        topSort on root
        for root with ind==0, in q

        top sort process - bfs
        use bfs layer step as rank
        pop root in q, for all nodes of root, 
                            set rank,
                            update ind of nxts,
*/
```

Solution: Greedy

```cpp

```