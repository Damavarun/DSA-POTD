# README 1 — LeetCode POTD: Minimum Distance to the Target Element

## Problem

Given an integer array `nums` (0-indexed) and two integers `target` and `start`, find an index `i` such that:

* `nums[i] == target`
* `abs(i - start)` is minimum

Return the minimum distance.

---

## Example

```text
Input:
nums = [1,2,3,4,5]
target = 5
start = 3

Output:
1
```

Explanation:

```text
nums[4] = 5
abs(4 - 3) = 1
```

---

## Approach

Traverse the array.
Whenever you find `target`, calculate:

```text
abs(i - start)
```

Keep the minimum value among all such distances.

---

## C++ Solution

```cpp
class Solution {
public:
    int getMinDistance(vector<int>& nums, int target, int start) {
        int ans = INT_MAX;

        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == target) {
                ans = min(ans, abs(i - start));
            }
        }

        return ans;
    }
};
```

---

## Complexity Analysis

* Time Complexity: `O(n)`
* Space Complexity: `O(1)`
