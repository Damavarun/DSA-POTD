# 🌳 POTD — Minimum Height Trees (MHT)

## 🧩 Problem Summary

Given an **undirected tree** with `n` nodes labeled from `0` to `n-1`, return **all possible roots** such that the height of the tree is **minimum**.

A **tree** is:

* Connected
* Undirected
* Has `n - 1` edges

---

## 📏 What is Tree Height?

If we choose any node as root:

> **Height = Maximum distance from root to any other node**

Example:

```
0 — 1 — 2 — 3
```

* Root = `0` → Height = `3`
* Root = `1` → Height = `2`
* Root = `2` → Height = `2`
* Root = `3` → Height = `3`

✅ Best roots → `[1, 2]`

These nodes are called:

> ⭐ **Centers of the Tree**

---

## 💡 Core Intuition — Leaf Trimming (Reverse BFS)

Instead of trying every node as root ❌

We:

> ⭐ Remove **leaf nodes (degree = 1)** layer by layer until only center(s) remain.

This is similar in pattern to **Kahn’s Algorithm (Toposort)** but applied on an **undirected tree**.

---

## 🌿 Why Leaf Nodes?

* Leaves are always **farthest from the center**
* They can **never be optimal roots**
* Removing them moves us **towards the tree’s core**

Think like **peeling an onion 🧅**

---

## 🔄 Algorithm Steps

### ✅ Step 1 — Build Graph

* Create adjacency list
* Maintain degree array

### ✅ Step 2 — Push All Leaves

```
if degree[i] == 1 → push into queue
```

### ✅ Step 3 — Trim Layers

```
while remaining_nodes > 2:
    remove all current leaves
    reduce neighbour degree
    if neighbour becomes leaf → push
```

### ✅ Step 4 — Remaining Nodes = Answer

Tree can have:

* **1 center**
* **2 centers**

So we stop when nodes ≤ 2.

---

## 🧠 Why `while (remaining > 2)` ?

Because:

* Diameter **odd length → 1 center**
* Diameter **even length → 2 centers**

Leaf trimming **automatically finds the diameter middle**, so no need to manually check parity.

---

## ⏱️ Time Complexity

```
O(V + E)
```

Each node and edge is processed only once.

---

## 🧑‍💻 C++ Implementation

```cpp
class Solution {
public:
    vector<int> findMinHeightTrees(int V, vector<vector<int>>& edges) {
        
        if(V == 1) return {0};
        
        vector<vector<int>> adj(V);
        vector<int> degree(V,0);
        
        // Build graph
        for(auto &e : edges){
            int u = e[0];
            int v = e[1];
            
            adj[u].push_back(v);
            adj[v].push_back(u);
            
            degree[u]++;
            degree[v]++;
        }
        
        queue<int> q;
        
        // Push initial leaves
        for(int i = 0; i < V; i++){
            if(degree[i] == 1){
                q.push(i);
            }
        }
        
        int remaining = V;
        
        // Leaf trimming BFS
        while(remaining > 2){
            
            int size = q.size();
            remaining -= size;
            
            while(size--){
                
                int node = q.front();
                q.pop();
                
                for(auto it : adj[node]){
                    
                    degree[it]--;
                    
                    if(degree[it] == 1){
                        q.push(it);
                    }
                }
            }
        }
        
        vector<int> ans;
        
        while(!q.empty()){
            ans.push_back(q.front());
            q.pop();
        }
        
        return ans;
    }
};
```

---

## 🚀 Alternative Approach (Advanced)

Find tree **Diameter using 2 BFS**:

1. BFS from any node → find farthest node `A`
2. BFS from `A` → find farthest node `B`
3. Diameter path = `A → B`
4. Return **middle node(s)**

---
