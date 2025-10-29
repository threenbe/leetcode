# Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.

## Sliding window solution:

```python3
    def maxProfit(self, prices: List[int]) -> int:
        maxProfit = 0
        # [7, 1, 5, 3, 6, 4]
        #     ^           ^
        # [6, 17, 1, 5, 9]
        #         ^     ^   
        buy = 0
        for sell in range(1, len(prices)):
            profit = prices[sell] - prices[buy]
            if (profit < 0):
                buy = sell
            else:
                maxProfit = profit if (profit > maxProfit) else maxProfit
        return maxProfit
```

## My brute force solution (times out in larger test cases):

```python3
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        maxProfit = 0
        for i in range(len(prices)):
            for j in range(i+1, len(prices)):
                profit = prices[j] - prices[i]
                maxProfit = profit if (profit > maxProfit) else maxProfit
        return maxProfit
```
