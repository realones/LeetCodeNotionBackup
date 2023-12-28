# Number of Valid Clock Times

#: 2437
Difficult: Easy
Tags: simulation, string
link: https://leetcode.com/problems/number-of-valid-clock-times/description/
Priority: Pass
Created time: December 16, 2023 10:44 AM
Last edited time: December 16, 2023 10:45 AM
source: googleFreq

Topics

Companies

Hint

You are given a string of length `5` called `time`, representing the current time on a digital clock in the format `"hh:mm"`. The **earliest** possible time is `"00:00"` and the **latest** possible time is `"23:59"`.

In the string `time`, the digits represented by the `?` symbol are **unknown**, and must be **replaced** with a digit from `0` to `9`.

Return *an integer* `answer`*, the number of valid clock times that can be created by replacing every* `?` *with a digit from* `0` *to* `9`.

**Example 1:**

```
Input: time = "?5:00"
Output: 2
Explanation: We can replace the ? with either a 0 or 1, producing "05:00" or "15:00". Note that we cannot replace it with a 2, since the time "25:00" is invalid. In total, we have two choices.

```

**Example 2:**

```
Input: time = "0?:0?"
Output: 100
Explanation: Each ? can be replaced by any digit from 0 to 9, so we have 100 total choices.

```

**Example 3:**

```
Input: time = "??:??"
Output: 1440
Explanation: There are 24 possible choices for the hours, and 60 possible choices for the minutes. In total, we have 24 * 60 = 1440 choices.

```

**Constraints:**

- `time` is a valid string of length `5` in the format `"hh:mm"`.
- `"00" <= hh <= "23"`
- `"00" <= mm <= "59"`
- Some of the digits might be replaced with `'?'` and need to be replaced with digits from `0` to `9`.

```cpp
class Solution {
public:
    int countTime(string time) {
        char h1=time[0];
        char h2=time[1];
        char m1=time[3];
        char m2=time[4];
        int res=1;
        //m
        if(m1=='?') res*=6;//0~5
        if(m2=='?') res*=10;//0~9
        //h
        if(h1=='?' && h2=='?') res*=24;
        else if(h1=='?'){
            if(h2<='3') res*=3;//0,1,2
            else res*=2;//0,1
        }
        else if(h2=='?'){
            if(h1<='1') res*=10;//0~9
            else res*=4;
        }
        return res;
    }
};
```