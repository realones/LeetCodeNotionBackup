# Exclusive Time of Functions

#: 636
Difficult: Medium
Tags: Array, simulation, stack
link: https://leetcode.com/problems/exclusive-time-of-functions/
Priority: High
Status: HardToImplement
Created time: June 27, 2023 10:57 PM
Last edited time: December 10, 2023 4:47 PM
source: HuaHua

On a **single-threaded** CPU, we execute a program containing `n` functions. Each function has a unique ID between `0` and `n-1`.

Function calls are **stored in a [call stack](https://en.wikipedia.org/wiki/Call_stack)**: when a function call starts, its ID is pushed onto the stack, and when a function call ends, its ID is popped off the stack. The function whose ID is at the top of the stack is **the current function being executed**. Each time a function starts or ends, we write a log with the ID, whether it started or ended, and the timestamp.

You are given a list `logs`, where `logs[i]` represents the `ith` log message formatted as a string `"{function_id}:{"start" | "end"}:{timestamp}"`. For example, `"0:start:3"` means a function call with function ID `0` **started at the beginning** of timestamp `3`, and `"1:end:2"` means a function call with function ID `1` **ended at the end** of timestamp `2`. Note that a function can be called **multiple times, possibly recursively**.

A function's **exclusive time** is the sum of execution times for all function calls in the program. For example, if a function is called twice, one call executing for `2` time units and another call executing for `1` time unit, the **exclusive time** is `2 + 1 = 3`.

Return *the **exclusive time** of each function in an array, where the value at the* `ith` *index represents the exclusive time for the function with ID* `i`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/04/05/diag1b.png](https://assets.leetcode.com/uploads/2019/04/05/diag1b.png)

```
Input: n = 2, logs = ["0:start:0","1:start:2","1:end:5","0:end:6"]
Output: [3,4]
Explanation:
Function 0 starts at the beginning of time 0, then it executes 2 for units of time and reaches the end of time 1.
Function 1 starts at the beginning of time 2, executes for 4 units of time, and ends at the end of time 5.
Function 0 resumes execution at the beginning of time 6 and executes for 1 unit of time.
So function 0 spends 2 + 1 = 3 units of total time executing, and function 1 spends 4 units of total time executing.

```

**Example 2:**

```
Input: n = 1, logs = ["0:start:0","0:start:2","0:end:5","0:start:6","0:end:6","0:end:7"]
Output: [8]
Explanation:
Function 0 starts at the beginning of time 0, executes for 2 units of time, and recursively calls itself.
Function 0 (recursive call) starts at the beginning of time 2 and executes for 4 units of time.
Function 0 (initial call) resumes execution then immediately calls itself again.
Function 0 (2nd recursive call) starts at the beginning of time 6 and executes for 1 unit of time.
Function 0 (initial call) resumes execution at the beginning of time 7 and executes for 1 unit of time.
So function 0 spends 2 + 4 + 1 + 1 = 8 units of total time executing.

```

**Example 3:**

```
Input: n = 2, logs = ["0:start:0","0:start:2","0:end:5","1:start:6","1:end:6","0:end:7"]
Output: [7,1]
Explanation:
Function 0 starts at the beginning of time 0, executes for 2 units of time, and recursively calls itself.
Function 0 (recursive call) starts at the beginning of time 2 and executes for 4 units of time.
Function 0 (initial call) resumes execution then immediately calls function 1.
Function 1 starts at the beginning of time 6, executes 1 unit of time, and ends at the end of time 6.
Function 0 resumes execution at the beginning of time 6 and executes for 2 units of time.
So function 0 spends 2 + 4 + 1 = 7 units of total time executing, and function 1 spends 1 unit of total time executing.

```

**Constraints:**

- `1 <= n <= 100`
- `1 <= logs.length <= 500`
- `0 <= function_id < n`
- `0 <= timestamp <= 109`
- No two start events will happen at the same timestamp.
- No two end events will happen at the same timestamp.
- Each function has an `"end"` log for each `"start"` log.

comment: has the idea but takes so long to get it right…

注意每个时间是个块，不是一条分割线，end的时候，下一次从end+1开始计算时间

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