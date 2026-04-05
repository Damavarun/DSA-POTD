# Robot Return to Origin

## Problem Statement

There is a robot starting at position `(0, 0)` on a 2D plane.

You are given a string `moves` where:

* `'U'` means move up
* `'D'` means move down
* `'L'` means move left
* `'R'` means move right

Return `true` if the robot returns back to the origin `(0, 0)` after performing all the moves, otherwise return `false`.

---

## C++ Solution

```cpp
class Solution {
public:
    bool judgeCircle(string moves) {
        int x = 0, y = 0;

        for (char move : moves) {
            if (move == 'U') y++;
            else if (move == 'D') y--;
            else if (move == 'R') x++;
            else if (move == 'L') x--;
        }

        return x == 0 && y == 0;
    }
};
```

---

## Approach

We track the robot's current position using two variables:

* `x` → horizontal position
* `y` → vertical position

For every move:

* `'U'` increases `y`
* `'D'` decreases `y`
* `'R'` increases `x`
* `'L'` decreases `x`

At the end:

* If `x == 0` and `y == 0`, the robot returned to the origin.
* Otherwise, it did not.

---

## Dry Run

### Example 1

```text
moves = "UD"
```

Steps:

```text
Start: (0, 0)
U -> (0, 1)
D -> (0, 0)
```

Output:

```text
true
```

---

### Example 2

```text
moves = "LL"
```

Steps:

```text
Start: (0, 0)
L -> (-1, 0)
L -> (-2, 0)
```

Output:

```text
false
```

---

## Time Complexity

```text
O(n)
```

Where `n` is the length of the string.

---

## Space Complexity

```text
O(1)
```

Only two variables are used.

---

## Constraints

```text
1 <= moves.length <= 2 * 10^4
moves contains only 'U', 'D', 'L', 'R'
```

---

## Key Idea

The robot returns to the origin only if:

* Number of `'U'` moves = Number of `'D'` moves
* Number of `'L'` moves = Number of `'R'` moves
