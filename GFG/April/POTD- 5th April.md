
# POTD - Target Sum Expressions

> Problem of the Day solution using Recursion + Memoization

# README

## Target Sum - Memoization Solution

### Problem Statement

Given an integer array `arr[]` and an integer `target`, place either a `'+'` or `'-'` sign before every number in the array.

Return the number of different expressions that evaluate to `target`.

Example:

```text
arr = [1, 1, 1, 1, 1]
target = 3
```

Possible expressions:

```text
-1 +1 +1 +1 +1 = 3
+1 -1 +1 +1 +1 = 3
+1 +1 -1 +1 +1 = 3
+1 +1 +1 -1 +1 = 3
+1 +1 +1 +1 -1 = 3
```

Answer:

```text
5
```

---

## Memoization Approach

For every index, we have two choices:

1. Add the current number
2. Subtract the current number

So we recursively try both possibilities.

Since many states repeat, we store results in a DP table.

A state is represented by:

```text
(index, currentSum)
```

---

## C++ Code

```cpp
class Solution {
  public:
    int solve(int idx, int sum, vector<int>& arr, int target,
              vector<vector<int>>& dp, int offset) {

        if (idx == arr.size()) {
            return (sum == target);
        }

        if (dp[idx][sum + offset] != -1) {
            return dp[idx][sum + offset];
        }

        int plus = solve(idx + 1, sum + arr[idx], arr, target, dp, offset);
        int minus = solve(idx + 1, sum - arr[idx], arr, target, dp, offset);

        return dp[idx][sum + offset] = plus + minus;
    }

    int totalWays(vector<int>& arr, int target) {
        int total = 0;
        for (int num : arr) total += num;

        int offset = total;

        vector<vector<int>> dp(arr.size(), vector<int>(2 * total + 1, -1));

        return solve(0, 0, arr, target, dp, offset);
    }
};
```

---

## Why Offset is Needed

`sum` can become negative.

For example:

```text
sum = -3
```

Array indices cannot be negative, so we shift by `offset`:

```text
index = sum + offset
```

If:

```text
offset = totalSum = 5
sum = -3
```

Then:

```text
index = -3 + 5 = 2
```

---

## Dry Run

Input:

```text
arr = [1, 2, 3]
target = 2
```

Recursive choices:

```text
+1 +2 +3 = 6
+1 +2 -3 = 0
+1 -2 +3 = 2   <- valid
+1 -2 -3 = -4
-1 +2 +3 = 4
-1 +2 -3 = -2
-1 -2 +3 = 0
-1 -2 -3 = -6
```

Only one expression gives target `2`.

Output:

```text
1
```

---

## Time Complexity

```text
O(n * totalSum)
```

Where:

* `n` = size of array
* `totalSum` = sum of all elements

Maximum states:

```text
50 * 2001
```

---

## Space Complexity

```text
O(n * totalSum)
```

---

## Constraints

```text
1 <= arr.size() <= 50
1 <= arr[i] <= 20
sum(arr) <= 1000
-1000 <= target <= 1000
```
