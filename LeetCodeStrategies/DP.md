# DP Strategies

[Lint to aticle](https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns#Minimum-(Maximum)-Path-to-Reach-a-Target)

## Before tackling DP Problems identify the following properties

1. Overlapping Subproblems
2. Optimal Substructure

## Identify the following things:
- Indeitfy the pattern and type of problem
- Identify the base case
- Create a recursive solution and check the overlapping sub problems
- Check for changing Parameters and Store the subproblems in a Table


# Types of Problems
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

## Distinct Ways/Possible Ways





# Article 2 [Article](https://ashutosh-kumar.medium.com/dynamic-programming-types-and-patterns-7b1406c46a6b)


## Types of problems

## 1. Knapsack 0/1

### Problem Statement
Given the weights and profit of ’N’ items, put these items in a knapsack of capacity ‘W’ to get the maximum total value in the knapsack.

### Code:

```python

def knapSack(cap, wt, profit, n): 
    K = [[0 for i in range(cap + 1)] for j in range(n + 1)] 

    for i in range(n + 1): 
        for j in range(cap + 1): 
            if i == 0 or j == 0: 
                K[i][j] = 0
            elif wt[i-1] <= j: 
                K[i][j] = max(profit[i-1] 
                          + K[i-1][j-wt[i-1]],   
                              K[i-1][j]) 
            else: 
                K[i][j] = K[i-1][j] 
  
    return K[n][cap] 

profit = [60, 100, 120] 
wt = [10, 20, 30] 
cap = 50
n = len(val) 
print(knapSack(cap, wt, profit, n))


```

### Similar Problems

- Subset Sum Problem
- Equal Sum Partition problem
- Target Sum




