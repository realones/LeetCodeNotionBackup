# [lint] Load Balancer

#: 526
Difficult: Medium
Tags: Hash, Random
link: https://www.lintcode.com/problem/526/
Priority: Medium
Status: Dig More
Created time: October 17, 2022 6:32 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

Description

Implement a load balancer for web servers. It provide the following functionality:

1. Add a new server to the cluster => `add(server_id)`.
2. Remove a bad server from the cluster => `remove(server_id)`.
3. Pick a server in the cluster randomly with equal probability => `pick()`.

At beginning, the cluster is empty. When `pick()` is called you need to randomly return a `server_id` in the cluster.

Example

**Example 1:**

```
Input:
  add(1)
  add(2)
  add(3)
  pick()
  pick()
  pick()
  pick()
  remove(1)
  pick()
  pick()
  pick()
Output:
  1
  2
  1
  3
  2
  3
  3
Explanation: The return value of pick() is random, it can be either 2 3 3 1 3 2 2 or other.

```

```cpp
class LoadBalancer {
public:
    unordered_map<int,int> mp;//val_idx -> put/delete O(1)
    vector<int> nums;//idx_val -> rand

    LoadBalancer() {
        // do intialization if necessary
    }

    /*
     * @param server_id: add a new server to the cluster
     * @return: nothing
     */
    void add(int server_id) {
        // write your code here
        if(mp.count(server_id)) return;

        nums.push_back(server_id);
        mp[server_id]=nums.size()-1;
    }

    /*
     * @param server_id: server_id remove a bad server from the cluster
     * @return: nothing
     */
    void remove(int server_id) {
        // write your code here
        if(!mp.count(server_id)) return;

        //swap to back
        int idx=mp[server_id];
        swap(nums[idx],nums[nums.size()-1]);
        mp[nums[idx]]=idx;
        // mp[server_id]=nums.size()-1;

        //rm
        mp.erase(server_id);
        nums.pop_back();
    }

    /*
     * @return: pick a server in the cluster randomly with equal probability
     */
    int pick() {
        // write your code here
        int idx=rand()%nums.size();
        return nums[idx];
    }
};
```