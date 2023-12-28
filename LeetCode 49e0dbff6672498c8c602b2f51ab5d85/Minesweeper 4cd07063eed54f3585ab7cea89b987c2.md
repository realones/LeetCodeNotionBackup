# Minesweeper

#: 529
Difficult: Medium
Tags: BFS, DFS, Matrix
link: https://leetcode.com/problems/minesweeper/description/
Priority: Low
Created time: December 10, 2023 5:14 PM
Last edited time: December 10, 2023 5:15 PM
source: facebookFreq

Let's play the minesweeper game ([Wikipedia](https://en.wikipedia.org/wiki/Minesweeper_(video_game)), [online game](http://minesweeperonline.com/))!

You are given an `m x n` char matrix `board` representing the game board where:

- `'M'` represents an unrevealed mine,
- `'E'` represents an unrevealed empty square,
- `'B'` represents a revealed blank square that has no adjacent mines (i.e., above, below, left, right, and all 4 diagonals),
- digit (`'1'` to `'8'`) represents how many mines are adjacent to this revealed square, and
- `'X'` represents a revealed mine.

You are also given an integer array `click` where `click = [clickr, clickc]` represents the next click position among all the unrevealed squares (`'M'` or `'E'`).

Return *the board after revealing this position according to the following rules*:

1. If a mine `'M'` is revealed, then the game is over. You should change it to `'X'`.
2. If an empty square `'E'` with no adjacent mines is revealed, then change it to a revealed blank `'B'` and all of its adjacent unrevealed squares should be revealed recursively.
3. If an empty square `'E'` with at least one adjacent mine is revealed, then change it to a digit (`'1'` to `'8'`) representing the number of adjacent mines.
4. Return the board when no more squares will be revealed.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/08/09/untitled.jpeg](https://assets.leetcode.com/uploads/2023/08/09/untitled.jpeg)

```
Input: board = [["E","E","E","E","E"],["E","E","M","E","E"],["E","E","E","E","E"],["E","E","E","E","E"]], click = [3,0]
Output: [["B","1","E","1","B"],["B","1","M","1","B"],["B","1","1","1","B"],["B","B","B","B","B"]]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/08/09/untitled-2.jpeg](https://assets.leetcode.com/uploads/2023/08/09/untitled-2.jpeg)

```
Input: board = [["B","1","E","1","B"],["B","1","M","1","B"],["B","1","1","1","B"],["B","B","B","B","B"]], click = [1,2]
Output: [["B","1","E","1","B"],["B","1","X","1","B"],["B","1","1","1","B"],["B","B","B","B","B"]]

```

**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 50`
- `board[i][j]` is either `'M'`, `'E'`, `'B'`, or a digit from `'1'` to `'8'`.
- `click.length == 2`
- `0 <= clickr < m`
- `0 <= clickc < n`
- `board[clickr][clickc]` is either `'M'` or `'E'`.

```cpp
int START=0, END=1;
using iii=tuple<int,int,int>;//id_status_time

class Solution {
public:
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        //deserialize logs and put into v
        vector<iii> v;
        for(auto& l: logs){
            int c1=l.find_first_of(':',0);
            int c2=l.find_first_of(':',c1+1);
            int id=stoi(l.substr(0,c1));
            int status=l.substr(c1+1, c2-c1-1)=="start"?0:1;
            int time=stoi(l.substr(c2+1));
            v.emplace_back(id,status,time);
        }
        //sort by timestamp
        sort(v.begin(), v.end(), [&](iii& a, iii& b)->bool{
            if(get<2>(a) == get<2>(b)) return get<1>(a) < get<1>(b);//start first
            return get<2>(a) < get<2>(b);
        });

        stack<iii> stk;
        vector<int> res(n,0);
        int prev=-1;
        for(auto& cur: v){
            auto [curId, curStatus, curTime] = cur;
            if(curStatus==START){
                if(!stk.empty()){
                    int id=get<0>(stk.top()); //to record in res
                    int time=curTime-prev;//prev~startTime-1
                    res[id]+=time;
                }
                prev=curTime;//prev=startTime
                stk.push(cur);
            } 
            else{
                int id=get<0>(stk.top()); //to record in res
                int time=curTime-prev+1;//prev~endTime
                res[id]+=time;
                prev=curTime+1;//prev=endTime+1
                stk.pop();
            }
        }
        return res;
    }
};

/*
n functions: 0~n-1
function call:
    start: push id in stack, write log with [id,start,timeStamp]
    end:   pop id from stack, write log with [id,end,timeStamp]
stk.top(): current executing func

func can be called multi times, possibly recursively

func's exclusive time = sum of execution time for all same id func calls

given:
    logs[i] = funcId_startOrEnd_timeStamp
return:
    exclusive time for each func
================================
n: 1~100
logs: 1~500
================================
res[i]: total execution time for func id == i
when func end - match with most recent start -> add to res[id]
    match with most recent
        stk: [(id, start/end, timeStamp)]
return res
*/
```