# Meeting Rooms III

#: 2404
Difficult: Hard
Tags: Array, PQ, Sort, queue, simulation
link: https://leetcode.com/problems/meeting-rooms-iii/
Priority: High
Status: CalcuComplexity, HardToImplement, checkBetterSolution
Created time: December 18, 2023 2:01 AM
Last edited time: December 18, 2023 2:03 AM
source: googleFreq
related: Single-Threaded CPU (Single-Threaded%20CPU%205a6365ece1ea464ba78f0d908e70b68a.md)

You are given an integer `n`. There are `n` rooms numbered from `0` to `n - 1`.

You are given a 2D integer array `meetings` where `meetings[i] = [starti, endi]` means that a meeting will be held during the **half-closed** time interval `[starti, endi)`. All the values of `starti` are **unique**.

Meetings are allocated to rooms in the following manner:

1. Each meeting will take place in the unused room with the **lowest** number.
2. If there are no available rooms, the meeting will be delayed until a room becomes free. The delayed meeting should have the **same** duration as the original meeting.
3. When a room becomes unused, meetings that have an earlier original **start** time should be given the room.

Return *the **number** of the room that held the most meetings.* If there are multiple rooms, return *the room with the **lowest** number.*

A **half-closed interval** `[a, b)` is the interval between `a` and `b` **including** `a` and **not including** `b`.

**Example 1:**

```
Input: n = 2, meetings = [[0,10],[1,5],[2,7],[3,4]]
Output: 0
Explanation:
- At time 0, both rooms are not being used. The first meeting starts in room 0.
- At time 1, only room 1 is not being used. The second meeting starts in room 1.
- At time 2, both rooms are being used. The third meeting is delayed.
- At time 3, both rooms are being used. The fourth meeting is delayed.
- At time 5, the meeting in room 1 finishes. The third meeting starts in room 1 for the time period [5,10).
- At time 10, the meetings in both rooms finish. The fourth meeting starts in room 0 for the time period [10,11).
Both rooms 0 and 1 held 2 meetings, so we return 0.

```

**Example 2:**

```
Input: n = 3, meetings = [[1,20],[2,10],[3,5],[4,9],[6,8]]
Output: 1
Explanation:
- At time 1, all three rooms are not being used. The first meeting starts in room 0.
- At time 2, rooms 1 and 2 are not being used. The second meeting starts in room 1.
- At time 3, only room 2 is not being used. The third meeting starts in room 2.
- At time 4, all three rooms are being used. The fourth meeting is delayed.
- At time 5, the meeting in room 2 finishes. The fourth meeting starts in room 2 for the time period [5,10).
- At time 6, all three rooms are being used. The fifth meeting is delayed.
- At time 10, the meetings in rooms 1 and 2 finish. The fifth meeting starts in room 1 for the time period [10,12).
Room 0 held 1 meeting while rooms 1 and 2 each held 2 meetings, so we return 1.

```

**Constraints:**

- `1 <= n <= 100`
- `1 <= meetings.length <= 105`
- `meetings[i].length == 2`
- `0 <= starti < endi <= 5 * 105`
- All the values of `starti` are **unique**.

comment: 思路很快想到，实现起来很慢

todo: 对于不同的取值范围，可以采用不同的解法，例如可以直接loop所有时间从0123到1e5, 而不是用pq来maintain时间

```cpp
//the idea is very similar to the system design: Ticketmaster with daemon thread
using ll=long long;
int END=0;//to sort end before start
int START=1;
using iii=tuple<ll,int,int>;
class Solution {
public:
    int mostBooked(int n, vector<vector<int>>& meetings) {
        vector<int> roomCnt(n,0);
        
        //time - start - duration
        //time - end - roomid
        priority_queue<iii,vector<iii>,greater<>> timePQ;
        for(auto& m: meetings){//init
            int time=m[0], duration=m[1]-m[0];
            timePQ.emplace(time,START,duration);
        }
        priority_queue<int,vector<int>,greater<>> roomPQ;//roomId
        for(int i=0; i<n; i++){roomPQ.push(i);}
        queue<int> delayQ;//duration

        //process
        while(!timePQ.empty()){
            auto [time, status, var]= timePQ.top(); timePQ.pop();
            // if there is some meeting end
            if(status==END){
                //relase the room
                int roomId=var;
                roomPQ.push(roomId);
                // if there are delayed meeting and there are available rooms - p1
                if(!roomPQ.empty() && !delayQ.empty()){
                    int duration=delayQ.front(); delayQ.pop();
                    int roomId=roomPQ.top(); roomPQ.pop();
                    //assign
                    timePQ.emplace(time+duration, END, roomId);
                    roomCnt[roomId]++;
                }
            }
            else{//start
                int duration=var;
                // if a meeting start and there are available rooms - p2
                if(!roomPQ.empty()){//can put
                    int roomId=roomPQ.top(); roomPQ.pop();
                    //assign
                    timePQ.emplace(time+duration, END, roomId);
                    roomCnt[roomId]++;
                }
                else{// else delay -> put into queue (FIFO to keep seq)
                    delayQ.push(duration);
                }
            }
        }

        return max_element(roomCnt.begin(), roomCnt.end()) - roomCnt.begin();
    }
};

/*
rooms: 0~n-1
meetings will allocate to the lowest available room number
if all rooms busy, delay meetings
================================
meetings:
    len: 1e5
    start, end: 1e5
    start is unique
n: 100
================================
simulation
sort meetings by start

for each time: 0,1,2,3...infinite
===>
for each time make sense:
    starting time of each meeting - we check if can start the meeting or put into delay queue
        so the start time should be sorted
    end time of each meeting - so we can clean up the related room, in order for next meeting 
        so the end time should be sorted
    overall, we should have a pq to maintain the times
        pq: [time-start/end-duration]

    if there is some meeting end, release the room

    if there are delayed meeting and there are available rooms - p1
        put in room
            pq-room to know the best smallest id to assign
    if a meeting start and there are available rooms - p2
        put in room
    else delay -> put into queue (FIFO to keep seq)
        q: [duration]

*/
```