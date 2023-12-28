# Sort Integers by The Number of 1 Bits

#: 1356
Difficult: Easy
Tags: Array, Bit Manipulation, Counting, Sort
link: https://leetcode.com/problems/sort-integers-by-the-number-of-1-bits/
Priority: Pass
Created time: November 1, 2023 8:46 PM
Last edited time: November 1, 2023 8:47 PM
source: leetcode_daily

You are given an integer array `arr`. Sort the integers in the array in ascending order by the number of `1`'s in their binary representation and in case of two or more integers have the same number of `1`'s you have to sort them in ascending order.

Return *the array after sorting it*.

**Example 1:**

```
Input: arr = [0,1,2,3,4,5,6,7,8]
Output: [0,1,2,4,8,3,5,6,7]
Explantion: [0] is the only integer with 0 bits.
[1,2,4,8] all have 1 bit.
[3,5,6] have 2 bits.
[7] has 3 bits.
The sorted array by bits is [0,1,2,4,8,3,5,6,7]

```

**Example 2:**

```
Input: arr = [1024,512,256,128,64,32,16,8,4,2,1]
Output: [1,2,4,8,16,32,64,128,256,512,1024]
Explantion: All integers have 1 bit in the binary representation, you should just sort them in ascending order.

```

**Constraints:**

- `1 <= arr.length <= 500`
- `0 <= arr[i] <= 104`

```cpp
bool cmp(const int &x, const int &y) {
    int a=x;
    int cnt1=0;
    while(a){
        cnt1+= a&1;
        a>>=1;
    }

    int b=y;
    int cnt2=0;
    while(b){
        cnt2+= b&1;
        b>>=1;
    }

    if(cnt1!=cnt2) return cnt1<cnt2;
    return x<y;
}

class Solution {
public:
    vector<int> sortByBits(vector<int>& nums) {
        sort(nums.begin(), nums.end(), cmp);
        return nums;
    }
};
```