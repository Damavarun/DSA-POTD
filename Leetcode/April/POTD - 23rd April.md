Sum of Distances (LeetCode 2615)
🧠 Problem Statement

For each index i, compute:

sum of |i - j| for all j where nums[j] == nums[i] and j ≠ i
💡 Key Idea

Instead of brute force:

👉 Group indices by value

Example:

nums = [1,3,1,1,2]
1 → [0,2,3]
🔥 Optimization Trick

For each group:

Split into:

Left side
Right side

Use formulas:

left  = i * idx[i] - sum(left indices)
right = sum(right indices) - count * idx[i]
⚡ Use Prefix Sum

To get sums quickly:

prefix[i] = sum of indices till i
💻 Code
class Solution {
public:
    vector<long long> distance(vector<int>& nums) {
        int n = nums.size();

        unordered_map<int, vector<int>> mpp;
        for (int i = 0; i < n; i++) {
            mpp[nums[i]].push_back(i);
        }

        vector<long long> ans(n, 0);

        for (auto &it : mpp) {
            vector<int> &idx = it.second;
            int m = idx.size();

            vector<long long> prefix(m);
            prefix[0] = idx[0];
            for (int i = 1; i < m; i++) {
                prefix[i] = prefix[i - 1] + idx[i];
            }

            for (int i = 0; i < m; i++) {
                long long left = 0, right = 0;

                if (i > 0)
                    left = (long long)i * idx[i] - prefix[i - 1];

                if (i < m - 1)
                    right = (prefix[m - 1] - prefix[i]) - (long long)(m - i - 1) * idx[i];

                ans[idx[i]] = left + right;
            }
        }

        return ans;
    }
};
⏱ Complexity
Time: O(n)
Space: O(n)
