# Number of Days Between Two Dates

#: 1360
Difficult: Easy
Tags: Math, string
link: https://leetcode.com/problems/number-of-days-between-two-dates/
Priority: Medium
Status: HardToImplement
Created time: December 16, 2023 1:32 AM
Last edited time: December 16, 2023 1:33 AM
source: googleFreq

Write a program to count the number of days between two dates.

The two dates are given as strings, their format is `YYYY-MM-DD` as shown in the examples.

**Example 1:**

```
Input: date1 = "2019-06-29", date2 = "2019-06-30"
Output: 1

```

**Example 2:**

```
Input: date1 = "2020-01-15", date2 = "2019-12-31"
Output: 15

```

**Constraints:**

- The given dates are valid dates between the years `1971` and `2100`.

```cpp
//ans from votrubac
class Solution {
public:
    int days[12] = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
    int daysBetweenDates(string d1, string d2) {
        return abs(daysFrom1971(d1) - daysFrom1971(d2));
    }
    bool isLeap(int y) { 
        return y % 4 == 0 && (y % 100 != 0 || y % 400 == 0); 
    }
    int daysFrom1971(string dt) {
        int y = stoi(dt.substr(0, 4)), m = stoi(dt.substr(5, 2)), d = stoi(dt.substr(8));
        for (int iy = 1971; iy < y; ++iy) 
            d += isLeap(iy) ? 366 : 365;
        return d + (m > 2 && isLeap(y)) + accumulate(begin(days), begin(days) + m - 1, 0);
    }
};
```