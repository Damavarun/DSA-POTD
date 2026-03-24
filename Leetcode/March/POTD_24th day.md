# 📅 Problem Of The Day — Construct Product Matrix

## 🧩 Problem Statement

Given a **0-indexed 2D integer matrix `grid` of size `n × m`**, construct another matrix `p` of the same size such that:

```
p[i][j] = product of all elements of grid except grid[i][j]
```

The result must be taken **modulo 12345**.

---

## 🧠 Intuition

This problem is an extension of the famous:

> **Product of Array Except Self**

Since division is not safe due to modulo constraints and possible large values, we use the **Prefix Product and Suffix Product technique.**

We first **flatten the matrix into a 1D array**, compute prefix and suffix products, then map the results back to a 2D matrix.

---

## 🚀 Approach

### Step 1

Flatten the matrix into a 1D array.

### Step 2

Compute **prefix product** where:

```
prefix[i] = product of all elements before index i
```

### Step 3

Compute **suffix product** where:

```
suffix[i] = product of all elements after index i
```

### Step 4

Final answer for index `i`:

```
answer[i] = prefix[i] × suffix[i] % 12345
```

### Step 5

Convert the 1D answer back into 2D using:

```
row = i / m  
col = i % m
```

---

## ⏱ Time Complexity

```
O(n × m)
```

Each element is processed a constant number of times.

---

## 📦 Space Complexity

```
O(n × m)
```

For flattened array and prefix/suffix storage.

---

## ✅ C++ Solution

```cpp
class Solution {
public:
    vector<vector<int>> constructProductMatrix(vector<vector<int>>& grid) {

        int n = grid.size();
        int m = grid[0].size();
        int N = n * m;
        const int MOD = 12345;

        vector<long long> arr(N);
        
        int k = 0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                arr[k++] = grid[i][j] % MOD;
            }
        }

        vector<long long> pref(N,1), suff(N,1);

        for(int i=1;i<N;i++){
            pref[i] = (pref[i-1] * arr[i-1]) % MOD;
        }

        for(int i=N-2;i>=0;i--){
            suff[i] = (suff[i+1] * arr[i+1]) % MOD;
        }

        vector<vector<int>> ans(n, vector<int>(m));

        for(int i=0;i<N;i++){
            ans[i/m][i%m] = (pref[i] * suff[i]) % MOD;
        }

        return ans;
    }
};
```

---

## 🔥 Key Learnings

* Prefix–Suffix Product Technique
* Matrix Flattening (Row-major indexing)
* Modulo Arithmetic in Product Problems
* Avoiding Division in Competitive Programming

---
