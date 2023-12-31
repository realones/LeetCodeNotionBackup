# Koko Eating Bananas

#: 875
Difficult: Medium
Tags: Array, BS_Val
link: https://leetcode.com/problems/koko-eating-bananas
Priority: Low
Created time: July 14, 2023 3:59 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75

Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return *the minimum integer* `k` *such that she can eat all the bananas within* `h` *hours*.

**Example 1:**

```
Input: piles = [3,6,7,11], h = 8
Output: 4

```

**Example 2:**

```
Input: piles = [30,11,23,4,20], h = 5
Output: 30

```

**Example 3:**

```
Input: piles = [30,11,23,4,20], h = 6
Output: 23

```

**Constraints:**

- `1 <= piles.length <= 104`
- `piles.length <= h <= 109`
- `1 <= piles[i] <= 109`

```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int l=1,r=INT_MAX;
        while(l<r){
            int m=l+(r-l)/2;
            if(!valid(piles,h,m)) l=m+1;
            else r=m;
        }
        return l;
    }

    bool valid(vector<int>& piles, int h, int k){
        int actualTime=0;
        for(auto p: piles){
            actualTime+=p/k+ (p%k==0?0:1) ;
        }
        return actualTime<=h;
    }
};

/*
bs_val
XXXXXYYYYY

*
```