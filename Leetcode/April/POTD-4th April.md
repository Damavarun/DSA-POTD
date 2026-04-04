# 📘 POTD README — LeetCode (April 4, 2026)

---

# if you know how to do this sort the matix diagonally then today's POTD is easy
#Sort the Matrix Diagonally

## 📝 Problem Statement

Given an `m x n` matrix, sort each diagonal in ascending order and return the resulting matrix.

A diagonal starts from any cell in the first row or first column and moves in the bottom-right direction.

### Example

```text
Input:
[[3,3,1,1],
 [2,2,1,2],
 [1,1,1,2]]

Output:
[[1,1,1,1],
 [1,2,2,2],
 [1,2,3,3]]
```

---

## 💡 Approach

1. Traverse all diagonals starting from:

   * First row
   * First column
2. Store elements of one diagonal in a vector.
3. Sort the vector.
4. Put the sorted values back into the matrix.

---

## ✅ C++ Code

```cpp
class Solution {
private:
    void sortDiagonal(vector<vector<int>>& mat, int row, int col) {
        int n = mat.size();
        int m = mat[0].size();

        vector<int> diagonal;

        int r = row, c = col;

        while (r < n && c < m) {
            diagonal.push_back(mat[r][c]);
            r++;
            c++;
        }

        sort(diagonal.begin(), diagonal.end());

        r = row;
        c = col;
        int idx = 0;

        while (r < n && c < m) {
            mat[r][c] = diagonal[idx++];
            r++;
            c++;
        }
    }

public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        int n = mat.size();
        int m = mat[0].size();

        for (int col = 0; col < m; col++) {
            sortDiagonal(mat, 0, col);
        }

        for (int row = 1; row < n; row++) {
            sortDiagonal(mat, row, 0);
        }

        return mat;
    }
};
```

---

## ⏱ Complexity

* Time Complexity: `O(m * n * log(min(m, n)))`
* Space Complexity: `O(min(m, n))`

---

---

#  LeetCode — Decode the Slanted Ciphertext

## 📝 Problem Statement

You are given:

* `encodedText`
* `rows`

The text was originally written diagonally in a matrix and then read row-wise.
Your task is to reconstruct the original text.

### Example

```text
Input:
encodedText = "ch   ie   pr"
rows = 3

Output:
"cipher"
```

---

## 💡 Approach

1. Compute the number of columns:

```text
cols = encodedText.length() / rows
```

2. Fill a matrix row-wise using `encodedText`.
3. Read the matrix diagonally starting from each column in the first row.
4. Remove trailing spaces.

---

## ✅ C++ Code

```cpp
class Solution {
public:
    string decodeCiphertext(string encodedText, int rows) {
        if (rows == 1) return encodedText;

        int cols = encodedText.size() / rows;

        vector<vector<char>> mat(rows, vector<char>(cols));

        int idx = 0;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                mat[i][j] = encodedText[idx++];
            }
        }

        string ans;

        for (int startCol = 0; startCol < cols; startCol++) {
            int r = 0;
            int c = startCol;

            while (r < rows && c < cols) {
                ans += mat[r][c];
                r++;
                c++;
            }
        }

        while (!ans.empty() && ans.back() == ' ') {
            ans.pop_back();
        }

        return ans;
    }
};
```

---

## ⏱ Complexity

* Time Complexity: `O(n)`
* Space Complexity: `O(n)`

---

# 🚀 Key Takeaways

* Matrix diagonal problems often require moving using:


* For diagonal sort: collect → sort → place back.
* For cipher decode: fill row-wise → read diagonally.
