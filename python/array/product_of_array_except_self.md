# ✖️ Product of Array Except Self

---

### 🏷️ Problem Tags

```
Category    : Array
Technique   : Prefix/Suffix Products
Pattern     : Two Pass (Left then Right)
Difficulty  : Medium
Similar To  : 42. Trapping Rain Water, 135. Candy
```

---

### 💡 The "Aha!" Moment

You are forbidden from using division. So, how do you get the product of everything *except* `i`?

Simple: **Left Side × Right Side**.

The product of array except `nums[i]` is just `(Product of all numbers left of i) * (Product of all numbers right of i)`. We don't need new arrays for this. We can re-use the result array.

Format: *"The trick is doing **two passes**. First, fill the result array with 'Left Products'. Second, walk backwards and multiply by 'Right Products' on the fly. No extra space needed."*

---

### ⚠️ Edge Cases to Handle

- **Zeros:** If there's one zero, everything else is 0 except the zero position. If there are two zeros, everything is 0. Our logic handles this naturally without special `if` checks!
- **Empty/Single Element:** Usually n >= 2 per constraints, but logic holds.
- **No Division Allowed:** This is the hard constraint.

---

### 🎯 Solution

```python
from typing import List

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        res = [1] * n
        
        # Pass 1: Calculate Prefix Products (Left to Right)
        # res[i] will contain product of nums[0]...nums[i-1]
        prefix = 1
        for i in range(n):
            res[i] = prefix
            prefix *= nums[i]
            
        # Pass 2: Calculate Suffix Products (Right to Left)
        # Multiply existing res[i] by "everything to the right"
        suffix = 1
        for i in range(n - 1, -1, -1):
            res[i] *= suffix
            suffix *= nums[i]
            
        return res
```

---

### 🧪 Test Cases

```python
solution = Solution()

# Test 1: Normal case
# Input:  [1, 2, 3, 4]
# Left:   [1, 1, 2, 6]  (Prefixes)
# Right:  [24, 12, 4, 1] (Suffixes - conceptual)
# Result: [24, 12, 8, 6]
assert solution.productExceptSelf([1, 2, 3, 4]) == [24, 12, 8, 6]

# Test 2: With Zero
# Input:  [-1, 1, 0, -3, 3]
# Only the spot with 0 gets a non-zero product.
assert solution.productExceptSelf([-1, 1, 0, -3, 3]) == [0, 0, 9, 0, 0]

print("All tests passed! ✓")
```

---

### ⏱️ Complexity Analysis

```
Time:  O(n) → We iterate through the list exactly twice (2n is O(n)).
Space: O(1) → The output array doesn't count towards space complexity (as per problem statement). We only use two variables (`prefix`, `suffix`).
```

**Why this is optimal:** You must read every number to compute the product. We do the minimum possible traversals (2) without strictly required extra memory.

---

### 📖 How It Works (Step by Step)

Walk through like explaining to a smart 12-year-old:

1.  **The Setup:** Imagine `res` is a blank sheet of paper, initialized to `[1, 1, 1, 1]`.
2.  **The Walk Forward:** You walk down the line. At step `i`, you whisper to current person: "Here is the product of everyone *before* you."
    *   Example `[1, 2, 3, 4]`:
    *   Index 0 gets 1. (Prefix becomes 1*1 = 1)
    *   Index 1 gets 1. (Prefix becomes 1*2 = 2)
    *   Index 2 gets 2. (Prefix becomes 2*3 = 6)
    *   Index 3 gets 6. (Prefix becomes 6*4 = 24)
    *   `res` is now `[1, 1, 2, 6]`.
3.  **The Moonwalk (Backward):** Now you walk backwards. You maintain a `suffix` product of everyone you've passed on the right.
    *   Index 3: multiplied by 1. `res` stays 6. (Suffix becomes 1*4 = 4)
    *   Index 2: multiplied by 4. `res` becomes 2*4 = 8. (Suffix becomes 4*3 = 12)
    *   Index 1: multiplied by 12. `res` becomes 1*12 = 12. (Suffix becomes 12*2 = 24)
    *   Index 0: multiplied by 24. `res` becomes 1*24 = 24.
4.  **Done.**

---

### 🔄 Alternative Approaches

| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| **Division** | O(n) | O(1) | Calculate total product, then `total / nums[i]`. **Fails if Zeros exist.** And explicitly forbidden. |
| **Brute Force** | O(n²) | O(1) | Nested loops. Way too slow for large inputs. |
| **3-Array approach** | O(n) | O(n) | Create explicit `prefix` and `suffix` arrays. Easier to understand, but wastes memory. |
| **This Solution** | **O(n)** | **O(1)** | **The Standard.** Memory efficient. |
