# Maximum Element-Sum of a Complete Subset of Indices

#: 2862
Difficult: Hard
Tags: Hash, Math, PrimeFactor
link: https://leetcode.com/problems/maximum-element-sum-of-a-complete-subset-of-indices/description/
Priority: High
Status: No Idea At First, toRevisit
Created time: September 16, 2023 10:43 PM
Last edited time: October 17, 2023 6:24 PM
source: LC_Contests

You are given a **1-indexed** array `nums` of `n` integers.

A set of numbers is **complete** if the product of every pair of its elements is a perfect square.

For a subset of the indices set `{1, 2, ..., n}` represented as `{i1, i2, ..., ik}`, we define its **element-sum** as: `nums[i1] + nums[i2] + ... + nums[ik]`.

Return *the **maximum element-sum** of a **complete** subset of the indices set* `{1, 2, ..., n}`.

A perfect square is a number that can be expressed as the product of an integer by itself.

**Example 1:**

```
Input: nums = [8,7,3,5,7,2,4,9]
Output: 16
Explanation: Apart from the subsets consisting of a single index, there are two other complete subsets of indices: {1,4} and {2,8}.
The sum of the elements corresponding to indices 1 and 4 is equal to nums[1] + nums[4] = 8 + 5 = 13.
The sum of the elements corresponding to indices 2 and 8 is equal to nums[2] + nums[8] = 7 + 9 = 16.
Hence, the maximum element-sum of a complete subset of indices is 16.

```

**Example 2:**

```
Input: nums = [5,10,3,10,1,13,7,9,4]
Output: 19
Explanation: Apart from the subsets consisting of a single index, there are four other complete subsets of indices: {1,4}, {1,9}, {2,8}, {4,9}, and {1,4,9}.
The sum of the elements conrresponding to indices 1 and 4 is equal to nums[1] + nums[4] = 5 + 10 = 15.
The sum of the elements conrresponding to indices 1 and 9 is equal to nums[1] + nums[9] = 5 + 4 = 9.
The sum of the elements conrresponding to indices 2 and 8 is equal to nums[2] + nums[8] = 10 + 9 = 19.
The sum of the elements conrresponding to indices 4 and 9 is equal to nums[4] + nums[9] = 10 + 4 = 14.
The sum of the elements conrresponding to indices 1, 4, and 9 is equal to nums[1] + nums[4] + nums[9] = 5 + 10 + 4 = 19.
Hence, the maximum element-sum of a complete subset of indices is 19.

```

**Constraints:**

- `1 <= n == nums.length <= 104`
- `1 <= nums[i] <= 109`

Solution

```cpp
class Solution {
public:
    long long maximumSum(vector<int>& nums) {
        int n=nums.size();
        //get factors
        vector<int> factors = findPrimes((int)floor(pow(n,0.5)));
        for(auto& f: factors) f=pow(f,2);
        //group nums
        unordered_map<int,vector<int>> mp;//root_num
        for(int i=1;i<=n;i++){
            int root=i;
            for(auto f: factors){
                if(root>=f) while(root%f==0) root/=f;
            }
            mp[root].push_back(nums[i-1]);
        }
        //get max sum of those groups
        long long res=0;
        for(auto [root,v]: mp){
            res=max(res, accumulate(v.begin(), v.end(), 0LL));
        }
        return res;
    }

    //Sieve of Eratosthenes -- T: O(n log log n), M: O(~n/ln(n))
    vector<int> findPrimes(int n) {
        vector<bool> isPrime(n + 1, true); // 初始化一个标记数组，默认所有数都是质数

        for (int i = 2; i * i <= n; i++) {
            if (isPrime[i]) { // 如果当前数是质数
                for (int j = i * i; j <= n; j += i) {
                    isPrime[j] = false; // 将当前质数的倍数标记为非质数
                }
            }
        }

        std::vector<int> primes;
        for (int i = 2; i <= n; i++) {
            if (isPrime[i]) {
                primes.push_back(i); // 将标记为质数的数加入到结果数组中
            }
        }

        return primes;
    }
};

/*
1,2,...,n, find complete subsets 
in all complete subsets, find the biggest sum of subsets

intui:
    for each num: can be divided to a * x^2 * y^2 * ...
    let's call a is the root of number, then all number with root==a can be grouped together
    root doesn't need to be prime number

    for x,y,.., we only care about prime numbers, since (2*3)^2 = 2^2 * 3^2
    let's say those x^2, y^2 as factors

impl:
    sort nums (1e4)
    for each num
        /= factors recursively if can divide
        the leftover is root
        put into mp[root]=st of numbers
    for each group, check sum, return the max sum
*/
```