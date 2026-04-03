# 3661. Maximum Walls Destroyed by Robots

## Problem Summary

You are given:

* `robots[i]` → position of the `i-th` robot
* `distance[i]` → maximum bullet distance for that robot
* `walls[j]` → position of a wall

Each robot can fire exactly one bullet:

* either to the left
* or to the right

The bullet:

* destroys every wall in its path
* stops immediately if it hits another robot
* cannot pass through another robot

The goal is to maximize the number of **unique walls** destroyed.

---

## Key Observation

A wall may be reachable by multiple robots.

So we cannot simply add:

```text
leftWalls[i] + rightWalls[i]
```

because that would count the same wall multiple times.

Example:

```text
Robot A destroys wall 50
Robot B also destroys wall 50
```

Correct contribution = `1`, not `2`.

Therefore, we must remember which walls have already been destroyed.

---

## DP + Memoization Idea

We sort:

* robots by position
* walls by position

Then define:

```text
dfs(idx, lastWall)
```

Where:

* `idx` = current robot index
* `lastWall` = first wall index not yet counted

For every robot, we try:

1. Shoot Left
2. Shoot Right

Count only the walls whose indices are `>= lastWall`.

Then recursively process the next robot.

---

## Transition

```text
dfs(idx, lastWall) = max(
    gainLeft  + dfs(idx + 1, newLastLeft),
    gainRight + dfs(idx + 1, newLastRight)
)
```

---

## Time Complexity

Let:

* `n` = number of robots
* `m` = number of walls

Complexity:

```text
O(n * m)
```

because each DP state `(idx, lastWall)` is computed once.

---

## C++ Solution

```cpp
class Solution {
public:
    int maxWalls(vector<int>& robots, vector<int>& distance, vector<int>& walls) {
        int n = robots.size(), m = walls.size();

        vector<pair<int,int>> r;
        for (int i = 0; i < n; i++) {
            r.push_back({robots[i], distance[i]});
        }

        sort(r.begin(), r.end());
        sort(walls.begin(), walls.end());

        vector<vector<int>> memo(n + 1, vector<int>(m + 1, -1));

        function<int(int,int)> dfs = [&](int idx, int lastWall) {
            if (idx == n) return 0;

            if (memo[idx][lastWall] != -1)
                return memo[idx][lastWall];

            int pos = r[idx].first;
            int dist = r[idx].second;

            // Shoot Left
            int L = pos - dist;
            if (idx > 0) L = max(L, r[idx - 1].first);

            int leftStart = lower_bound(walls.begin(), walls.end(), L) - walls.begin();
            int leftEnd = upper_bound(walls.begin(), walls.end(), pos) - walls.begin() - 1;

            int gainLeft = 0;
            int newLastLeft = lastWall;

            if (leftStart <= leftEnd) {
                int start = max(leftStart, lastWall);
                if (start <= leftEnd) {
                    gainLeft = leftEnd - start + 1;
                    newLastLeft = max(lastWall, leftEnd + 1);
                }
            }

            // Shoot Right
            int R = pos + dist;
            if (idx + 1 < n) R = min(R, r[idx + 1].first);

            int rightStart = lower_bound(walls.begin(), walls.end(), pos) - walls.begin();
            int rightEnd = upper_bound(walls.begin(), walls.end(), R) - walls.begin() - 1;

            int gainRight = 0;
            int newLastRight = lastWall;

            if (rightStart <= rightEnd) {
                int start = max(rightStart, lastWall);
                if (start <= rightEnd) {
                    gainRight = rightEnd - start + 1;
                    newLastRight = max(lastWall, rightEnd + 1);
                }
            }

            return memo[idx][lastWall] = max(
                gainLeft + dfs(idx + 1, newLastLeft),
                gainRight + dfs(idx + 1, newLastRight)
            );
        };

        return dfs(0, 0);
    }
};
```

---

## Example Dry Run

```text
robots   = [10, 2]
distance = [5, 1]
walls    = [5, 2, 7]
```

After sorting:

```text
robots = [(2,1), (10,5)]
walls  = [2,5,7]
```

* Robot at `2` fires left → destroys wall `2`
* Robot at `10` fires left → destroys walls `5` and `7`

Total = `3`

---

## Why Simple Greedy Fails

A greedy solution like:

```text
always choose the direction that destroys more walls
```

fails because:

* it may destroy walls already destroyed by earlier robots
* it may block a future robot from destroying more walls

Hence memoization / DP is required.

----
``` Difficulty Estimate ```
Codeforces: 2000–2200+
LeetCode Contest: 2600–2800
