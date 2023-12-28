# Maximum Students Taking Exam

#: 1349
Difficult: Hard
Tags: dp-TimeSeqFromLastOne-Power, dp-bitMask
link: https://leetcode.com/problems/maximum-students-taking-exam/
Priority: High
Status: Dig More
Created time: January 14, 2023 5:03 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路

Given a `m * n` matrix `seats`  that represent seats distributions in a classroom. If a seat is broken, it is denoted by `'#'` character otherwise it is denoted by a `'.'` character.

Students can see the answers of those sitting next to the left, right, upper left and upper right, but he cannot see the answers of the student sitting directly in front or behind him. Return the **maximum** number of students that can take the exam together without any cheating being possible..

Students must be placed in seats in good condition.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/29/image.png](https://assets.leetcode.com/uploads/2020/01/29/image.png)

```
Input: seats = [["#",".","#","#",".","#"],
                [".","#","#","#","#","."],
                ["#",".","#","#",".","#"]]
Output: 4
Explanation: Teacher can place 4 students in available seats so they don't cheat on the exam.

```

**Example 2:**

```
Input: seats = [[".","#"],
                ["#","#"],
                ["#","."],
                ["#","#"],
                [".","#"]]
Output: 3
Explanation: Place all students in available seats.

```

**Example 3:**

```
Input: seats = [["#",".",".",".","#"],
                [".","#",".","#","."],
                [".",".","#",".","."],
                [".","#",".","#","."],
                ["#",".",".",".","#"]]
Output: 10
Explanation: Place students in available seats in column 1, 3 and 5.

```

**Constraints:**

- `seats` contains only characters `'.' and'#'.`
- `m == seats.length`
- `n == seats[i].length`
- `1 <= m <= 8`
- `1 <= n <= 8`

Solution new:

```cpp
class Solution {
public:
    int maxStudents(vector<vector<char>>& matrix) {
        int rowCnt=matrix.size();
        int seatCnt=matrix[0].size();
        //row -> state : for validation - 1 means broken
        vector<int> rowStates(rowCnt,0);
        for(int i=0; i<rowCnt; i++){
            for(int j=0;j<seatCnt;j++){
                if(matrix[i][j]=='#'){
                    rowStates[i]+=(1<<j);
                }
            }
        }
        //dp
        int N=1<<seatCnt;
        vector<vector<int>> dp(rowCnt+1,vector<int>(N,INT_MIN));
        dp[0][0]=0;
        for(int r=1;r<rowCnt+1;r++){
            for(int s=0;s<N;s++){
                if(hasNeighbors(s)) continue;//invalid row state with no neighbors
                if(s&rowStates[r-1]) continue;//invalid row state with broken
                for(int ps=0;ps<N;ps++){//pre state
                    if(hasNeighbors(s|ps)) continue;
                    dp[r][s]=max(dp[r][s], dp[r-1][ps]+s2p(s));
                }
            }
        }
        return *max_element(dp.back().begin(), dp.back().end());
    }
    
    bool hasNeighbors(int s){//s: state
        bool nei=false;//flag if just visited a people
        while(s){
            if(s&1){
                if(nei) return true;
                nei=true;
            }
            else{nei=false;}
            s>>=1;
        }
        return false;
    }
    
    int s2p(int s){
        int cnt=0;
        while(s){
            cnt+=s&1;
            s>>=1;
        }
        return cnt;
    }
};

/*
dp row state = first i rows, with state, max people

for each row
    for each possible row state - 8bit
        dp[row][state] max = dp[row-1][valid_state] + curRowState.people
return dp.b.max

valid_state:
backpack i <- multi i-1 with loop
*/
```

Solution old:

```jsx
class Solution {
public:
    int maxStudents(vector<vector<char>>& seats) {
        int m=seats.size(); if(m==0) {return 0;}
        int n=seats[0].size(); if(n==0) {return 0;}
        
        //dp ith_row, state = max people so far
        int N=(1<<n); //possible state + 1
        vector<vector<int>> dp(m+1,vector<int>(N,-1));//-1, invalid
        
        dp[0][0]=0;//-1 dummy row, only 0 people allowed
            
        /*
        for i 1~nth row: 8
          for state from 0000 to 11111: 2^8
                if not valid -> continue
                dp i s max= for pre vall possible state(another for loop from 000 to 111): 2^8
                                dp i-1 preState + people(s): 8
        
        T: 2^ 3+8+8+3 -> fine
        */
        seats.insert(seats.begin(), vector<char>(n,'.'));//so not use i-1 to represent the row, use i directly
        for(int i=1; i<m+1; i++){//get res from pre row
            for(int s=0;s<N;s++){
                if(!validSeats(seats, i, s)) continue;//check cur row only
                for(int ps=0;ps<N;ps++){//pre row
                    if(dp[i-1][ps]==-1) continue;
                    if(!validPreSeats(seats,i,s,ps)) continue;//check pre and cur row
                    dp[i][s]=max(dp[i][s],dp[i-1][ps]+getPeople(s));
                }
            }
        }
        
        return *max_element(dp.back().begin(),dp.back().end());
    }
    
    bool validSeats(vector<vector<char>>& seats, int row, int state){
        if(!notSitOnBroke(seats[row],state)) return false;
        return notNearBy(state);
    }
    
    bool validPreSeats(vector<vector<char>>& seats, int row, int state, int preState){
        if(!notSitOnBroke(seats[row-1],preState)) return false;
        state=state|preState;
        return notNearBy(state);
    }
    
    bool notNearBy(int state){
        bool res=true;
        bool seeOne=false;
        while(state){
            if(state&1){
                if(seeOne) return false;
                seeOne=true;
            }
            else{seeOne=false;}
            state>>=1;
        }
        return res;
    }
    
    bool notSitOnBroke(vector<char>& row, int state){
        bool res=true;
        int i=row.size()-1;
        while(state){
            if(state&1){
                if(row[i]=='#') return false;
            }
            i--;
            state>>=1;
        }
        return res;
    }
    
    int getPeople(int state){
        int res=0;
        while(state){
            res+=(state&1);
            state>>=1;
        }
        return res;
    }
};
```