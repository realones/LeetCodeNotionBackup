# Can Place Flowers

#: 605
Difficult: Easy
Tags: Array, Greedy
link: https://leetcode.com/problems/can-place-flowers
Priority: High
Status: HardToImplement
Created time: July 1, 2023 11:54 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.

Given an integer array `flowerbed` containing `0`'s and `1`'s, where `0` means empty and `1` means not empty, and an integer `n`, return `true` *if* `n` *new flowers can be planted in the* `flowerbed` *without violating the no-adjacent-flowers rule and* `false` *otherwise*.

**Example 1:**

```
Input: flowerbed = [1,0,0,0,1], n = 1
Output: true

```

**Example 2:**

```
Input: flowerbed = [1,0,0,0,1], n = 2
Output: false

```

**Constraints:**

- `1 <= flowerbed.length <= 2 * 104`
- `flowerbed[i]` is `0` or `1`.
- There are no two adjacent flowers in `flowerbed`.
- `0 <= n <= flowerbed.length`

Solution:

idea is easy, but implementation get idx and val mixed bad.

```cpp
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int expected) {
        int n=flowerbed.size();
        int actual=0;//max can put
        for(int i=0; i<n; i++){
            if(flowerbed[i]==1) continue;
            if(i>0 && flowerbed[i-1]==1) continue;
            if(i<n-1 && flowerbed[i+1]==1) continue;
            flowerbed[i]=1;
            actual++;
        }
        return expected<=actual;
    }
};
```