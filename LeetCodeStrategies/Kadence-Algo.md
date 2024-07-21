# Kadane's ALgo

### Keywords
Maximum Sum subarray

# Algorithm:

Talk is cheap show me the code

```python
def maxSubArray(self, nums: List[int]) -> int:
    n = len(nums)
    maximumSum, currSumSubarray = float('-inf'), 0
    for i in range(n):
        currSumSubarray += nums[i]
        maximumSum = max(maximumSum, currSumSubarray)
        currSumSubarray = max(currSumSubarray, 0)
    return maximumSum
```

## Approach
Suppose we have the following input vector: [-2, 1, -3, 4, -1, 2, 1, -5, 4].

We initialize the maximumSum = INT_MIN (-2147483648) and currSumSubarray = 0.

We loop through the input vector and perform the following operations:

At the first iteration, currSumSubarray becomes -2 and since it is less than 0, we set it to 0. maximumSum remains at INT_MIN.

At the second iteration, currSumSubarray becomes 1, which is greater than 0, so we keep it as it is. We update maximumSum to 1.

At the third iteration, currSumSubarray becomes -2, which is less than 0, so we set it to 0. maximumSum remains at 1.

At the fourth iteration, currSumSubarray becomes 4, which is greater than 0, so we keep it as it is. We update maximumSum to 4.

At the fifth iteration, currSumSubarray becomes 3, which is greater than 0, so we keep it as it is. maximumSum remains at 4.

At the sixth iteration, currSumSubarray becomes 5, which is greater than 0, so we keep it as it is. We update maximumSum to 5.

At the seventh iteration, currSumSubarray becomes 6, which is greater than 0, so we keep it as it is. We update maximumSum to 6.

At the eighth iteration, currSumSubarray becomes 1, which is greater than 0, so we keep it as it is. maximumSum remains at 6.

At the ninth iteration, currSumSubarray becomes 5, which is greater than 0, so we keep it as it is. maximumSum remains at 6.

After iterating through the input vector, we return maximumSum which is equal to 6. Therefore, the maximum sum subarray of the given input vector is [4, -1, 2, 1], and the sum of this subarray is 6.

## Implementation

1. Initialize two variables, maximumSum and currSumSubarray to the minimum integer value (INT_MIN) and 0, respectively.
2. Loop through the array from index 0 to n-1, where n is the size of the array.
3. In each iteration, add the current element of the array to the currSumSubarray variable.
4. Take the maximum between maximumSum and currSumSubarray and store it in the maximumSum variable.
5. Take the maximum between currSumSubarray and 0 and store it in currSumSubarray. This is done because if the currSumSubarray becomes negative, it means that we should start a new subarray, so we reset currSumSubarray to 0.
6. After the loop ends, return the maximumSum variable, which contains the maximum sum of a subarray.