# Anti-Diagonals of a Square Matrix

## Problem Statement

Given an `n x n` square matrix `mat[][]`, return all the elements of its anti-diagonals from top to bottom.

An anti-diagonal contains all elements having the same:

```text
row + column
```

---

## Example

Input:

```text
mat =
[
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```

Anti-diagonals:

```text
1
2 4
3 5 7
6 8
9
```

Output:

```text
[1, 2, 4, 3, 5, 7, 6, 8, 9]
```

---

## Key Observation

All elements belonging to the same anti-diagonal satisfy:

```text
row + col = constant
```

For the matrix:

```text
1 2 3
4 5 6
7 8 9
```

The positions are:

```text
(0,0) -> 0
(0,1), (1,0) -> 1
(0,2), (1,1), (2,0) -> 2
(1,2), (2,1) -> 3
(2,2) -> 4
```

So we loop through every possible value of:

```text
sum = row + col
```

---

## Approach

1. Loop `sum` from `0` to `2 * (n - 1)`
2. For each row:

   * Compute:

```text
col = sum - row
```

3. If `col` is inside the matrix, include `mat[row][col]` in the answer.

---

## Why `2 * (n - 1)`?

In an `n x n` matrix:

* minimum possible value of `row + col` is:

```text
0 + 0 = 0
```

* maximum possible value is:

```text
(n - 1) + (n - 1) = 2 * (n - 1)
```

So we must process all sums from:

```text
0 to 2 * (n - 1)
```

---

## Dry Run

For:

```text
mat =
[
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```

### sum = 0

```text
(0,0) -> 1
```

Answer:

```text
[1]
```

### sum = 1

```text
(0,1) -> 2
(1,0) -> 4
```

Answer:

```text
[1, 2, 4]
```

### sum = 2

```text
(0,2) -> 3
(1,1) -> 5
(2,0) -> 7
```

Answer:

```text
[1, 2, 4, 3, 5, 7]
```

### sum = 3

```text
(1,2) -> 6
(2,1) -> 8
```

Answer:

```text
[1, 2, 4, 3, 5, 7, 6, 8]
```

### sum = 4

```text
(2,2) -> 9
```

Final Answer:

```text
[1, 2, 4, 3, 5, 7, 6, 8, 9]
```

---

## C++ Code

```cpp
class Solution {
public:
    vector<int> antiDiagonal(vector<vector<int>>& mat) {
        int n = mat.size();
        vector<int> ans;

        for (int sum = 0; sum <= 2 * (n - 1); sum++) {
            for (int row = 0; row < n; row++) {
                int col = sum - row;

                if (col >= 0 && col < n) {
                    ans.push_back(mat[row][col]);
                }
            }
        }

        return ans;
    }
};
```

---

## Time Complexity

```text
O(n^2)
```

Because every matrix element is visited exactly once.

---

## Space Complexity

```text
O(1)
```

Ignoring the output array.

---

## Difficulty Estimate

```text
Easy
```

Approximate rating:

```text
800 - 1000
```

This is mainly a matrix traversal / observation problem.
