# Student Attendance Record I

#: 551
Difficult: Easy
Tags: string
link: https://leetcode.com/problems/student-attendance-record-i/
Priority: Low
Created time: October 6, 2022 1:39 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, googleFreq

You are given a string `s` representing an attendance record for a student where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

- `'A'`: Absent.
- `'L'`: Late.
- `'P'`: Present.

The student is eligible for an attendance award if they meet **both** of the following criteria:

- The student was absent (`'A'`) for **strictly** fewer than 2 days **total**.
- The student was **never** late (`'L'`) for 3 or more **consecutive** days.

Return `true` *if the student is eligible for an attendance award, or* `false` *otherwise*.

**Example 1:**

```
Input: s = "PPALLP"
Output: true
Explanation: The student has fewer than 2 absences and was never late 3 or more consecutive days.

```

**Example 2:**

```
Input: s = "PPALLL"
Output: false
Explanation: The student was late 3 consecutive days in the last 3 days, so is not eligible for the award.

```

**Constraints:**

- `1 <= s.length <= 1000`
- `s[i]` is either `'A'`, `'L'`, or `'P'`.

```cpp
class Solution {
public:
    bool checkRecord(string s) {
        int n=s.size(); if(n==0) return false;
        int numA=0, consecutiveNumL=0;
        for(int i=0; i<n; i++){
            char c=s[i];
            if(c=='A') {
                numA++;
                if(numA>=2) return false;
            }
            if(c=='L'){
                consecutiveNumL+=1;
                if(consecutiveNumL>=3) return false;
            }
            else{consecutiveNumL=0;}
        }
        return true;
    }
};
```