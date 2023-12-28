# Find Median from Data Stream

#: 295
Difficult: Hard
Tags: Design, PQ, Tree
link: https://leetcode.com/problems/find-median-from-data-stream/
Priority: Medium
Created time: August 15, 2022 2:40 PM
Last edited time: October 21, 2023 1:24 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初Hash&Heap, 算高Heap&Stack

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

- For example, for `arr = [2,3,4]`, the median is `3`.
- For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

- `MedianFinder()` initializes the `MedianFinder` object.
- `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
- `double findMedian()` returns the median of all elements so far. Answers within `105` of the actual answer will be accepted.

**Example 1:**

```
Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0

```

**Constraints:**

- `105 <= num <= 105`
- There will be at least one element in the data structure before calling `findMedian`.
- At most `5 * 104` calls will be made to `addNum` and `findMedian`.

**Follow up:**

- If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
- If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?

maintain max-heap for small part and min-heap for large part

```cpp
class MedianFinder {
public:
    priority_queue<int,vector<int>,less<>> l;
    priority_queue<int,vector<int>,greater<>> r;

    MedianFinder() {  }
    
    void addNum(int num) {
        if(l.size()<r.size()+1) l.push(num);
        else r.push(num);
        //swap top
        if(l.size()>0 && r.size()>0){
            int a=l.top(); l.pop();
            int b=r.top(); r.pop();
            l.push(min(a,b));
            r.push(max(a,b));
        }
    }
    
    double findMedian() {
        if((l.size()+r.size())%2==0) return (l.top()+r.top())*0.5;
        else return l.top();
    }
};

/*
    l          r
------->    <--------
s.     l.    s.     l
pq           pq(greater<>)
*/

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```