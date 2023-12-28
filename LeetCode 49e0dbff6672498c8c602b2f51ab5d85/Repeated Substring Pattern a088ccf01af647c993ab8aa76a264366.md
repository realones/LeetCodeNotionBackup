# Repeated Substring Pattern

#: 459
Difficult: Easy
Tags: string
link: https://leetcode.com/problems/repeated-substring-pattern/description/
Priority: Medium
Status: More Solution
Created time: August 20, 2023 10:31 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given a string `s`, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

**Example 1:**

```
Input: s = "abab"
Output: true
Explanation: It is the substring "ab" twice.

```

**Example 2:**

```
Input: s = "aba"
Output: false

```

**Example 3:**

```
Input: s = "abcabcabcabc"
Output: true
Explanation: It is the substring "abc" four times or the substring "abcabc" twice.

```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of lowercase English letters.

Solution 1 - prime_divisors:

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int n=s.size();
        unordered_set<int> primes = primeFactorization(n);

        for(auto p: primes){
            string piece=s.substr(0,n/p);
            string tmp;
            for(int i=0; i<p; i++){
                tmp+=piece;
            }
            if(tmp==s) return true;
        }
        return false;
    }

    unordered_set<int> primeFactorization(int n) {
        unordered_set<int> factors;
        // 分解质因数
        for (int i = 2; i * i <= n; i++) {
            while (n % i == 0) {
                factors.insert(i);
                n /= i;
            }
        }
        // 处理可能的剩余质因数
        if (n > 1) {
            factors.insert(n);
        }
        return factors;
    }
};

/*
for prime_divisor from 2 to n
        try to split to prime_divisor pieces
*/
```

Solution 2: without using prime divisor to cut branches

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int n=s.size();
        for(int sz=1;sz<=n/2;sz++){
            if(n%sz!=0) continue;
            string piece = s.substr(0,sz);
            string tmp;
            for(int i=0; i<n/sz; i++){
                tmp+=piece;
            }
            if(tmp==s) return true;
        }
        return false;
    }
};

/*
for substr size: 1~n/2
    if dividable by n, try to match subtr*n/substrSz == s
*/
```

Solution 3: dup the s, check if there is any substr in the 2s == s, notice substr can’t start from 0 nor n

abcd *2 = abcdabcd

after move some how find below is same

abcd

bcda

means a=b=c=d, can be divided by 4 pieces

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int n=s.size();
        string s2=s+s;
        return s2.find(s,1) != n;
    }
};

/*
dup the s, check if there is any substr in the 2s == s, notice substr can’t start from 0 nor n

abcd *2 = abcdabcd

after move some how find below is same
    abcd
    bcda
means a=b=c=d, can be divided by 4 pieces
*
```