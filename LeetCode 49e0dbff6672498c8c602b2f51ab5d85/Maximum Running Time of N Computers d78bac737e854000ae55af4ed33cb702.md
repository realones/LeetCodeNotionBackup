# Maximum Running Time of N Computers

#: 2141
Difficult: Hard
Tags: Array, BS_Val, Greedy, Sort
link: https://leetcode.com/problems/maximum-running-time-of-n-computers/
Priority: High
Status: CalcuComplexity, More Solution, No Idea At First, NotUnderstand
Created time: August 7, 2023 7:19 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You have `n` computers. You are given the integer `n` and a **0-indexed** integer array `batteries` where the `ith` battery can **run** a computer for `batteries[i]` minutes. You are interested in running **all** `n` computers **simultaneously** using the given batteries.

Initially, you can insert **at most one battery** into each computer. After that and at any integer time moment, you can remove a battery from a computer and insert another battery **any number of times**. The inserted battery can be a totally new battery or a battery from another computer. You may assume that the removing and inserting processes take no time.

Note that the batteries cannot be recharged.

Return *the **maximum** number of minutes you can run all the* `n` *computers simultaneously.*

**Example 1:**

![https://assets.leetcode.com/uploads/2022/01/06/example1-fit.png](https://assets.leetcode.com/uploads/2022/01/06/example1-fit.png)

```
Input: n = 2, batteries = [3,3,3]
Output: 4
Explanation:
Initially, insert battery 0 into the first computer and battery 1 into the second computer.
After two minutes, remove battery 1 from the second computer and insert battery 2 instead. Note that battery 1 can still run for one minute.
At the end of the third minute, battery 0 is drained, and you need to remove it from the first computer and insert battery 1 instead.
By the end of the fourth minute, battery 1 is also drained, and the first computer is no longer running.
We can run the two computers simultaneously for at most 4 minutes, so we return 4.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/01/06/example2.png](https://assets.leetcode.com/uploads/2022/01/06/example2.png)

```
Input: n = 2, batteries = [1,1,1,1]
Output: 2
Explanation:
Initially, insert battery 0 into the first computer and battery 2 into the second computer.
After one minute, battery 0 and battery 2 are drained so you need to remove them and insert battery 1 into the first computer and battery 3 into the second computer.
After another minute, battery 1 and battery 3 are also drained so the first and second computers are no longer running.
We can run the two computers simultaneously for at most 2 minutes, so we return 2.

```

**Constraints:**

- `1 <= n <= batteries.length <= 105`
- `1 <= batteries[i] <= 109`

**todo**: solution “Editorial - Solution 1” miss some proof in the beginning of comment, there are bs solution and lee215 solution

**Solution** WisdomPeak - BS:

from wisdompeak: 本题本质就是允许你把每个电池拆分成若干个单位电量1的小电池任意用在各个电脑上。你手头有这么多单位1的小电池，不用操心具体如何分配 - why?

```cpp
using ll=long long;

class Solution {
public:
    long long maxRunTime(int n, vector<int>& batteries) {
        ll l=0;
        ll r=accumulate(batteries.begin(), batteries.end(), 0L)/n;
        while(l<r){
            ll m=l+(r-l+1)/2;
            if(!valid(n,batteries,m)) {r=m-1;}//if not qualify
            else {l=m;}
        }
        return r;
    }

    //电池可以任意分配给各个电脑和各个时段
    //策略: 把所有的电池容量加起来，查看能否支撑T*N即可。
    //任意一个电池都不能贡献超过时间T（因为我们只会让电脑运行时间T）。所以我们在算电池总容量的时候，取T为上限。
    bool valid(int n, vector<int>& batteries, ll time){
        ll sum=0;
        for(auto b: batteries){
            sum+=min(time,(ll)b);
        }
        return sum>=n*time;
    }

};

/*
my question:
 is it possible that:
    YYYYNNNYYYYNNNYYYYYYYNNNNNNNNNNN

e.g. 2 computers, [1,1,10] batteries
	valid time=2
		sum = 1+1+2=4 >= 2*2 is true
	valid time=3
		sum = 1+1+3=5 >= 2*3 is false

*/
```

**Solution** Editorial - Solution 1:

```cpp
/*
from Editorial - Solution 1
"all the extra power is interchangeable", hmmm
need to prove it to get this solution, which is not proved yet in Editorial, there is something in discuss not checked yet.
*/
using ll=long long;

class Solution {
public:
    long long maxRunTime(int n, vector<int>& batteries) {
        //small small ... large large large
        //                -----  n --------
        sort(batteries.begin(), batteries.end());
        //get extra
        ll extra=0;
        for(int i=0; i<batteries.size()-n; i++){
            extra+=batteries[i];
        }
        //live stand for n largest batteries
        //small to large
        vector<int> live(batteries.end()-n, batteries.end());

        //inc the total running time using 'extra' by increasing
        //the running time of computer with the smallest battery
        for(int i=0;i<n-1;i++){//for each gap diff between each near by computers
            ll diff=live[i+1]-live[i];
            //here (i+1) means the computers which need to use extra power
            if(extra < (i+1)*diff){//total extra running time less the diff
                return live[i]+ extra/(i+1);
            }
            //reduce extra
            extra -= (i+1)*diff;
        }

        //if there a extra power left, we can inc the running time of all computers
        return live[n-1] + extra/n;
    }
};

/*
n computers
batteries: [power for x minutes for one computer]

run all n computers simultaneously

init: insert <=1 battery into each computer
then: at integer time, remove battery and insert another battery

return:
    max of minutes than all computers run simultaneously

================================
n: 1~batteries.len
batteries.len: n~1e5
batteries.val: 1~1e9

here batteries.len>n means always have valid solution, worst is 0
================================
so simultaneously running can from start[0 mins], if start not possible, then overall not possible, end untill no longer can support simultaneously.
================================
maintain pq every min for top n large batteries?
purpose: make all batteries as even as possible to avoid use up some ones then can't use the rest because batteries not enough
proof: best strategy, 
->

improve:
1. use till smallest in the top n is no longer the top n
2. when put back to container, can move the last one[can have dup] from pq and switch with top of the rest

algorithm:
1. sort batteries
2. find largest n batteries, assign to computers
        arr 'live' to maintain them in sorted order
3. the rest batteries -> sum up to be 'extra'
4. iterate over 'live' from 0 to n-2, for each i:
    - if 'extra' power can increase the running time of the first i computers from live[i] to live[i + 1], then we subtract the required power from extra and move on to the next index.
    - else, stop and return live[i]+extra/(i+1)
5. If there is still power left after the iteration, it means we can further increase the total running time of n computers from live[n - 1] by extra / n. Therefore, return live[n - 1] + extra / n.

================================
e.g. 1
n=2, batteries:[3,3,3] => 4

init - assign
0 1
3 3 3
run
0 1
2 2 3
switch
0 1
2 3 2
run
0 1
1 2 2
run
0 1
0 1 2
switch
0 1
1 2 0
run
0 1
0 1 0
in total 4 runs -> 4 mins

================================
e.g. 2
init - assign
0 1
2 2 2 2
run
run
0 1
0 0 2 2
switch
0 1
2 2 0 0
run
run
0 1
0 0 0 0

================================

*/
```