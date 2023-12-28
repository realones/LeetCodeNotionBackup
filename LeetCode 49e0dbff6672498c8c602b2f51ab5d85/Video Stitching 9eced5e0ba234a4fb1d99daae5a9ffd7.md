# Video Stitching

#: 1024
Difficult: Medium
Tags: Array, DP, Greedy, Interval
link: https://leetcode.com/problems/video-stitching/
Priority: Medium
Status: More Solution
Created time: June 28, 2023 12:44 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Minimum Number of Taps to Open to Water a Garden (Minimum%20Number%20of%20Taps%20to%20Open%20to%20Water%20a%20Garden%20e739aa4e5e1e47348c15083afa0e06ca.md)

You are given a series of video clips from a sporting event that lasted `time` seconds. These video clips can be overlapping with each other and have varying lengths.

Each video clip is described by an array `clips` where `clips[i] = [starti, endi]` indicates that the ith clip started at `starti` and ended at `endi`.

We can cut these clips into segments freely.

- For example, a clip `[0, 7]` can be cut into segments `[0, 1] + [1, 3] + [3, 7]`.

Return *the minimum number of clips needed so that we can cut the clips into segments that cover the entire sporting event* `[0, time]`. If the task is impossible, return `-1`.

**Example 1:**

```
Input: clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], time = 10
Output: 3
Explanation: We take the clips [0,2], [8,10], [1,9]; a total of 3 clips.
Then, we can reconstruct the sporting event as follows:
We cut [1,9] into segments [1,2] + [2,8] + [8,9].
Now we have segments [0,2] + [2,8] + [8,10] which cover the sporting event [0, 10].

```

**Example 2:**

```
Input: clips = [[0,1],[1,2]], time = 5
Output: -1
Explanation: We cannot cover [0,5] with only [0,1] and [1,2].

```

**Example 3:**

```
Input: clips = [[0,1],[6,8],[0,2],[5,6],[0,4],[0,3],[6,7],[1,3],[4,7],[1,4],[2,5],[2,6],[3,4],[4,5],[5,7],[6,9]], time = 9
Output: 3
Explanation: We can take clips [0,4], [4,7], and [6,9].

```

**Constraints:**

- `1 <= clips.length <= 100`
- `0 <= starti <= endi <= 100`
- `1 <= time <= 100`

```cpp
class Solution {
public:
    static bool cmp(const vector<int> &x, const vector<int> &y) {
        if(x[0]!=y[0]) return x[0]<y[0];
        else return x[1]>y[1];
    }

    int videoStitching(vector<vector<int>>& clips, int time) {
        return minItvsToCoverRange(clips, 0, time);
    }
    int minItvsToCoverRange(vector<vector<int>>& itvs, int start, int end) {
        sort(itvs.begin(), itvs.end(),cmp);

        int cnt=0,i=0;
        int coveredR=0;
        int newCoveredR=0;
        while(coveredR<end){//not all covered
            while(i<itvs.size() && itvs[i][0]<=coveredR){
                newCoveredR=max(newCoveredR,itvs[i][1]);
                i++;
            }
            if(newCoveredR<=coveredR) return -1;
            coveredR=newCoveredR;
            cnt++;
        }
        return cnt;
    }
};

/*
clip = [start,end]

goal: to cover [0,time]
return: min num of clips
================================
len: 1~100
start,end: 0~100
time: 1~100
================================

----------- 1
------ 1
  ------- 1
    --------------- 2
        ---------------------- 2
                 -------------------3   
                                        ----- fail

sort by left first"<", then by right">"
init curRange=[0,0]
while curRange not reach targetEnd
    while nxt itv left in curRange
        for the one that reach the farthest, set as nxtRange
    res++
    curRange=nxtRange
    if(curRange reach the end) return res
return fallback
*/
```