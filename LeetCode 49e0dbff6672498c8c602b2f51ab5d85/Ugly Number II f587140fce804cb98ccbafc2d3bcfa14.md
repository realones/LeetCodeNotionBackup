# Ugly Number II

#: 264
Difficult: Medium
Tags: DP, Hash, Math, PQ, kth
link: https://leetcode.com/problems/ugly-number-ii/
Priority: High
Status: Dig More, No Idea At First
Created time: August 11, 2022 7:19 PM
Last edited time: October 21, 2023 4:03 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初DP, 算初Hash&Heap

An **ugly number** is a positive integer whose prime factors are limited to `2`, `3`, and `5`.

Given an integer `n`, return *the* `nth` ***ugly number***.

**Example 1:**

```
Input: n = 10
Output: 12
Explanation: [1, 2, 3, 4, 5, 6, 8, 9, 10, 12] is the sequence of the first 10 ugly numbers.

```

**Example 2:**

```
Input: n = 1
Output: 1
Explanation: 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.

```

**Constraints:**

- `1 <= n <= 1690`

DP or PQ

DP

但是对于DP的方法，要考虑是不是可以从之前的状态推过来，可以使用数学归纳法，

- 找关系，找规律：往前推，每一个数都会乘以2and3and5推出后来的数
- 从头开始推一推，
- 从尾巴倒推一下，
- 然后中间值t如果成立，t会导致后面的什么结果成立
- **如果要使得t成立，前面需要哪些成立**

**然后在脑子里建模。**

通过数学归纳法，很容易发现t成立需要的前提条件，然后罗列一下相互关系，脑子里建建模型。

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        if(n<=0) return 0;
        if(n==1) return 1;
        
        int t2=0,t3=0,t5=0;
        vector<int> k(n,0);
        k[0]=1;
        for(int i=1;i<n;i++){
            k[i]=min(k[t2]*2,min(k[t3]*3,k[t5]*5));
            if(k[i] == k[t2]*2) t2++;
            if(k[i] == k[t3]*3) t3++;
            if(k[i] == k[t5]*5) t5++;
        }
        return k[n-1];
    }
};
```

PQ - java

```cpp
// version 2 O(nlogn) HashMap + Heap
class Solution {
    /**
     * @param n an integer
     * @return the nth prime number as description.
     */
    public int nthUglyNumber(int n) {
        // Write your code here
        Queue<Long> Q = new PriorityQueue<Long>();
        HashSet<Long> inQ = new HashSet<Long>();
        Long[] primes = new Long[3];
        primes[0] = Long.valueOf(2);
        primes[1] = Long.valueOf(3);
        primes[2] = Long.valueOf(5);
        for (int i = 0; i < 3; i++) {
            Q.add(primes[i]);
            inQ.add(primes[i]);
        }
        Long number = Long.valueOf(1);
        for (int i = 1; i < n; i++) {
            number = Q.poll();
            for (int j = 0; j < 3; j++) {
                if (!inQ.contains(primes[j] * number)) {
                    Q.add(number * primes[j]);
                    inQ.add(number * primes[j]);
                }
            }
        }
        return number.intValue();
    }
};
```