# Arranging Coins

#: 441
Difficult: Easy
Tags: BS_Val, Math
link: https://leetcode.com/problems/arranging-coins/description/
Priority: Pass
Created time: December 24, 2023 11:31 PM
Last edited time: December 24, 2023 11:32 PM
source: googleFreq

You have `n` coins and you want to build a staircase with these coins. The staircase consists of `k` rows where the `ith` row has exactly `i` coins. The last row of the staircase **may be** incomplete.

Given the integer `n`, return *the number of **complete rows** of the staircase you will build*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/09/arrangecoins1-grid.jpg](https://assets.leetcode.com/uploads/2021/04/09/arrangecoins1-grid.jpg)

```
Input: n = 5
Output: 2
Explanation: Because the 3rd row is incomplete, we return 2.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/04/09/arrangecoins2-grid.jpg](https://assets.leetcode.com/uploads/2021/04/09/arrangecoins2-grid.jpg)

```
Input: n = 8
Output: 3
Explanation: Because the 4th row is incomplete, we return 3.

```

**Constraints:**

- `1 <= n <= 231 - 1`

```java
class Solution {
  public int arrangeCoins(int n) {
    long left = 0, right = n;
    long k, curr;
    while (left <= right) {
      k = left + (right - left) / 2;
      curr = k * (k + 1) / 2;

      if (curr == n) return (int)k;

      if (n < curr) {
        right = k - 1;
      } else {
        left = k + 1;
      }
    }
    return (int)right;
  }
}
```