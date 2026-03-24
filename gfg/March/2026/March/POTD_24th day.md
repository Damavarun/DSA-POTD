# 📅 Problem Of The Day — Course Schedule I

## 🧩 Problem Statement

You are given **n courses** labeled from `0` to `n-1` and a list of prerequisite pairs `prerequisites[i] = [x, y]` which means **course y must be completed before course x.**

Your task is to determine whether it is possible to complete **all courses.**

Return:

* `true` → if all courses can be completed
* `false` → if it is impossible

---

## 🧠 Intuition

This problem can be modeled as a **Directed Graph Cycle Detection** problem.

* Each course represents a **node**
* Each prerequisite `[x, y]` represents a **directed edge** from `y → x`
* If the graph contains a **cycle**, it means there is a circular dependency → courses cannot be completed.

---

## 🚀 Approach — Topological Sort (Kahn’s Algorithm)

We use **BFS-based Topological Sorting** to detect cycles.

### Steps:

1. Build an adjacency list from prerequisites.
2. Compute **indegree** of each node.
3. Push all nodes with **indegree = 0** into a queue.
4. Perform BFS and reduce indegree of neighbouring nodes.
5. Count how many nodes are processed.

If processed nodes = `n` → No cycle → All courses can be completed.
Otherwise → Cycle exists → Not possible.

---

## ⏱ Time Complexity

```
O(V + E)
```

* `V` → number of courses
* `E` → number of prerequisite relations

---

## 📦 Space Complexity

```
O(V + E)
```

For adjacency list and queue.

---

## ✅ C++ Solution

```cpp
class Solution {
public:
    bool canFinish(int V, vector<vector<int>>& prerequisites) {

        vector<vector<int>> adj(V);
        vector<int> indegree(V, 0);

        // build graph
        for(auto &it : prerequisites){
            int x = it[0];
            int y = it[1];

            adj[y].push_back(x);
            indegree[x]++;
        }

        queue<int> q;

        for(int i = 0; i < V; i++){
            if(indegree[i] == 0)
                q.push(i);
        }

        int cnt = 0;

        while(!q.empty()){
            int node = q.front();
            q.pop();

            cnt++;

            for(auto it : adj[node]){
                indegree[it]--;
                if(indegree[it] == 0)
                    q.push(it);
            }
        }

        return cnt == V;
    }
};
```

---

## 🔥 Key Learning

* Topological Sort helps in **detecting cycles in Directed Graphs.**
* If topo ordering exists → scheduling is possible.
* This pattern is widely used in:

  * Course Scheduling
  * Task Dependency Problems
  * Build Systems
  * Project Planning

---
