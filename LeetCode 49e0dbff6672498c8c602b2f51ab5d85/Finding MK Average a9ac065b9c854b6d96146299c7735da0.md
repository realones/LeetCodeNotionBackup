# Finding MK Average

#: 1825
Difficult: Hard
Tags: Data_stream, Design, OrderSet, PQ, deque
link: https://leetcode.com/problems/finding-mk-average/
Priority: High
Status: HardToImplement
Created time: September 4, 2023 4:23 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given two integers, `m` and `k`, and a stream of integers. You are tasked to implement a data structure that calculates the **MKAverage** for the stream.

The **MKAverage** can be calculated using these steps:

1. If the number of the elements in the stream is less than `m` you should consider the **MKAverage** to be `1`. Otherwise, copy the last `m` elements of the stream to a separate container.
2. Remove the smallest `k` elements and the largest `k` elements from the container.
3. Calculate the average value for the rest of the elements **rounded down to the nearest integer**.

Implement the `MKAverage` class:

- `MKAverage(int m, int k)` Initializes the **MKAverage** object with an empty stream and the two integers `m` and `k`.
- `void addElement(int num)` Inserts a new element `num` into the stream.
- `int calculateMKAverage()` Calculates and returns the **MKAverage** for the current stream **rounded down to the nearest integer**.

**Example 1:**

```
Input
["MKAverage", "addElement", "addElement", "calculateMKAverage", "addElement", "calculateMKAverage", "addElement", "addElement", "addElement", "calculateMKAverage"]
[[3, 1], [3], [1], [], [10], [], [5], [5], [5], []]
Output
[null, null, null, -1, null, 3, null, null, null, 5]

ExplanationMKAverage obj = new MKAverage(3, 1);
obj.addElement(3);        // current elements are [3]
obj.addElement(1);        // current elements are [3,1]
obj.calculateMKAverage(); // return -1, because m = 3 and only 2 elements exist.
obj.addElement(10);       // current elements are [3,1,10]
obj.calculateMKAverage(); // The last 3 elements are [3,1,10].
                          // After removing smallest and largest 1 element the container will be [3].
                          // The average of [3] equals 3/1 = 3, return 3
obj.addElement(5);        // current elements are [3,1,10,5]
obj.addElement(5);        // current elements are [3,1,10,5,5]
obj.addElement(5);        // current elements are [3,1,10,5,5,5]
obj.calculateMKAverage(); // The last 3 elements are [5,5,5].
                          // After removing smallest and largest 1 element the container will be [5].
                          // The average of [5] equals 5/1 = 5, return 5

```

**Constraints:**

- `3 <= m <= 105`
- `1 <= k*2 < m`
- `1 <= num <= 105`
- At most `105` calls will be made to `addElement` and `calculateMKAverage`.

comment: fucked by this quesiont, I spent >2h on this question, super hard for me to implement…

Solution: roll over from left to right

```cpp
class MKAverage {
public:
    int m,k;
    vector<multiset<int>> st;//left mid right
    vector<int> stSz;
    vector<long long> stSum;
    deque<int> dq;

    MKAverage(int m, int k) {
        this->m=m, this->k=k;
        st.resize(3);
        stSz={k,m-2*k,k};
        stSum.resize(3,0LL);
    }
    
    void addElement(int num) {
        dq.push_back(num);

        //trim left
        if(dq.size()>m){ //if full
            int toRemove=dq.front();
            dq.pop_front();

            for(int i=0; i<3; i++){
                if(toRemove <= *st[i].rbegin()){
                    st[i].erase(st[i].find(toRemove)); stSum[i]-=toRemove;
                    break;
                }
            }
        }

        //insert
        st[0].insert(num); stSum[0]+=num;
        for(int i=0; i<2; i++){
            if(st[i].size()>stSz[i]){
                int a=*prev(st[i].end()); st[i].erase(prev(st[i].end())); stSum[i]-=a;
                st[i+1].insert(a); stSum[i+1]+=a;
            }
            else{
                if(!st[i].empty() && !st[i+1].empty()){
                    int a=*prev(st[i].end()); st[i].erase(prev(st[i].end())); stSum[i]-=a;
                    int b=*st[i+1].begin(); st[i+1].erase(st[i+1].begin()); stSum[i+1]-=b;
                    st[i].insert(min(a,b)); stSum[i]+=min(a,b);
                    st[i+1].insert(max(a,b)); stSum[i+1]+=max(a,b);
                }
            }
        }
    }

    int calculateMKAverage() {
        if(dq.size()<m) return -1;
        return stSum[1]/stSz[1];
    }

};

/**
 * Your MKAverage object will be instantiated and called as such:
 * MKAverage* obj = new MKAverage(m, k);
 * obj->addElement(num);
 * int param_2 = obj->calculateMKAverage();
 */

/*
3 <= m <= 1e5
1 <= k*2 < m
1 <= num <= 1e5
At most 1e5 calls will be made to addElement and calculateMKAverage.

...... ##### ......

*/
```