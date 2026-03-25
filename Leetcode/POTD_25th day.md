# 🧩 POTD — Equal Grid Partition (Horizontal / Vertical Cut)

## 📌 Problem Statement

You are given an `m x n` matrix **grid** consisting of **positive integers**.

Your task is to determine whether it is possible to make **exactly ONE straight cut**:

* either **horizontal** (between two rows)
* or **vertical** (between two columns)

Such that:

✅ Both resulting sections are **non-empty**
✅ The **sum of elements** in both sections is **equal**

Return **`true`** if such a cut exists, otherwise return **`false`**.

---

## 📊 Example

### ✅ Example 1

```
grid = [
 [1,4],
 [2,3]
]
```

Total Sum = `10`

Horizontal Cut:

Top → `1 + 4 = 5`
Bottom → `2 + 3 = 5`

✅ Equal → **Answer = true**

---

### ❌ Example 2

```
grid = [
 [1,3],
 [2,4]
]
```

Total Sum = `10`

Horizontal:

Top = `4`
Bottom = `6` ❌

Vertical:

Left = `3`
Right = `7` ❌

No valid cut → **Answer = false**

---

## 💡 Key Observations

### ⭐ Observation 1 — Equal Partition Rule

If total sum is **odd**, equal partition is impossible.

```
if(total % 2 != 0) → return false
```

Because both parts must have:

```
target = total / 2
```

---

### ⭐ Observation 2 — Horizontal Cut Logic

We try cutting **after each row**.

Maintain a running prefix:

```
runningRowSum += rowSum[i]
```

If at any point:

```
runningRowSum == target
```

Then a valid **horizontal cut exists.**

---

### ⭐ Observation 3 — Vertical Cut Logic

Similarly, try cutting **after each column**.

```
runningColSum += colSum[j]
```

If:

```
runningColSum == target
```

Then valid **vertical cut exists.**

---

## 🚀 Algorithm

1. Compute total sum of grid
2. If total is odd → return false
3. Compute `rowSum[]` and `colSum[]`
4. Try all horizontal prefix splits
5. Try all vertical prefix splits
6. If any matches → return true

---

## ⏱️ Time Complexity

```
O(m * n)
```

Only a few passes over the matrix.

---

## 🧑‍💻 C++ Implementation

```cpp
class Solution {
public:
    bool canPartitionGrid(vector<vector<int>>& grid) {
        
        int n = grid.size();      // rows
        int m = grid[0].size();   // cols

        long long sum = 0;

        vector<long long> rowSum(n,0);
        vector<long long> colSum(m,0);

        // compute sums
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                sum += grid[i][j];
                rowSum[i] += grid[i][j];
                colSum[j] += grid[i][j];
            }
        }

        if(sum % 2) return false;

        long long target = sum / 2;

        // horizontal cut
        long long hrSum = 0;
        for(int i=0;i<n-1;i++){
            hrSum += rowSum[i];
            if(hrSum == target) return true;
        }

        // vertical cut
        long long vrSum = 0;
        for(int j=0;j<m-1;j++){
            vrSum += colSum[j];
            if(vrSum == target) return true;
        }

        return false;
    }
};
```

---

## 🧠 Interview Takeaways

✅ Equal partition → always think **total /**
