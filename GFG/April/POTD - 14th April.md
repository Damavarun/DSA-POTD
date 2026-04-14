# Remove Spaces from a String

# Problem

# Given a string `s`, remove all spaces from the string and return the updated string.

---

## Example

```text
Input:
s = "a b  c d"

Output:
"abcd"
```

## Instead of creating another string, overwrite the original string.

```cpp
class Solution {
  public:
    string removeSpaces(string& s) {
        int j = 0;

        for (int i = 0; i < s.size(); i++) {
            if (s[i] != ' ') {
                s[j++] = s[i];
            }
        }

        s.resize(j);
        return s;
    }
};
```
----

### Complexity

* Time Complexity: `O(n)`
* Space Complexity: `O(1)`

