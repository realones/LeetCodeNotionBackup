# Snakes and Ladders

#: 909
Difficult: Medium
Tags: Array, BFS, Graph, Matrix
link: https://leetcode.com/problems/snakes-and-ladders/
Priority: High
Status: No Idea At First
Created time: June 6, 2023 12:06 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Solution BFS

```cpp
class Solution {
public:
    int snakesAndLadders(vector<vector<int>>& matrix) {
        int n=matrix.size();
        queue<int> q;
        vector<int> visited(n*n+1,0);
        visited[1]=1;
        q.push(1);
        int step=0;
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                int cur=q.front(); q.pop();
                if(cur==n*n) return step;
                for(int k=1;k<=6;k++){
                    int nxt=cur+k;
                    if(nxt>n*n) continue;
                    auto [x,y] = trans(n,nxt);
                    if(matrix[x][y]!=-1) nxt=matrix[x][y];
                    if(visited[nxt]) continue;
                    visited[nxt]=1;
                    q.push(nxt);
                }
            }
            step++;
        }
        return -1;
    }
    
    //trans cell to r,c
    pair<int,int> trans(int n, int cur){
        cur--;
        int x=cur/n, y=cur%n;
        return {n-1-x, (x%2==0)?y:n-1-y};
    }
};

/*
next: [curr + 1, min(curr + 6, n2)]

snake -> snake end

!=-1: snake, go to the element number

dp from start to end
*/
```

Solution DP - incorrect for usecase: 
    jump to near end
    jump back to near front
    move a few steps
    jump to end

```cpp
class Solution {
public:
    int snakesAndLadders(vector<vector<int>>& matrix) {
        int n=matrix.size();
        vector<int> dp(n*n+1,INT_MAX);//0..n^2
        dp[1]=0;
        //i->i+1
        for(int i=1; i<dp.size(); i++){
            if(dp[i]==INT_MAX) continue;
            for(int k=1; k<=6; k++){
                int nxt=i+k;
                if(nxt>dp.size()-1) continue;
                auto [x,y] = trans(n,nxt);
                if(matrix[x][y]!=-1) nxt=matrix[x][y];
                dp[nxt]=min(dp[nxt],dp[i]+1);
            }
        }
        return dp.back()==INT_MAX? -1 : dp.back();
    }
    
    //trans cell to r,c
    pair<int,int> trans(int n, int cur){
        cur--;
        int x=cur/n, y=cur%n;
        return {n-1-x, (x%2==0)?y:n-1-y};
    }
};

/*
next: [curr + 1, min(curr + 6, n2)]

snake -> snake end

!=-1: snake, go to the element number

dp from start to end
*/
```