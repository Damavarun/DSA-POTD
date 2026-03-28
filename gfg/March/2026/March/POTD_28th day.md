# 🔥 GFG POTD — Articulation Point - II

## 📌 Problem Summary

You are given an undirected graph with `V` vertices and `E` edges.

You must return all articulation points (cut vertices).

> An articulation point is a vertex whose removal increases the number of connected components in the graph.

If none exist → return `{-1}`

---

# 🧠 What is an Articulation Point?

Suppose graph is:

```text
0 — 1 — 2
    |
    3
```

If we remove node `1`:

```text
0    2    3
```

Now graph becomes disconnected.

So:

```text
1 is an articulation point
```

---

# 💡 Key Idea — Tarjan’s Algorithm

We use DFS and maintain two arrays:

```text
tin[node]  = time when node is first visited
low[node]  = earliest visited node reachable from node
```

---

# ⭐ Meaning of `low[node]`

`low[node]` tells:

> How far upward in DFS tree this node (or its subtree) can reach using back edges.

---

# 📊 DFS Tree Example

```text
0
|
1
|
2
```

Suppose during DFS:

```text
tin[0] = 1
tin[1] = 2
tin[2] = 3
```

If `2` cannot go back to any ancestor of `1`, then:

```text
low[2] >= tin[1]
```

That means removing `1` disconnects `2`.

So:

```text
1 is articulation point
```

---

# 🚨 Main Condition

For a non-root node:

```cpp
if(low[child] >= tin[node])
    node is articulation point
```

Why?

Because child subtree cannot connect to any ancestor of node.

So removing node disconnects that subtree.

---

# 🌳 Special Case for Root Node

Root behaves differently.

Root is articulation point only if:

```cpp
it has more than 1 DFS child
```

Because removing root splits those DFS branches.

```cpp
if(parent == -1 && child > 1)
```

---

# 🧑‍💻 Your Code Logic

---

## Step 1 — Build Adjacency List

```cpp
for(auto it : edges){
    int u = it[0], v = it[1];
    adj[u].push_back(v);
    adj[v].push_back(u);
}
```

---

## Step 2 — DFS Traversal

During DFS:

```cpp
low[node] = tin[node] = timer++;
```

---

## Step 3 — Explore Child

```cpp
dfs(it,node,...)
```

After DFS returns:

```cpp
low[node] = min(low[node], low[it]);
```

---

## Step 4 — Check Articulation Point

```cpp
if(low[it] >= tin[node] && parent != -1)
    mark[node] = 1;
```

---

## Step 5 — Handle Back Edge

```cpp
else {
    low[node] = min(low[node], tin[it]);
}
```

Because we can reach an earlier ancestor.

---

# 🔥 Dry Run Example

Graph:

```text
0 — 1 — 4
    |
    2 — 3
```

DFS order:

```text
tin[0]=1
tin[1]=2
tin[2]=3
tin[3]=4
tin[4]=5
```

Suppose subtree of `4` cannot reach above `1`.

Then:

```text
low[4] >= tin[1]
```

So `1` is articulation point.

---

# ⏱️ Complexity

```text
Time  : O(V + E)
Space : O(V + E)
```

Because DFS visits every node and edge once.

---

# 🧑‍💻 Code

```cpp
class Solution {
public:
    int timer = 1;

    void dfs(int node, int parent, vector<int> adj[],
             vector<int>& vis, int low[], int tin[],
             vector<int>& mark) {

        vis[node] = 1;
        low[node] = tin[node] = timer++;
        int child = 0;

        for(auto it : adj[node]) {

            if(it == parent) continue;

            if(!vis[it]) {

                dfs(it, node, adj, vis, low, tin, mark);

                low[node] = min(low[node], low[it]);

                // Non-root articulation condition
                if(low[it] >= tin[node] && parent != -1) {
                    mark[node] = 1;
                }

                child++;
            }
            else {
                // Back edge
                low[node] = min(low[node], tin[it]);
            }
        }

        // Root articulation condition
        if(parent == -1 && child > 1) {
            mark[node] = 1;
        }
    }

    vector<int> articulationPoints(int V, vector<vector<int>>& edges) {

        vector<int> adj[V];

        for(auto it : edges) {
            int u = it[0], v = it[1];
            adj[u].push_back(v);
            adj[v].push_back(u);
        }

        vector<int> vis(V, 0);
        int low[V], tin[V];
        vector<int> mark(V, 0);

        for(int i = 0; i < V; i++) {
            if(!vis[i]) {
                dfs(i, -1, adj, vis, low, tin, mark);
            }
        }

        vector<int> ans;

        for(int i = 0; i < V; i++) {
            if(mark[i]) ans.push_back(i);
        }

        if(ans.empty()) return {-1};

        return ans;
    }
};
```
```cpp
low[child] >= tin[node]
```
