#LeetCode 2463: Minimum Total Distance Traveled

## Problem

There are robots and factories on the X-axis.

* `robot[i]` = position of the ith robot
* `factory[j] = [positionj, limitj]`

  * `positionj` = position of the factory
  * `limitj` = maximum robots that factory can repair

Every robot must be assigned to exactly one factory.

The goal is to minimize the total distance traveled by all robots.

---

## Example

```text
Input:
robot = [0,4,6]
factory = [[2,2],[6,2]]

Output:
4
```

Explanation:

```text
0 -> 2   distance = 2
4 -> 2   distance = 2
6 -> 6   distance = 0

Total = 4
```

---

## Memoization Code

```cpp
class Solution {
public:
    long long memo[101][101];

    long long dfs(int i, int j, vector<int>& robot, vector<vector<int>>& factory) {
        if (i == robot.size()) return 0;
        if (j == factory.size()) return 1e18;

        if (memo[i][j] != -1) return memo[i][j];

        long long ans = dfs(i, j + 1, robot, factory);

        long long currDist = 0;
        int pos = factory[j][0];
        int limit = factory[j][1];

        for (int k = 0; k < limit && i + k < robot.size(); k++) {
            currDist += abs((long long)robot[i + k] - pos);

            ans = min(ans,
                      currDist + dfs(i + k + 1, j + 1, robot, factory));
        }

        return memo[i][j] = ans;
    }

    long long minimumTotalDistance(vector<int>& robot, vector<vector<int>>& factory) {
        sort(robot.begin(), robot.end());
        sort(factory.begin(), factory.end());

        memset(memo, -1, sizeof(memo));

        return dfs(0, 0, robot, factory);
    }
};
```

---

## Complexity

* Time Complexity: `O(R * F * L)`
* Space Complexity: `O(R * F)`
