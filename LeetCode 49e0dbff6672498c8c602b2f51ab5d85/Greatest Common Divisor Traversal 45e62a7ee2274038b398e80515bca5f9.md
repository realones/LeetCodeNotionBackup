# Greatest Common Divisor Traversal

#: 2709
Difficult: Hard
Tags: PrimeFactor, Union Find
link: https://leetcode.com/problems/greatest-common-divisor-traversal/
Priority: High
Status: No Idea At First
Created time: May 28, 2023 9:44 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** integer array `nums`, and you are allowed to **traverse** between its indices. You can traverse between index `i` and index `j`, `i != j`, if and only if `gcd(nums[i], nums[j]) > 1`, where `gcd` is the **greatest common divisor**.

Your task is to determine if for **every pair** of indices `i` and `j` in nums, where `i < j`, there exists a **sequence of traversals** that can take us from `i` to `j`.

Return `true` *if it is possible to traverse between all such pairs of indices, or* `false` *otherwise.*

**Example 1:**

```
Input: nums = [2,3,6]
Output: true
Explanation: In this example, there are 3 possible pairs of indices: (0, 1), (0, 2), and (1, 2).
To go from index 0 to index 1, we can use the sequence of traversals 0 -> 2 -> 1, where we move from index 0 to index 2 because gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1, and then move from index 2 to index 1 because gcd(nums[2], nums[1]) = gcd(6, 3) = 3 > 1.
To go from index 0 to index 2, we can just go directly because gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1. Likewise, to go from index 1 to index 2, we can just go directly because gcd(nums[1], nums[2]) = gcd(3, 6) = 3 > 1.

```

**Example 2:**

```
Input: nums = [3,9,5]
Output: false
Explanation: No sequence of traversals can take us from index 0 to index 2 in this example. So, we return false.

```

**Example 3:**

```
Input: nums = [4,3,12,8]
Output: true
Explanation: There are 6 possible pairs of indices to traverse between: (0, 1), (0, 2), (0, 3), (1, 2), (1, 3), and (2, 3). A valid sequence of traversals exists for each pair, so we return true.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`

Solution 1:

```cpp
class Solution {
public:
    bool canTraverseAllPairs(vector<int>& nums) {
        int maxEle = *max_element(nums.begin(), nums.end());
        vector<int> primes=findPrimes(maxEle);
        
        for(auto num: nums){//for each num, unit possible primes
            if(num==1) return nums.size()==1;
            int lastP=-1;
            for (auto p: primes) {
                if(p>num) break;
                if(p*p>num) {
                    if(!father.count(num)) father[num]=num;//init unionfind
                    if(lastP!=-1) Union(num,lastP);
                    break;
                }
                if (num%p == 0) {
                    if(!father.count(p)) father[p]=p;//init unionfind
                    if(lastP!=-1) Union(p,lastP);
                    lastP=p;
                    while(num%p == 0) num/=p;
                }
            }
        }
        
        int root=-1;
        for(auto [p,_]: father){
            if(root==-1) root=find(p);
            else{
                if(find(p)!=root) return false;
            }
        }
        return true;
    }
    
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
    
    vector<int> factorize(vector<int> primes, int num) {
        vector<int> factor;
        
        return factor;
    }
    
    unordered_map<int,int> father;

    //with pass compression
    // O(1)
    int find(int a){
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    //union - union roots, tree like
    // O(1)
    bool Union(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            return true;
        }
        return false;
    }
};
```

Solution TLE (just because using vector in factorize):

```cpp
class Solution {
public:
    bool canTraverseAllPairs(vector<int>& nums) {
        int maxEle = *max_element(nums.begin(), nums.end());
        vector<int> primes=findPrimes(maxEle);
        
        for(auto num: nums){//for each num, unit possible primes
            if(num==1) return nums.size()==1;
            int lastP=-1;
            for(auto p: factorize(primes, num)){
                if(!father.count(p)) father[p]=p;//init unionfind
                if(lastP!=-1) Union(p,lastP);
                lastP=p;
            }
        }
        
        int root=-1;
        for(auto [p,_]: father){
            if(root==-1) root=find(p);
            else{
                if(find(p)!=root) return false;
            }
        }
        return true;
    }
    
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
    
    vector<int> factorize(vector<int> primes, int num) {
        vector<int> factor;
        for (auto p: primes) {
            if(p>num) break;
            if(p*p>num) {
                factor.push_back(num);
                break;
            }
            if (num%p == 0) {
                factor.push_back(p);
                while(num%p == 0) num/=p;
            }
        }
        return factor;
    }
    
    unordered_map<int,int> father;

    //with pass compression
    // O(1)
    int find(int a){
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    //union - union roots, tree like
    // O(1)
    bool Union(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            return true;
        }
        return false;
    }
};
```

Solution TLE:

```cpp
//1e5 -> 3e3 primes
//1e5 * 3e3 = 3e8, will not pass

class Solution {
public:
    bool canTraverseAllPairs(vector<int>& nums) {
        int minEle = *min_element(nums.begin(), nums.end());
        if(minEle==1) return nums.size()==1;
        
        unordered_set<int> st(nums.begin(),nums.end());
        nums = vector<int>(st.begin(), st.end());
        
        int maxEle = *max_element(nums.begin(), nums.end());
        vector<int> primes=findPrimes(maxEle);
        init(primes.size());
        unordered_set<int> visitedP;
        for(auto num: nums){//for each num, unit possible primes
            int lastPrimeIdx=-1;
            for(int i=0;i<primes.size();i++){
                if(num%primes[i]==0){
                    visitedP.insert(primes[i]);
                    if(lastPrimeIdx!=-1) Union(i,lastPrimeIdx);
                    lastPrimeIdx=i;
                }
            }
        }
        return sz==primes.size()-visitedP.size()+1;
    }
    
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
    
    vector<int> father;
    int sz;
    void init(int n) {
        father.resize(n,0);
        for(int i=0;i<n;i++){
            father[i]=i;//parent is itself
        }
        sz=n;
    }

    //with pass compression
    // O(1)
    int find(int a){
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    //union - union roots, tree like
    // O(1)
    bool Union(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            sz--;
            return true;
        }
        return false;
    }
};
```

Solution: TLE

```cpp
class Solution {
public:
    vector<int> father;
    int sz;
    void init(int n) {
        father.resize(n,0);
        for(int i=0;i<n;i++){
            father[i]=i;//parent is itself
        }
        sz=n;
    }

    //with pass compression
    // O(1)
    int find(int a){
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    //union - union roots, tree like
    // O(1)
    bool Union(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            sz--;
            return true;
        }
        return false;
    }
    
    bool canTraverseAllPairs(vector<int>& nums) {
        int n=nums.size();
        init(n);
        for(int i=1; i<n; i++){
            for(int j=0; j<i; j++){
                if(gcd(nums[i],nums[j])>1) Union(i,j);
            }
        }
        return sz==1;
    }
    
    int gcd(int a, int b){
        return b==0?a:gcd(b,a%b);
    }
};

/*
1 <= nums.length <= 1e5
1 <= nums[i] <= 1e5

T: n
T: nlogn

can traverse i to j (i<j), if gcd(nums[i], nums[j]) > 1 ->edge

for every pair(i,j) i<j

unionfind

*/
```