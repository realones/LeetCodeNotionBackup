# [lint] Top k Largest Numbers II

#: 545
Difficult: Medium
Tags: PQ, TopK
link: https://www.lintcode.com/problem/top-k-largest-numbers-ii
Priority: Pass
Created time: August 11, 2022 7:19 PM
Last edited time: October 21, 2023 4:06 PM
practicedTimes: 1
source: jiuzhang, 算初Hash&Heap

Implement a data structure, provide two interfaces:

1. `add(number)`. Add a new number in the data structure.
2. `topk()`. Return the top *k* largest numbers in this data structure. *k* is given when we create the data structure.

Example:
**Example1**

```
Input:
s = new Solution(3);
s.add(3)
s.add(10)
s.topk()
s.add(1000)
s.add(-99)
s.topk()
s.add(4)
s.topk()
s.add(100)
s.topk()

Output:
[10, 3]
[1000, 10, 3]
[1000, 10, 4]
[1000, 100, 10]

Explanation:
s = new Solution(3);
>> create a new data structure, and k = 3.
s.add(3)
s.add(10)
s.topk()
>> return [10, 3]
s.add(1000)
s.add(-99)
s.topk()
>> return [1000, 10, 3]
s.add(4)
s.topk()
>> return [1000, 10, 4]
s.add(100)
s.topk()
>> return [1000, 100, 10]

```

**Example2**

```
Input:
s = new Solution(1);
s.add(3)
s.add(10)
s.topk()
s.topk()

Output:
[10]
[10]

Explanation:
s = new Solution(1);
>> create a new data structure, and k = 1.
s.add(3)
s.add(10)
s.topk()
>> return [10]
s.topk()
>> return [10]

```

reverse order to think

```cpp
class Solution {
private:
    priority_queue<int,vector<int>,greater<int>> q;
    int size;

public:
    /*
    * @param k: An integer
    */Solution(int k) {
        size=k;
    }

    /*
     * @param num: Number to be added
     * @return: nothing
     */
    void add(int num) {
        q.push(num);
        if(q.size()>size) q.pop();
    }

    /*
     * @return: Top k element
     */
    vector<int> topk() {
        vector<int> res;
        while(!q.empty()){
            res.push_back(q.top());q.pop();
        }

        for (auto num:res){q.push(num);}

        reverse(res.begin(),res.end());
        return res;
    }
};
```