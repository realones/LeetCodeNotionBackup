# Minimum Penalty for a Shop

#: 2483
Difficult: Medium
Tags: Prefix, string
link: https://leetcode.com/problems/minimum-penalty-for-a-shop/description/
Priority: High
Status: HardToImplement, checkBetterSolution
Created time: August 29, 2023 12:24 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given the customer visit log of a shop represented by a **0-indexed** string `customers` consisting only of characters `'N'` and `'Y'`:

- if the `ith` character is `'Y'`, it means that customers come at the `ith` hour
- whereas `'N'` indicates that no customers come at the `ith` hour.

If the shop closes at the `jth` hour (`0 <= j <= n`), the **penalty** is calculated as follows:

- For every hour when the shop is open and no customers come, the penalty increases by `1`.
- For every hour when the shop is closed and customers come, the penalty increases by `1`.

Return *the **earliest** hour at which the shop must be closed to incur a **minimum** penalty.*

**Note** that if a shop closes at the `jth` hour, it means the shop is closed at the hour `j`.

**Example 1:**

```
Input: customers = "YYNY"
Output: 2
Explanation:
- Closing the shop at the 0th hour incurs in 1+1+0+1 = 3 penalty.
- Closing the shop at the 1st hour incurs in 0+1+0+1 = 2 penalty.
- Closing the shop at the 2nd hour incurs in 0+0+0+1 = 1 penalty.
- Closing the shop at the 3rd hour incurs in 0+0+1+1 = 2 penalty.
- Closing the shop at the 4th hour incurs in 0+0+1+0 = 1 penalty.
Closing the shop at 2nd or 4th hour gives a minimum penalty. Since 2 is earlier, the optimal closing time is 2.

```

**Example 2:**

```
Input: customers = "NNNNN"
Output: 0
Explanation: It is best to close the shop at the 0th hour as no customers arrive.
```

**Example 3:**

```
Input: customers = "YYYY"
Output: 4
Explanation: It is best to close the shop at the 4th hour as customers arrive at each hour.

```

**Constraints:**

- `1 <= customers.length <= 105`
- `customers` consists only of characters `'Y'` and `'N'`.

```cpp
class Solution {
public:
    int bestClosingTime(string customers) {
        int n=customers.size();
        unordered_map<int,int> preCntN;//to use vector, use mp now to handle -1 and n
        unordered_map<int,int> postCntY;
        for(int i=0; i<n; i++){
            if(customers[i]=='N') preCntN[i]=preCntN[i-1]+1;
            else preCntN[i]=preCntN[i-1];
        }
        for(int i=n-1; i>=0; i--){
            if(customers[i]=='Y') postCntY[i]=postCntY[i+1]+1;
            else postCntY[i]=postCntY[i+1];
        }
        int minPen=INT_MAX;
        int res=-1;
        for(int i=0; i<=n; i++){
            if(minPen>preCntN[i-1]+postCntY[i]){
                minPen=preCntN[i-1]+postCntY[i];
                res=i;
            }
        }
        return res;
    }
};

/*
customers = [N/Y]
0 1 2 3 ... n-1:    n: 1~1e5
Y Y N N N Y

Y: come at ith hour
N: not come at ith hour

shop close at hour j( 0<=j<=n)

penalty:
- hour no visit: pen++
- close and cutomer come, pen++

earliest hour to close to incur min pen
================================

OOOOOOOOOOCCCCCCCCC

min(pre_cnt_N + post_cnt_Y)
3 sweep

*/
```