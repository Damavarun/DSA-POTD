# GFG POTD — Gray Code (April 4, 2026)

##  Problem Statement

Given a number `n`, generate all `n`-bit Gray codes.

A Gray code sequence is an ordering of binary numbers such that:

* The sequence starts with `0`
* Every consecutive number differs by exactly one bit

---

## Example

```text
Input:
n = 2

Output:
["00", "01", "11", "10"]
```

### Explanation

```text
00 -> 01   (1 bit changes)
01 -> 11   (1 bit changes)
11 -> 10   (1 bit changes)
```

---

# 💡 Idea

For every number `i` from `0` to `(2^n)-1`, its Gray code can be generated using:

```text
gray = i ^ (i >> 1)
```

Where:

* `^` = XOR operator
* `>> 1` = right shift by 1

---

## Example for n = 2

| i | Binary | Gray = i ^ (i>>1) | Result |
| - | ------ | ----------------- | ------ |
| 0 | 00     | 00 ^ 00 = 00      | 00     |
| 1 | 01     | 01 ^ 00 = 01      | 01     |
| 2 | 10     | 10 ^ 01 = 11      | 11     |
| 3 | 11     | 11 ^ 01 = 10      | 10     |

So the answer becomes:

```text
["00", "01", "11", "10"]
```

---

# C++ Solution

```cpp
class Solution {
  public:
    vector<string> graycode(int n) {
        vector<string> result;

        for (int i = 0; i < (1 << n); i++) {
            int gray = i ^ (i >> 1);

            string binary = "";

            for (int j = n - 1; j >= 0; j--) {
                if (gray & (1 << j))
                    binary += '1';
                else
                    binary += '0';
            }

            result.push_back(binary);
        }

        return result;
    }
};
```

---

# 🔍 Dry Run for n = 3

```text
0 -> 000
1 -> 001
2 -> 011
3 -> 010
4 -> 110
5 -> 111
6 -> 101
7 -> 100
```

Output:

```text
["000", "001", "011", "010", "110", "111", "101", "100"]
```

Observe that every adjacent pair differs by exactly one bit.

---

# ⏱ Complexity Analysis

* Time Complexity: `O(n * 2^n)`
* Space Complexity: `O(2^n)`

---

# 🚀 Key Takeaway

Gray code can be generated directly without recursion using:

```text
i ^ (i >> 1)
```

This is the most efficient and simplest way to solve the problem.
