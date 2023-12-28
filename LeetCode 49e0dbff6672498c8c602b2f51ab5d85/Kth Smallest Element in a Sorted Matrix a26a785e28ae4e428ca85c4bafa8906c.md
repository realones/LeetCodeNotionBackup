# Kth Smallest Element in a Sorted Matrix

#: 378
Difficult: Medium
Tags: BS_Val, PQ, kth
link: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
Priority: Medium
Status: CalcuComplexity, No Idea At First
Created time: August 15, 2022 9:30 PM
Last edited time: October 21, 2023 1:22 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初Hash&Heap, 算高TwoPointers&PQ
related: Kth Smallest Number in Multiplication Table (Kth%20Smallest%20Number%20in%20Multiplication%20Table%20cc17c7c0be7945be91a361ee02bc6731.md), K-th Smallest Prime Fraction (K-th%20Smallest%20Prime%20Fraction%207c951e5c8ed7479dac2997b627722d17.md), Find K-th Smallest Pair Distance (Find%20K-th%20Smallest%20Pair%20Distance%200447f8ad46f94f05991e0c16d880e076.md)

Given an `n x n` `matrix` where each of the rows and columns is sorted in ascending order, return *the* `kth` *smallest element in the matrix*.

Note that it is the `kth` smallest element **in the sorted order**, not the `kth` **distinct** element.

You must find a solution with a memory complexity better than `O(n2)`.

**Example 1:**

```
Input: matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
Output: 13
Explanation: The elements in the matrix are [1,5,9,10,11,12,13,13,15], and the 8th smallest number is 13

```

**Example 2:**

```
Input: matrix = [[-5]], k = 1
Output: -5

```

**Constraints:**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 300`
- `109 <= matrix[i][j] <= 109`
- All the rows and columns of `matrix` are **guaranteed** to be sorted in **non-decreasing order**.
- `1 <= k <= n2`

**Follow up:**

- Could you solve the problem with a constant memory (i.e., `O(1)` memory complexity)?
- Could you solve the problem in `O(n)` time complexity? The solution may be too advanced for an interview but you may find reading [this paper](http://www.cse.yorku.ca/~andy/pubs/X+Y.pdf) fun.

Solution 1:

是否需要遍历全部的元素呢?

**总结:**见到集合求Min/Max 就要想到堆

与上面的ugly-number-ii相似, 也可以BS - BS on scope but not on index****

S: O(x), x is min(k,n), since 最长的情况就是右上到左下对角线一共n个item在pq里

T: O(k * log x)

```cpp
//Priority queue solution:
class Tuple{
public:
    int x,y,val;
    Tuple(int x,int y,int val){
        this->x=x;
        this->y=y;
        this->val=val;
    }
};

class MyComp{
public:
    bool operator()(Tuple left,Tuple right){
        return left.val>right.val;
    }
};

class Solution {
public:
    /*
     * @param matrix: a matrix of integers
     * @param k: An integer
     * @return: the kth smallest number in the matrix
     */
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int m=matrix.size(); if(m==0) return INT_MIN;
        int n=matrix[0].size(); if(n==0) return INT_MIN;
        priority_queue<Tuple,vector<Tuple>,MyComp> q;
        vector<vector<bool>> visit(m,vector<bool>(n,false));
        
        vector<int> x({0,1});
        vector<int> y({1,0});
        
        q.emplace(0,0,matrix[0][0]); visit[0][0]=true;
        for(int i=0;i<k-1;i++){
            Tuple cur=q.top();q.pop();
            for(int j=0;j<2;j++){
                int newx=cur.x+x[j];
                int newy=cur.y+y[j];
                if(newx<m && newy<n && !visit[newx][newy]){//valid
                    visit[newx][newy]=true;
                    q.emplace(newx,newy,matrix[newx][newy]);
                }
            }
        }
        return q.top().val;
    }
};
```

solution 1.2:

```cpp
class Solution {
public:
    using vxy=tuple<int,int,int>;
    vector<int> dx{0,1};
    vector<int> dy{1,0};

    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int m=matrix.size(), n=matrix[0].size();
        vector<vector<bool>> visited(m,vector<bool>(n,0));
        priority_queue<vxy,vector<vxy>,greater<>> pq;
        visited[0][0]=true;
        pq.emplace(matrix[0][0],0,0);
        for(int i=0; i<k-1; i++){
            auto [v,x,y] = pq.top(); pq.pop();
            for(int k=0; k<2; k++){
                int nx=x+dx[k];
                int ny=y+dy[k];
                if(nx>m-1 || ny>n-1) continue;
                if(visited[nx][ny]) continue;
                visited[nx][ny]=true;
                pq.emplace(matrix[nx][ny],nx,ny);
            }
        }
        return get<0>(pq.top());
    }
};
```

Solution 2:

T: O(n*log(max-min))

```cpp
//BS solution:
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int m=matrix.size(); if(m==0) return INT_MIN;
        int n=matrix[0].size(); if(n==0) return INT_MIN;
        int lv=matrix[0][0],rv=matrix[m-1][n-1];
        while(lv<rv){
            int mv=lv+(rv-lv)/2;
            int cnt=0,j=n-1;
            for(int i=0;i<m;i++){
                for(;j>=0 && matrix[i][j]>mv;j--);
                //find the number of elements in current row that <= mv
                cnt+=(j+1);    
            }
            
            if(cnt<k) lv=mv+1;
            else rv=mv;
        }
        return lv;
    }
};
```