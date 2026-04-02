# Painting the Fence - Memoization README

## Problem Statement

Given `n` fence posts and `k` colors, count the number of ways to paint the fence such that not more than two consecutive posts have the same color.

---

## Example

```text
Input:
n = 3, k = 2

Output:
6
```

Valid colorings:

```text
RRB
RBR
RBB
BRR
BRB
BBR
```

---

## DP Idea

We use recursion + memoization.

Define:

```text
solve(idx, same)
```

Where:

* `idx` = number of posts left to paint
* `same = 1` means the last two painted posts have the same color
* `same = 0` means the last two painted posts have different colors

---

## Transition

If the previous two posts are same:

```text
f(idx, 1) = f(idx - 1, 0) * (k - 1)
```

Because the next post must be different.

If the previous two posts are different:

```text
f(idx, 0) = f(idx - 1, 1) + f(idx - 1, 0) * (k - 1)
```

Because:

* We can paint current post same as previous
* Or choose one of the remaining `k - 1` colors

---

## Base Case

```text
If idx == 1, return k
```

Because the last remaining post can be painted in any of the `k` colors.

---

## C++ Code

```cpp
class Solution {
public:
    int solve(int idx, int same, int k, vector<vector<int>>& dp) {
        if (idx == 1) return k;

        if (dp[idx][same] != -1)
            return dp[idx][same];

        long long ans = 0;

        if (same) {
            ans = 1LL * solve(idx - 1, 0, k, dp) * (k - 1);
        } else {
            ans = solve(idx - 1, 1, k, dp)
                + 1LL * solve(idx - 1, 0, k, dp) * (k - 1);
        }

        return dp[idx][same] = ans;
    }

    int countWays(int n, int k) {
        vector<vector<int>> dp(n + 1, vector<int>(2, -1));
        return solve(n, 0, k, dp);
    }
};
```

---

## Complexity

```text
Time Complexity: O(n)
Space Complexity: O(n)
```

There are only `n * 2` states and each state is computed once.
