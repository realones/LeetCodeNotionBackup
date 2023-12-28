# Employee Importance

#: 690
Difficult: Medium
Tags: BFS, DFS, Hash, Tree
link: https://leetcode.com/problems/employee-importance/
Priority: Pass
Created time: June 27, 2023 11:51 PM
Last edited time: December 24, 2023 5:19 PM
source: HuaHua, googleFreq

You have a data structure of employee information, including the employee's unique ID, importance value, and direct subordinates' IDs.

You are given an array of employees `employees` where:

- `employees[i].id` is the ID of the `ith` employee.
- `employees[i].importance` is the importance value of the `ith` employee.
- `employees[i].subordinates` is a list of the IDs of the direct subordinates of the `ith` employee.

Given an integer `id` that represents an employee's ID, return *the **total** importance value of this employee and all their direct and indirect subordinates*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/05/31/emp1-tree.jpg](https://assets.leetcode.com/uploads/2021/05/31/emp1-tree.jpg)

```
Input: employees = [[1,5,[2,3]],[2,3,[]],[3,3,[]]], id = 1
Output: 11
Explanation: Employee 1 has an importance value of 5 and has two direct subordinates: employee 2 and employee 3.
They both have an importance value of 3.
Thus, the total importance value of employee 1 is 5 + 3 + 3 = 11.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/05/31/emp2-tree.jpg](https://assets.leetcode.com/uploads/2021/05/31/emp2-tree.jpg)

```
Input: employees = [[1,2,[5]],[5,-3,[]]], id = 5
Output: -3
Explanation: Employee 5 has an importance value of -3 and has no direct subordinates.
Thus, the total importance value of employee 5 is -3.

```

**Constraints:**

- `1 <= employees.length <= 2000`
- `1 <= employees[i].id <= 2000`
- All `employees[i].id` are **unique**.
- `100 <= employees[i].importance <= 100`
- One employee has at most one direct leader and may have several subordinates.
- The IDs in `employees[i].subordinates` are valid IDs.

```cpp
// cp from https://leetcode.com/problems/employee-importance/solutions/112605/c-12-lines-bfs-and-7-lines-dfs/ to pass

/*
// Definition for Employee.
class Employee {
public:
    int id;
    int importance;
    vector<int> subordinates;
};
*/

class Solution {
public:
    int getImportance(vector<Employee*> employees, int id) {
        unordered_map<int, Employee*>m;
        for(auto x: employees) m[x->id] = x;
        int sum = 0;
        DFS(m, id, sum);
        return sum;
    }
    
    void DFS(unordered_map<int, Employee*>& m, int id, int& sum){
        sum += m[id]->importance;
        for(auto x: m[id]->subordinates) DFS(m, x, sum);
    }
};

/*
find employee with id 
dfs/recur to accummulate the result
*/
```