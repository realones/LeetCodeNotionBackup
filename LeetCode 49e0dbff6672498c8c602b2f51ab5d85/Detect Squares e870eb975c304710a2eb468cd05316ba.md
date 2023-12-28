# Detect Squares

#: 2013
Difficult: Medium
Tags: Hash
link: https://leetcode.com/problems/detect-squares/
Priority: Low
Created time: October 24, 2022 12:52 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

ou are given a stream of points on the X-Y plane. Design an algorithm that:

- **Adds** new points from the stream into a data structure. **Duplicate** points are allowed and should be treated as different points.
- Given a query point, **counts** the number of ways to choose three points from the data structure such that the three points and the query point form an **axis-aligned square** with **positive area**.

An **axis-aligned square** is a square whose edges are all the same length and are either parallel or perpendicular to the x-axis and y-axis.

Implement the `DetectSquares` class:

- `DetectSquares()` Initializes the object with an empty data structure.
- `void add(int[] point)` Adds a new point `point = [x, y]` to the data structure.
- `int count(int[] point)` Counts the number of ways to form **axis-aligned squares** with point `point = [x, y]` as described above.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/09/01/image.png](https://assets.leetcode.com/uploads/2021/09/01/image.png)

```
Input
["DetectSquares", "add", "add", "add", "count", "count", "add", "count"]
[[], [[3, 10]], [[11, 2]], [[3, 2]], [[11, 10]], [[14, 8]], [[11, 2]], [[11, 10]]]
Output
[null, null, null, null, 1, 0, null, 2]

Explanation
DetectSquares detectSquares = new DetectSquares();
detectSquares.add([3, 10]);
detectSquares.add([11, 2]);
detectSquares.add([3, 2]);
detectSquares.count([11, 10]); // return 1. You can choose:
                               //   - The first, second, and third points
detectSquares.count([14, 8]);  // return 0. The query point cannot form a square with any points in the data structure.
detectSquares.add([11, 2]);    // Adding duplicate points is allowed.
detectSquares.count([11, 10]); // return 2. You can choose:
                               //   - The first, second, and third points
                               //   - The first, third, and fourth points

```

**Constraints:**

- `point.length == 2`
- `0 <= x, y <= 1000`
- At most `3000` calls **in total** will be made to `add` and `count`.

```cpp
class DetectSquares {
public:
    //can imprveo with vec[1000], and change set to mp y-cnt
    unordered_map<int,unordered_map<int,int>> x_ys; //x - y-cnt
    unordered_map<int,unordered_map<int,int>> y_xs; //y - x-cnt
    
    DetectSquares() {
        
    }
    
    void add(vector<int> point) {
        int x=point[0], y=point[1];
        x_ys[x][y]++;
        y_xs[y][x]++;
    }
    
    int count(vector<int> point) {
        int res=0;
        int px=point[0], py=point[1];
        //for same x, each y from x-ys
        //  get len
        //. check +- x'
        for(auto& [y, ycnt]: x_ys[px]){
            int len=abs(y-py); if(len==0) continue;
            int x1=px-len, x2=px+len;
            
            int x1yCnt=x_ys[x1][y];
            int x1pyCnt=x_ys[x1][py];
            
            res+=x1yCnt*x1pyCnt*ycnt;
                
            int x2yCnt=x_ys[x2][y];
            int x2pyCnt=x_ys[x2][py];
            
            res+=x2yCnt*x2pyCnt*ycnt;
        }
        return res;
    }
};

/**
 * Your DetectSquares object will be instantiated and called as such:
 * DetectSquares* obj = new DetectSquares();
 * obj->add(point);
 * int param_2 = obj->count(point);
 */
```