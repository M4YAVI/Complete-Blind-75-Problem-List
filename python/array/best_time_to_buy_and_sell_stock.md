# 📉 Best Time to Buy and Sell Stock: The Lazy Genius Solution

---

### 🏷️ Problem Tags

```
Category    : Array
Technique   : Greedy / One Pass
Pattern     : Track Minimum So Far
Difficulty  : Easy
Similar To  : 122. Best Time II, 53. Maximum Subarray
```

---

### 💡 The "Aha!" Moment

You don't need to predict the future. You just need to know the **cheapest price you've seen so far**. 

If you sell today, your profit is `current_price - min_price_seen_so_far`. We calculate this potential profit for every day and keep the biggest one. That's it. No nested loops needed.

Format: *"The trick is maintaining a running `min_price`. For every price, update the minimum, then calculate `price - min_price`. The maximum value of that calculation is your answer."*

---

### ⚠️ Edge Cases to Handle

- **Empty array:** Cannot buy or sell. Profit 0.
- **Single element:** Cannot sell (need to buy first). Profit 0.
- **Price drops only:** E.g., `[5, 4, 3, 2, 1]`. Profit should be 0 (don't buy).
- **All same prices:** Profit 0.

---

### 🎯 Solution

```python
from typing import List
from math import inf

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices: return 0
        
        min_price = float('inf')
        max_profit = 0
        
        for price in prices:
            # Update minimum price seen so far
            min_price = min(min_price, price)
            
            # Update max profit if selling today is better
            max_profit = max(max_profit, price - min_price)
            
        return max_profit
```

---

### 🧪 Test Cases

```python
solution = Solution()

# Test 1: Normal case (Buy at 1, Sell at 6)
# Min starts at 7, then 1. Profit at 1 is 0.
# Next is 5: Profit 5-1=4.
# Next is 3: Profit 3-1=2.
# Next is 6: Profit 6-1=5 (New Max!).
# Next is 4: Profit 4-1=3.
assert solution.maxProfit([7, 1, 5, 3, 6, 4]) == 5

# Test 2: Decreasing prices (Never buy)
assert solution.maxProfit([7, 6, 4, 3, 1]) == 0

# Test 3: Edge case - empty or single
assert solution.maxProfit([]) == 0
assert solution.maxProfit([5]) == 0

print("All tests passed! ✓")
```

---

### ⏱️ Complexity Analysis

```
Time:  O(n) → We walk through the list of prices exactly once.
Space: O(1) → We track two variables (`min_price`, `max_profit`). No extra lists.
```

**Why this is optimal:** You must check every price at least once to ensure you didn't miss a massive spike or dip. O(n) is the floor. O(1) space is the minimum possible memory.

---

### 📖 How It Works (Step by Step)

Walk through like explaining to a smart 12-year-old:

1.  **The Time Machine:** Imagine you're walking through the days one by one. You have a sticky note for `min_price` and one for `max_profit`.
2.  **The Rule:** On day 1 (Price 7), finding a low price is all that matters. You write `7` on your `min_price` note.
3.  **The Algorithm:**
    *   Day 2 (Price 1): Is 1 lower than 7? Yes! Update `min_price` to 1.
    *   Day 3 (Price 5): Is 5 lower than 1? No. *But* if we sold now, we'd make 4 dollars (`5 - 1`). Write `4` on `max_profit`.
    *   Day 5 (Price 6): Is 6 lower than 1? No. If we sold now, we'd make 5 dollars (`6 - 1`). Is that better than our old `max_profit` (4)? Yes! Update `max_profit` to 5.
4.  **The Laziness:** We did it in one pass. No "what if I bought on Tuesday and sold on Friday?" thinking required.

---

### 🔄 Alternative Approaches

| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| **Brute Force** | O(n²) | O(1) | Checking every pair (buy day `i`, sell day `j`). Too slow. Don't do it. |
| **Kadane's Algo** | O(n) | O(1) | Converting array to differences (`prices[i] - prices[i-1]`) and finding max subarray sum. Same complexity, but harder to explain. |
| **This Solution** | **O(n)** | **O(1)** | **The Standard.** Cleanest logic. |
