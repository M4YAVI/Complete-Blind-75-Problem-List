# 🐍 Two Sum

---

### 🏷️ Problem Tags

```
Category    : Array
Technique   : Hash Map (Dictionary)
Pattern     : Complement Search
Difficulty  : Easy
```

---

### 💡 The "Aha!" Moment

The trick is **Math**. Instead of looking for a pair `(a, b)` by iterating twice, realize that for every number `num`, we are strictly looking for `target - num`. We check if we've *seen* that needed number before.

Format: *"The trick is calculating the **complement** (`needed = target - current`) and checking if it's already in our visited map. Once you see this, the code writes itself."*

---

### ⚠️ Edge Cases to Handle

- **No valid pair:** Problem usually guarantees one solution, but good to know.
- **Duplicates:** The map handles this naturally (we look for the *previous* occurrence).
- **Same element used twice:** Implicitly handled because we check the map *before* adding the current number.
- **Negative numbers:** Arithmetic works the same way (−3 + 5 = 2).

---

### 🎯 Solution

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # Map: value -> index
        seen = {}
        
        for i, num in enumerate(nums):
            needed = target - num
            
            # Found the matching piece? Return immediately.
            if needed in seen:
                return [seen[needed], i]
            
            # Remember this number for the future
            seen[num] = i
            
        return [] # Should not reach here if solution is guaranteed
```

---

### 🧪 Test Cases

```python
solution = Solution()

# Test 1: Normal case
# 2 + 7 = 9
assert solution.twoSum([2, 7, 11, 15], 9) == [0, 1]

# Test 2: Tricky duplicate case
# 3 + 3 = 6 (Must pick different indices)
assert solution.twoSum([3, 2, 4], 6) == [1, 2] # Wait, 2+4=6. Indices 1, 2.
assert solution.twoSum([3, 3], 6) == [0, 1]

# Test 3: Negative numbers
# -3 + (-5) = -8
assert solution.twoSum([-1, -2, -3, -4, -5], -8) == [2, 4]

print("All tests passed! ✓")
```

---

### ⏱️ Complexity Analysis

```
Time:  O(n) → We pass through the list exactly once. Lookup in dict is O(1).
Space: O(n) → In the worst case, we store nearly every element in the dictionary.
```

**Why this is optimal:** You absolutely MUST look at every number at least once to find the pair, so O(n) is the theoretical lower bound. We can't do better than "instant lookup" for the complement.

---

### 📖 How It Works (Step by Step)

Walk through like explaining to a smart 12-year-old:

1.  **The Memory Bank:** Imagine you are walking down a line of people holding numbers. You have a notebook (the `seen` dictionary).
2.  **The Question:** For each person you meet (like number `7`), you ask: "Has anyone passed by who holds the number `2`?" (Because `9 - 7 = 2`).
3.  **The Check:** You look in your notebook. 
    *   If "Yes, index 0 held a 2!", you stop. You found the pair! `[0, current_index]`.
    *   If "No", you write down "Index 1 holds a 7" in your notebook and move to the next person.
4.  **The Efficiency:** You never look back. You only look at your notebook. That's why it's fast.

---

### 🔄 Alternative Approaches

| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| **Brute Force** | O(n²) | O(1) | Never. Unless n < 50 and you forgot what a Hash Map is. |
| **Two Pointers** | O(n log n) | O(1) | If the array is **sorted** (or you sort it first). Good if memory is extremely tight. |
| **This Solution** | **O(n)** | **O(n)** | **The Standard.** Fastest for unsorted arrays. |
