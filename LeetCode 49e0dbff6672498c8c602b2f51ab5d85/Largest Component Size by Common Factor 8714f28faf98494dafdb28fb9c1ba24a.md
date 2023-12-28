# Largest Component Size by Common Factor

#: 952
Difficult: Hard
Tags: Array, Graph, Math, PrimeFactor, Union Find
link: https://leetcode.com/problems/largest-component-size-by-common-factor/
Priority: Medium
Status: checkBetterSolution
Created time: June 28, 2023 12:24 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given an integer array of unique positive integers `nums`. Consider the following graph:

- There are `nums.length` nodes, labeled `nums[0]` to `nums[nums.length - 1]`,
- There is an undirected edge between `nums[i]` and `nums[j]` if `nums[i]` and `nums[j]` share a common factor greater than `1`.

Return *the size of the largest connected component in the graph*.

**Example 1:**

![https://assets.leetcode.com/uploads/2018/12/01/ex1.png](https://assets.leetcode.com/uploads/2018/12/01/ex1.png)

```
Input: nums = [4,6,15,35]
Output: 4

```

**Example 2:**

![https://assets.leetcode.com/uploads/2018/12/01/ex2.png](https://assets.leetcode.com/uploads/2018/12/01/ex2.png)

```
Input: nums = [20,50,9,63]
Output: 2

```

**Example 3:**

![https://assets.leetcode.com/uploads/2018/12/01/ex3.png](https://assets.leetcode.com/uploads/2018/12/01/ex3.png)

```
Input: nums = [2,3,6,7,4,12,21,39]
Output: 8

```

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `1 <= nums[i] <= 105`
- All the values of `nums` are **unique**.

comment: 两个loop会超时的话，就用prime factors作为桥梁

Solution unionFind - TLE:

```cpp
vector<int> father;
void init(int n) {
    father.resize(n,0);
    for(int i=0;i<n;i++){
        father[i]=i;//parent is itself
    }
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
        return true;
    }
    return false;
}
```

Solution unionFind + primeFactors improvement

```cpp
class Solution {
public:
    unordered_map<int,int> father;

    //with pass compression
    // O(1)
    int find(int a){
        if(!father.count(a)) father[a]=a;
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

    int largestComponentSize(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        unordered_set<int> primeSt;
        for(int i=0;i<n;i++){
            unordered_set<int> primes=primeFactorization(nums[i]);
            for(auto prime: primes){
                Union(i,prime+n);
                primeSt.insert(prime);
            }
        }
        unordered_map<int,int> mp;//root_sz
        for(int i=0; i<n; i++){
            mp[find(i)]++;
        }
        
        int res=0;
        for(auto [_,sz]: mp){
            res=max(res,sz);
        }
        return res;
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
```