# 🧩 LeetCode 2573 — Find the String with LCP

## 📌 Problem Summary

You are given an `n x n` matrix `lcp`.

`lcp[i][j]` represents:

> Length of the Longest Common Prefix between:

```text
word[i ... n-1]
and
word[j ... n-1]
```

You must construct:

* A valid string `word`
* Such that its LCP matrix exactly matches the given matrix
* Among all valid strings, return the lexicographically smallest one

If impossible → return `""`

---

# 🧠 What Does LCP Mean?

Example:

```text
word = "abab"
```

Suffixes:

```text
0 → "abab"
1 → "bab"
2 → "ab"
3 → "b"
```

Now:

```text
lcp[0][2]
```

Compare:

```text
"abab"
"ab"
```

Longest common prefix = `"ab"` → length = 2

So:

```text
lcp[0][2] = 2
```

---

# 💡 Main Idea

We build the answer string greedily.

If:

```text
lcp[i][j] > 0
```

That means:

```text
word[i] == word[j]
```

Because if two suffixes share even 1 common character at the start, their first characters must be same.

So we:

* Start from left to right
* Assign smallest unused character (`'a'`, `'b'`, ...)
* If `lcp[i][j] > 0`, assign same character to `j`

This guarantees the lexicographically smallest valid string.

---

# 🔥 Step 1 — Construct Candidate String

Example:

```text
lcp =
[
 [4,0,2,0],
 [0,3,0,1],
 [2,0,2,0],
 [0,1,0,1]
]
```

Start:

```text
word[0] = 'a'
```

Since:

```text
lcp[0][2] > 0
```

So:

```text
word[2] = 'a'
```

Next unassigned position = 1

```text
word[1] = 'b'
```

Since:

```text
lcp[1][3] > 0
```

So:

```text
word[3] = 'b'
```

Final:

```text
"abab"
```

---

# 🚨 Why Construction Alone Is Not Enough

Sometimes we may create a string, but it still does not match the matrix.

So we must validate every pair `(i, j)`.

---

# ✅ Validation Rules

---

## Rule 1 — If Characters Differ

If:

```text
word[i] != word[j]
```

Then longest common prefix must be 0

So:

```cpp
if(word[i] != word[j] && lcp[i][j] != 0)
    return "";
```

---

## Rule 2 — If Characters Are Same

Then:

```text
lcp[i][j] = 1 + lcp[i+1][j+1]
```

Because first characters match, so answer extends one more character.

Example:

```text
word = "abab"

lcp[0][2]
= 1 + lcp[1][3]
= 1 + 1
= 2
```

---

## Special Case — Last Row / Last Column

If either index is the last position:

```text
i == n-1 OR j == n-1
```

Then only one character remains.

So if characters are same:

```text
lcp[i][j] must be 1
```

Otherwise invalid.

---

# ⏱️ Complexity

Construction:

```text
O(n²)
```

Validation:

```text
O(n²)
```

Overall:

```text
O(n²)
```

Works because:

```text
n ≤ 1000
```

---

# 🧑‍💻 Code

```cpp
class Solution {
public:
    string findTheString(vector<vector<int>>& lcp) {
        int n = lcp.size();
        string word(n, '\0');
        char current = 'a';

        // Step 1: Build smallest lexicographical string
        for (int i = 0; i < n; i++) {
            if (word[i] == '\0') {

                if (current > 'z')
                    return "";

                word[i] = current;

                for (int j = i + 1; j < n; j++) {
                    if (lcp[i][j] > 0) {
                        word[j] = current;
                    }
                }

                current++;
            }
        }

        // Step 2: Validate the generated string
        for (int i = n - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {

                if (word[i] != word[j]) {
                    if (lcp[i][j] != 0)
                        return "";
                }
                else {
                    if (i == n - 1 || j == n - 1) {
                        if (lcp[i][j] != 1)
                            return "";
                    }
                    else {
                        if (lcp[i][j] != lcp[i + 1][j + 1] + 1)
                            return "";
                    }
                }
            }
        }

        return word;
    }
};
```

