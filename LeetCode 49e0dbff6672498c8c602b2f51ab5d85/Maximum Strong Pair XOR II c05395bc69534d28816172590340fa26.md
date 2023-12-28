# Maximum Strong Pair XOR II

#: 2935
Difficult: Hard
Tags: Array, Bit Manipulation, Hash, Trie
link: https://leetcode.com/problems/maximum-strong-pair-xor-ii/
Priority: High
Status: More Solution, No Idea At First, todo
Created time: November 12, 2023 7:15 PM
Last edited time: November 12, 2023 7:21 PM
source: LC_Contests
related: Maximum XOR of Two Numbers in an Array (Maximum%20XOR%20of%20Two%20Numbers%20in%20an%20Array%20fb44a67db12049edab559786595df89b.md)

You are given a **0-indexed** integer array `nums`. A pair of integers `x` and `y` is called a **strong** pair if it satisfies the condition:

- `|x - y| <= min(x, y)`

You need to select two integers from `nums` such that they form a strong pair and their bitwise `XOR` is the **maximum** among all strong pairs in the array.

Return *the **maximum*** `XOR` *value out of all possible strong pairs in the array* `nums`.

**Note** that you can pick the same integer twice to form a pair.

**Example 1:**

```
Input: nums = [1,2,3,4,5]
Output: 7
Explanation: There are 11 strong pairs in the arraynums: (1, 1), (1, 2), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (3, 5), (4, 4), (4, 5) and (5, 5).
The maximum XOR possible from these pairs is 3 XOR 4 = 7.

```

**Example 2:**

```
Input: nums = [10,100]
Output: 0
Explanation: There are 2 strong pairs in the array nums: (10, 10) and (100, 100).
The maximum XOR possible from these pairs is 10 XOR 10 = 0 since the pair (100, 100) also gives 100 XOR 100 = 0.

```

**Example 3:**

```
Input: nums = [500,520,2500,3000]
Output: 1020
Explanation: There are 6 strong pairs in the array nums: (500, 500), (500, 520), (520, 520), (2500, 2500), (2500, 3000) and (3000, 3000).
The maximum XOR possible from these pairs is 500 XOR 520 = 1020 since the only other non-zero XOR value is 2500 XOR 3000 = 636.

```

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `1 <= nums[i] <= 220 - 1`

Solution: lee215

```cpp
class Solution {
public:
    int maximumStrongPairXor(vector<int>& A) {
        int res = 0;
        for (int i = 20; i >= 0; --i) {
            res <<= 1;
            unordered_map<int, int> pref, pref2;
            for (int& a : A) {
                int p = a >> i;
                if (pref.find(p) == pref.end())
                    pref[p] = pref2[p] = a;
                pref[p] = min(pref[p], a);
                pref2[p] = max(pref2[p], a);
            }
            for (auto& it: pref) {
                int x = it.first, y = res ^ 1 ^ x;
                if (x >= y && pref.count(y) && pref[x] <= pref2[y] * 2) {
                    res |= 1;
                    break;
                }
            }
        }
        return res;
    }
};
```

Solution votrubac:

```cpp
class Solution {
public:
    struct xorTrie {
        static constexpr int max_b = 1 << 19;
        xorTrie *p[2] = {};
        void insert(int n) {
            xorTrie *r = this;
            for (int b = max_b; b > 0; b >>= 1) {
                int bit = (n & b) > 0;
                if (r->p[bit] == nullptr)
                    r->p[bit] = new xorTrie();
                r = r->p[bit];
            }
        }
        bool remove(int n, int b = max_b) {
            if (b == 0)
                return true;
            int bit = (n & b) > 0;
            if (p[bit] != nullptr && p[bit]->remove(n, b >> 1)) {
                delete p[bit];
                p[bit] = nullptr;
            }
            return p[0] == p[1]; // both are nullptr
        }    
        int maxXor(int n) {
            int res = 0;
            xorTrie *r = this;
            for (int b = max_b; b > 0; b >>= 1)
                if (int bit = (n & b) > 0; r->p[!bit] != nullptr) {
                    res += b;
                    r = r->p[!bit];
                }
                else
                    r = r->p[bit];
            return res;
        }  
    };
    int maximumStrongPairXor(vector<int>& nums) {
        xorTrie t;
        sort(begin(nums), end(nums));
        int i = 0, j = 0, res = 0; 
        for (int i = 0; i < nums.size(); ++i) {
            t.insert(nums[i]);
            while (nums[j] * 2 < nums[i])
                t.remove(nums[j++]);
            res = max(res, t.maxXor(nums[i]));
        }
        return res;
    }
};
```