# Count Integers in Intervals

#: 2276
Difficult: Hard
Tags: BS_lower_upper_bound, OrderMap, SegmentTree_idx
link: https://leetcode.com/problems/count-integers-in-intervals/
Priority: Medium
Status: More Solution
Created time: October 6, 2022 2:01 AM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

Given an **empty** set of intervals, implement a data structure that can:

- **Add** an interval to the set of intervals.
- **Count** the number of integers that are present in **at least one** interval.

Implement the `CountIntervals` class:

- `CountIntervals()` Initializes the object with an empty set of intervals.
- `void add(int left, int right)` Adds the interval `[left, right]` to the set of intervals.
- `int count()` Returns the number of integers that are present in **at least one** interval.

**Note** that an interval `[left, right]` denotes all the integers `x` where `left <= x <= right`.

**Example 1:**

```
Input
["CountIntervals", "add", "add", "count", "add", "count"]
[[], [2, 3], [7, 10], [], [5, 8], []]
Output
[null, null, null, 6, null, 8]

Explanation
CountIntervals countIntervals = new CountIntervals(); // initialize the object with an empty set of intervals.
countIntervals.add(2, 3);  // add [2, 3] to the set of intervals.
countIntervals.add(7, 10); // add [7, 10] to the set of intervals.
countIntervals.count();    // return 6
                           // the integers 2 and 3 are present in the interval [2, 3].
                           // the integers 7, 8, 9, and 10 are present in the interval [7, 10].
countIntervals.add(5, 8);  // add [5, 8] to the set of intervals.
countIntervals.count();    // return 8
                           // the integers 2 and 3 are present in the interval [2, 3].
                           // the integers 5 and 6 are present in the interval [5, 8].
                           // the integers 7 and 8 are present in the intervals [5, 8] and [7, 10].
                           // the integers 9 and 10 are present in the interval [7, 10].

```

**Constraints:**

- `1 <= left <= right <= 109`
- At most `105` calls **in total** will be made to `add` and `count`.
- At least **one** call will be made to `count`.

```cpp
class CountIntervals {
public:
    map<int,int> mp;//left, right
    int cnt;
    
    CountIntervals() {
        cnt=0;
    }
    
    //KKK AAA BBB CCC DDD
    //.    NNNNNNNN
    void add(int left, int right) {
        //find A
        auto iter1=mp.lower_bound(left);//B
        int leftBound=left;
        if(iter1!=mp.begin() && prev(iter1)->second>=left){
            iter1=prev(iter1);//A
            leftBound=iter1->first;
        }
        //find C
        auto iter2=mp.upper_bound(right);//D
        int rightBound=right;
        if(iter2!=mp.begin() && prev(iter2)->second>right){
            rightBound=prev(iter2)->second;
        }
        
        //rm A B C
        for(auto iter=iter1;iter!=iter2;){
            cnt-=iter->second-iter->first+1;
            iter=mp.erase(iter);
        }
        
        //add N
        mp[leftBound]=rightBound;
        cnt+=rightBound-leftBound+1;
    }
    
    int count() {
        //for loop to calcu for each interval
        return cnt;
    }
};

/**
 * Your CountIntervals object will be instantiated and called as such:
 * CountIntervals* obj = new CountIntervals();
 * obj->add(left,right);
 * int param_2 = obj->count();
 */
```