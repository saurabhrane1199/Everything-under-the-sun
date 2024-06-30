# DP Strategies

[Lint to aticle](https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns#Minimum-(Maximum)-Path-to-Reach-a-Target)

## Minimum/Maximum

### Keywords : 
Minimum/Maximum cost, effort, days to do something

### Approach:

### Top Down Approach
Set a DP Struct (1-D Array, 2-D Array) where ```dp[i]``` is the cost till ```i```

Choose minimum (maximum) path among all possible paths before the current state, then add value for the current state.

dp should always contain increament calues of the answer such that dp[-1] is the final answer

### Code Snippet

Top-Down
```python
for (int j = 0; j < ways.size(); ++j) {
    result = min(result, topDown(target - ways[j]) + cost/ path / sum);
}
return memo[/*state parameters*/] = result;
```
Bottom-Up
```python
for (int i = 1; i <= target; ++i) {
   for (int j = 0; j < ways.size(); ++j) {
       if (ways[j] <= i) {
           dp[i] = min(dp[i], dp[i - ways[j]] + cost / path / sum) ;
       }
   }
}
return dp[target]
```

### Related Problems Solved

1. [ Minimum Path Sum ](https://leetcode.com/problems/minimum-path-sum/)
2. [Minimum Cost for tickets](https://leetcode.com/problems/minimum-cost-for-tickets/description/?envType=problem-list-v2&envId=55ac4kuc)
3. [Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/description/?envType=problem-list-v2&envId=55ac4kuc)
    - Not able to solve HARD question
    - Stupid of me to incorrectly set dp[i] ALWAYS SET DP to your final answer
4. [Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/description/?envType=problem-list-v2&envId=55ac4kuc)

