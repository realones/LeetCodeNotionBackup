# Apply Operations to Maximize Score

#: 2818
Difficult: Hard
link: https://leetcode.com/problems/apply-operations-to-maximize-score/
Priority: High
Status: HardToImplement, More Solution
Created time: August 12, 2023 11:06 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given an array `nums` of `n` positive integers and an integer `k`.

Initially, you start with a score of `1`. You have to maximize your score by applying the following operation at most `k` times:

- Choose any **non-empty** subarray `nums[l, ..., r]` that you haven't chosen previously.
- Choose an element `x` of `nums[l, ..., r]` with the highest **prime score**. If multiple such elements exist, choose the one with the smallest index.
- Multiply your score by `x`.

Here, `nums[l, ..., r]` denotes the subarray of `nums` starting at index `l` and ending at the index `r`, both ends being inclusive.

The **prime score** of an integer `x` is equal to the number of distinct prime factors of `x`. For example, the prime score of `300` is `3` since `300 = 2 * 2 * 3 * 5 * 5`.

Return *the **maximum possible score** after applying at most* `k` *operations*.

Since the answer may be large, return it modulo `109 + 7`.

**Example 1:**

```
Input: nums = [8,3,9,3,8], k = 2
Output: 81
Explanation: To get a score of 81, we can apply the following operations:
- Choose subarray nums[2, ..., 2]. nums[2] is the only element in this subarray. Hence, we multiply the score by nums[2]. The score becomes 1 * 9 = 9.
- Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 1, but nums[2] has the smaller index. Hence, we multiply the score by nums[2]. The score becomes 9 * 9 = 81.
It can be proven that 81 is the highest score one can obtain.
```

**Example 2:**

```
Input: nums = [19,12,14,6,10,18], k = 3
Output: 4788
Explanation: To get a score of 4788, we can apply the following operations:
- Choose subarray nums[0, ..., 0]. nums[0] is the only element in this subarray. Hence, we multiply the score by nums[0]. The score becomes 1 * 19 = 19.
- Choose subarray nums[5, ..., 5]. nums[5] is the only element in this subarray. Hence, we multiply the score by nums[5]. The score becomes 19 * 18 = 342.
- Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 2, but nums[2] has the smaller index. Hence, we multipy the score by nums[2]. The score becomes 342 * 14 = 4788.
It can be proven that 4788 is the highest score one can obtain.

```

**Constraints:**

- `1 <= nums.length == n <= 105`
- `1 <= nums[i] <= 105`
- `1 <= k <= min(n * (n + 1) / 2, 109)`

**my comment**:

first place i see modPow

there are more solutions for mono_stk/ heap..

**Solution 1 - greedy - TLE**:

I tried it in contest, hard to implement, take some time to debug, and TLE

```cpp
using ll=long long;
ll M=1e9+7;
using ii=pair<int,int>;

class Solution {
public:
    int maximumScore(vector<int>& nums, int k) {
        int n=nums.size();
        ll res=1;
        //build scores for each num
        vector<int> scores(n);
        for(int i=0;i<n;i++){
            scores[i]=countDistinctPrimeFactors(nums[i]);
        }
        //x-idx, largest to small
        vector<ii> x_idx;//if dup x, put smaller idx first
        for(int i=0; i<n; i++){
            x_idx.emplace_back(nums[i],i);
        }
        sort(x_idx.begin(), x_idx.end(),greater<>());
        //k times of op
        int opTimes=0;
        //greedy: use up largest ele, then go to next largest one
        for(auto [maxEle,maxIdx]: x_idx){
            //get cnt can use this maxEle
            int score = scores[maxIdx];
            
            int pre=maxIdx-1;
            while(pre>=0 && scores[pre]<score) pre--;//in order to use cur val, pre score should be < cur score 
                                                         //if ==, not use current one
            
            int post=maxIdx+1;
            while(post<n && scores[post]<=score) post++;//in order to use cur val, later score should be <= cur score 
            
            int l=pre+1, r=post-1;
            int cnt=(maxIdx-l+1) * (r-maxIdx+1);
            
            //*=maxEle
            for(int t=0; t<cnt; t++) {
                res=(res*maxEle)%M;
                opTimes++; if(opTimes==k) return res;
            }
        }
        return res;
    }
    
    int countDistinctPrimeFactors(int x) {
        int count = 0;

        // Handle the factor of 2
        if (x % 2 == 0) {
            count++;
            while (x % 2 == 0) {
                x /= 2;
            }
        }

        // Check for odd factors
        for (int i = 3; i <= std::sqrt(x); i += 2) {
            if (x % i == 0) {
                count++;
                while (x % i == 0) {
                    x /= i;
                }
            }
        }

        // If x becomes a prime number greater than 2
        if (x > 2) {
            count++;
        }

        return count;
    }
};

/*
seq matters -> no sorting

*/
```

**Solution - solution with improve of modPow**:

```cpp
using ll=long long;
ll M=1e9+7;
using ii=pair<int,int>;

class Solution {
public:
    int maximumScore(vector<int>& nums, int k) {
        int n=nums.size();
        ll res=1;
        //build scores for each num
        vector<int> scores(n);
        for(int i=0;i<n;i++){
            scores[i]=countDistinctPrimeFactors(nums[i]);
        }
        //x-idx, largest to small
        vector<ii> x_idx;//if dup x, put smaller idx first
        for(int i=0; i<n; i++){
            x_idx.emplace_back(nums[i],i);
        }
        sort(x_idx.begin(), x_idx.end(),greater<>());
        //k times of op
        int opTimes=0;
        //greedy: use up largest ele, then go to next largest one
        for(auto [val,idx]: x_idx){
            //get cnt can use this val
            int score = scores[idx];
            
            int pre=idx-1;
            while(pre>=0 && scores[pre]<score) pre--;//in order to use cur val, pre score should be < cur score 
                                                         //if ==, not use current one
            int post=idx+1;
            while(post<n && scores[post]<=score) post++;//in order to use cur val, later score should be <= cur score 
            
            int cnt=(idx-pre) * (post-idx);
            //*=val
            /*
            for(int t=0; t<cnt; t++) {
                res=(res*val)%M;
                opTimes++; if(opTimes==k) return res;
            }
            */
            int t=min(k-opTimes, cnt);
            res=(res * modPow(val,t,M)) % M;
            opTimes+=t; if(opTimes==k) return res;
        }
        return res;
    }
    
    int countDistinctPrimeFactors(int x) {
        int count = 0;

        // Handle the factor of 2
        if (x % 2 == 0) {
            count++;
            while (x % 2 == 0) {
                x /= 2;
            }
        }

        // Check for odd factors
        for (int i = 3; i <= std::sqrt(x); i += 2) {
            if (x % i == 0) {
                count++;
                while (x % i == 0) {
                    x /= i;
                }
            }
        }

        // If x becomes a prime number greater than 2
        if (x > 2) {
            count++;
        }

        return count;
    }

    int modPow(int x, unsigned int y, unsigned int m) {
        if (y == 0)
            return 1;
        long p = modPow(x, y / 2, m) % m;
        p = (p * p) % m;
        return y % 2 ? (p * x) % m : p;
    }
};

/*
seq matters -> no sorting
intui:
    We want to use the largest number as many times as possible.
    We can use a number as many times as the number of subarrays where that number has the largest prime score.
*/
```