Problem 1: Split Array into Two Equal Subarrays (GFG)
🧠 Problem Statement

Given an array arr[], determine whether it can be split into two contiguous subarrays such that both parts have equal sum.

💡 Key Idea
Total sum must be even

Find a point where:

prefixSum == totalSum - prefixSum

prefixSum == totalSum / 2
⚡ Approach
Compute total sum
If total is odd → return false
Traverse array and maintain prefix sum
If prefix == total/2 → return true
💻 Code
class Solution {
public:
    bool canSplit(vector<int>& arr) {
        int total = 0;
        for (int x : arr) total += x;

        if (total % 2 != 0) return false;

        int prefix = 0;
        for (int i = 0; i < arr.size() - 1; i++) {
            prefix += arr[i];
            if (prefix == total / 2) return true;
        }

        return false;
    }
};
⏱ Complexity
Time: O(n)
Space: O(1)
