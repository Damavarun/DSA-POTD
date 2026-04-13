
# README 2 — GFG POTD: Next Smallest Palindrome

## Problem

Given a number in the form of an array `num[]` containing digits, find the next smallest palindrome strictly greater than the given number.

A palindrome is a number that reads the same from left to right and right to left.

---

## Example 1

```text
Input:
[2, 3, 5, 4, 5]

Output:
[2, 3, 6, 3, 2]
```

Explanation:

```text
23545
↓ mirror left to right
23532

23532 < 23545
So increase the middle digit:
23632
```

---

## Example 2

```text
Input:
[1, 9, 9, 1]

Output:
[2, 0, 0, 2]
```

Explanation:

```text
1991
↓ already palindrome
Increase middle pair 99
Carry propagates to left
2002
```

---

## Approach

1. Copy the left half to the right half.
2. If the resulting palindrome is greater than the original number, return it.
3. Otherwise, increase the middle digit(s).
4. Propagate carry if required.
5. Again mirror the left half to the right half.

---

## C++ Solution

```cpp
class Solution {
public:
    vector<int> generateNextPalindrome(int num[], int n) {
        vector<int> ans(num, num + n);

        int i = 0, j = n - 1;

        // Mirror left to right
        while (i < j) {
            ans[j] = ans[i];
            i++;
            j--;
        }

        // Check if mirrored number is greater
        bool greater = false;
        for (int k = 0; k < n; k++) {
            if (ans[k] > num[k]) {
                greater = true;
                break;
            }
            else if (ans[k] < num[k]) {
                break;
            }
        }

        if (greater) return ans;

        int carry = 1;
        int mid = n / 2;

        if (n % 2 == 1) {
            ans[mid] += carry;
            carry = ans[mid] / 10;
            ans[mid] %= 10;
            i = mid - 1;
            j = mid + 1;
        } else {
            i = mid - 1;
            j = mid;
        }

        // Propagate carry and mirror
        while (i >= 0) {
            ans[i] += carry;
            carry = ans[i] / 10;
            ans[i] %= 10;

            ans[j] = ans[i];

            i--;
            j++;
        }

        // Special case: all digits are 9
        if (carry) {
            vector<int> res(n + 1, 0);
            res[0] = 1;
            res[n] = 1;
            return res;
        }

        return ans;
    }
};
```

---

## Complexity Analysis

* Time Complexity: `O(n)`
* Space Complexity: `O(n)`
