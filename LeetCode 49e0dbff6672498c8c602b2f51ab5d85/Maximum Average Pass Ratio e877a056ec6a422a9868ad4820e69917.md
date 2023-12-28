# Maximum Average Pass Ratio

#: 1792
Difficult: Medium
Tags: Array, Greedy, PQ
link: https://leetcode.com/problems/maximum-average-pass-ratio/
Priority: High
Status: No Idea At First
Created time: June 25, 2023 4:27 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

There is a school that has classes of students and each class will be having a final exam. You are given a 2D integer array `classes`, where `classes[i] = [passi, totali]`. You know beforehand that in the `ith` class, there are `totali` total students, but only `passi` number of students will pass the exam.

You are also given an integer `extraStudents`. There are another `extraStudents` brilliant students that are **guaranteed** to pass the exam of any class they are assigned to. You want to assign each of the `extraStudents` students to a class in a way that **maximizes** the **average** pass ratio across **all** the classes.

The **pass ratio** of a class is equal to the number of students of the class that will pass the exam divided by the total number of students of the class. The **average pass ratio** is the sum of pass ratios of all the classes divided by the number of the classes.

Return *the **maximum** possible average pass ratio after assigning the* `extraStudents` *students.* Answers within `10-5` of the actual answer will be accepted.

**Example 1:**

```
Input: classes = [[1,2],[3,5],[2,2]],extraStudents = 2
Output: 0.78333
Explanation: You can assign the two extra students to the first class. The average pass ratio will be equal to (3/4 + 3/5 + 2/2) / 3 = 0.78333.

```

**Example 2:**

```
Input: classes = [[2,4],[3,9],[4,5],[2,10]],extraStudents = 4
Output: 0.53485

```

**Constraints:**

- `1 <= classes.length <= 105`
- `classes[i].length == 2`
- `1 <= passi <= totali <= 105`
- `1 <= extraStudents <= 105`

```cpp
class Solution {
public:
    double maxAverageRatio(vector<vector<int>>& classes, int extraStudents) {
        priority_queue<tuple<double,int,int>> pq;//profit, pass, total
        for(auto &c: classes){//c: {pass,total}
            pq.emplace(profit(c[0],c[1]),c[0],c[1]);
        }

        for(int i=0; i<extraStudents; i++){
            auto [_, pass, total]=pq.top(); pq.pop();//get the most profit one
            pass++,total++;
            pq.emplace(profit(pass,total),pass,total);
        }

        double ratio_sum=0;
        while(!pq.empty()){
            auto [_, pass, total]=pq.top(); pq.pop();
            ratio_sum+=pass*1.0/total;
        }
        return ratio_sum*1.0/classes.size();
    }

    double profit(double pass, double total){
        return (pass+1)/(total+1) - pass/total;
    }
};

/*
greedy:
ave rate = (r1 + r2 + r3 +...)/cnt
since cnt is fixed and everytime we can only enhance one rate, so choose the one we can enhance most

caveat: not choosing the min(r1,r2,r3,...) to enhance, but choose the one we can enhance the most
*
```