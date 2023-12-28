# Design SQL

#: 2408
Difficult: Medium
Tags: Array, Design, Hash, string
link: https://leetcode.com/problems/design-sql/description
Priority: Low
Created time: November 15, 2023 3:24 PM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq

You are given `n` tables represented with two arrays `names` and `columns`, where `names[i]` is the name of the `ith` table and `columns[i]` is the number of columns of the `ith` table.

You should be able to perform the following **operations**:

- **Insert** a row in a specific table. Each row you insert has an id. The id is assigned using an auto-increment method where the id of the first inserted row is 1, and the id of each other row inserted into the same table is the id of the last inserted row (even if it was deleted) plus one.
- **Delete** a row from a specific table. **Note** that deleting a row does not affect the id of the next inserted row.
- **Select** a specific cell from any table and return its value.

Implement the `SQL` class:

- `SQL(String[] names, int[] columns)` Creates the `n` tables.
- `void insertRow(String name, String[] row)` Adds a row to the table `name`. It is **guaranteed** that the table will exist, and the size of the array `row` is equal to the number of columns in the table.
- `void deleteRow(String name, int rowId)` Removes the row `rowId` from the table `name`. It is **guaranteed** that the table and row will **exist**.
- `String selectCell(String name, int rowId, int columnId)` Returns the value of the cell in the row `rowId` and the column `columnId` from the table `name`.

**Example 1:**

```
Input
["SQL", "insertRow", "selectCell", "insertRow", "deleteRow", "selectCell"]
[[["one", "two", "three"], [2, 3, 1]], ["two", ["first", "second", "third"]], ["two", 1, 3], ["two", ["fourth", "fifth", "sixth"]], ["two", 1], ["two", 2, 2]]
Output
[null, null, "third", null, null, "fifth"]

Explanation
SQL sql = new SQL(["one", "two", "three"], [2, 3, 1]); // creates three tables.
sql.insertRow("two", ["first", "second", "third"]); // adds a row to the table "two". Its id is 1.
sql.selectCell("two", 1, 3); // return "third", finds the value of the third column in the row with id 1 of the table "two".
sql.insertRow("two", ["fourth", "fifth", "sixth"]); // adds another row to the table "two". Its id is 2.
sql.deleteRow("two", 1); // deletes the first row of the table "two". Note that the second row will still have the id 2.
sql.selectCell("two", 2, 2); // return "fifth", finds the value of the second column in the row with id 2 of the table "two".

```

**Constraints:**

- `n == names.length == columns.length`
- `1 <= n <= 104`
- `1 <= names[i].length, row[i].length, name.length <= 20`
- `names[i]`, `row[i]`, and `name` consist of lowercase English letters.
- `1 <= columns[i] <= 100`
- All the strings of `names` are **distinct**.
- `name` exists in the array `names`.
- `row.length` equals the number of columns in the chosen table.
- `rowId` and `columnId` will be valid.
- At most `250` calls will be made to `insertRow` and `deleteRow`.
- At most `104` calls will be made to `selectCell`.

comment: why not use map instead of set

```cpp
using p=pair<int,vector<string>>;

class SQL {
public:
    int n=0;//1~1e4
    //todo, use vector instead of set
    unordered_map<string, set<pair<int,vector<string>>>> tables;
    //name: name of ith table, 1~20, distinct
    //column: column num of ith table, 1~100
    SQL(vector<string>& names, vector<int>& columns) {
        n=names.size();
        tables={};
        //name -> tableId -> colomns : mp[name]=table{columnNum,}, seems not important
        //mp[name] = {(id,row)}
        //unordered_map<name, set<pair<id,row>>>
    }
    
    //name: 1~20, exist in names - update only
    //row: 1~20, row.len==numOfCol
    //250 calls
    void insertRow(string name, vector<string> row) {
        //first row id: 1, else inc, even if pre is deleted
        int id = tables[name].empty()? 1: tables[name].rbegin()->first+1;
        tables[name].insert({id, row});//only place set slower than vector?
    }
    
    //250 calls
    void deleteRow(string name, int rowId) {
        //return tables[name].lower_bound({rowId,{}})->second.clear();
        //why do we need delete operation? since all select calls are valid
    }
    
    //1e4 calls
    string selectCell(string name, int rowId, int columnId) {
        return tables[name].lower_bound({rowId,{}})->second[columnId-1];
    }

    //lower case only - 26
    //row id, col id: valid
};

/**
 * Your SQL object will be instantiated and called as such:
 * SQL* obj = new SQL(names, columns);
 * obj->insertRow(name,row);
 * obj->deleteRow(name,rowId);
 * string param_3 = obj->selectCell(name,rowId,columnId);
 */
```