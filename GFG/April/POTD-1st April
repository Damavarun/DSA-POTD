# POTD - 1st April 2026

## Problem
Consecutive 1's not allowed

## Difficulty
Medium

## Approach
- Use DP / Fibonacci relation
- f(n) = f(n-1) + f(n-2)

## Code
```cpp
class Solution {
  public:
    int solve(int n, vector<int>& dp){
        if(n==1) return 2;
        if(n==2) return 3;

        if(dp[n]!=-1) return dp[n];

        return dp[n] = solve(n-1,dp) + solve(n-2,dp);
    }

    int countStrings(int n) {
        vector<int> dp(n+1,-1);
        return solve(n,dp);
    }
};
