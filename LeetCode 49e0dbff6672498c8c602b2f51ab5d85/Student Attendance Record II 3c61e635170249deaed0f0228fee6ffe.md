# Student Attendance Record II

#: 552
Difficult: Hard
Tags: DP, stateMachine
link: https://leetcode.com/problems/student-attendance-record-ii/
Priority: High
Status: More Solution, No Idea At First, toSummarize
Created time: October 6, 2022 7:58 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

An attendance record for a student can be represented as a string where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

- `'A'`: Absent.
- `'L'`: Late.
- `'P'`: Present.

Any student is eligible for an attendance award if they meet **both** of the following criteria:

- The student was absent (`'A'`) for **strictly** fewer than 2 days **total**.
- The student was **never** late (`'L'`) for 3 or more **consecutive** days.

Given an integer `n`, return *the **number** of possible attendance records of length* `n` *that make a student eligible for an attendance award. The answer may be very large, so return it **modulo*** `109 + 7`.

**Example 1:**

```
Input: n = 2
Output: 8
Explanation: There are 8 records with length 2 that are eligible for an award:
"PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL"
Only "AA" is not eligible because there are 2 absences (there need to be fewer than 2).

```

**Example 2:**

```
Input: n = 1
Output: 3

```

**Example 3:**

```
Input: n = 10101
Output: 183236316

```

**Constraints:**

- `1 <= n <= 105`

```cpp
/*
六种状态：dp00,dp01,dp02,dp10,dp11,dp12.
状态dp_ij的定义是：其中i表示这个数列里absence出现的次数，显然对于一个valid的数列，i的范围只能是0和1；
j表示这个数列里末尾连续出现late的次数，显然对于一个valid的数列，i的范围只能是0，1，2。因此有六种状态的定义。

Action: A L P

画状态转移关系图

---->

i: ith day 1...n-1
a:absent l:late
al00[i]  =  al00[i-1]+al01[i-1]+al02[i-1] // 分别加'P'转变而来
al01[i]  =  al00[i-1]  // 加'L'转变而来
al02[i]  =  do01[i-1]  // 加'L'转变而来
al10[i]  =  al10[i-1]+al11[i-1]+al12[i-1]+al00[i-1]+al01[i-1]+al02[i-1] // 前三种加'P'转变而来，后三种加'A'转变而来
al11[i]  =  al10[i-1]  // 加'L'转变而来
al12[i]  =  al11[i-1]  // 加'L'转变而来

init
al00[0]=1

res: max of alXY.back
*/
class Solution {
public:
    int checkRecord(int n) {
        vector<long> al00(n+1,0);
        vector<long> al01(n+1,0);
        vector<long> al02(n+1,0);
        vector<long> al10(n+1,0);
        vector<long> al11(n+1,0);
        vector<long> al12(n+1,0);
        
        al00[0]=1;
        long M = 1e9+7;
        
        for(int i=1; i<n+1; i++){
            al00[i]  =  (al00[i-1]+al01[i-1]+al02[i-1])%M; // 分别加'P'转变而来
            al01[i]  =  al00[i-1];  // 加'L'转变而来
            al02[i]  =  al01[i-1];  // 加'L'转变而来
            al10[i]  =  (al10[i-1]+al11[i-1]+al12[i-1]+al00[i-1]+al01[i-1]+al02[i-1])%M; // 前三种加'P'转变而来，后三种加'A'转变而来
            al11[i]  =  al10[i-1];  // 加'L'转变而来
            al12[i]  =  al11[i-1];  // 加'L'转变而来
        }
        
        return (al00.back()+al01.back()+al02.back()+al10.back()+al11.back()+al12.back())%M;
    }
};
```