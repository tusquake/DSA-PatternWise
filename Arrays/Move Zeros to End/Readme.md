# Move Zeroes

## Problem Kya Hai?

Array mein saare **non-zero elements** ko left side move karo, **zeroes** ko right side (in-place).

**Goal:** `[0,1,0,3,12]` → `[1,3,12,0,0]`

---

## Approach - Two Pointers

```
index → Next position for non-zero element
i     → Current element being checked
```

**Logic:**
1. Non-zero mila → `index` position pe swap karo, `index++`
2. Zero mila → skip karo
3. Result: Non-zeroes left, zeroes automatically right

---

## Code

### C++:
```cpp
void moveZeroes(vector<int>& nums) {
    int index = 0;  // Next position for non-zero
    int i = 0;      // Current pointer
    
    while (i < nums.size()) {
        if (nums[i] != 0) {
            // Swap non-zero to index position
            swap(nums[i], nums[index]);
            index++;
        }
        i++;
    }
}
```

### Java:
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int index = 0;  // Next position for non-zero
        
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                // Swap non-zero to index position
                int temp = nums[i];
                nums[i] = nums[index];
                nums[index] = temp;
                index++;
            }
        }
    }
}
```

---

## Visual Example

### Input: `[0, 1, 0, 3, 12]`

```
Step 1: i=0, nums[0]=0 (skip)
[0, 1, 0, 3, 12]
 ↑  ↑
index=0, i=1

Step 2: i=1, nums[1]=1 (swap with index=0)
[1, 0, 0, 3, 12]
    ↑  ↑
index=1, i=2

Step 3: i=2, nums[2]=0 (skip)
[1, 0, 0, 3, 12]
    ↑     ↑
index=1, i=3

Step 4: i=3, nums[3]=3 (swap with index=1)
[1, 3, 0, 0, 12]
       ↑     ↑
index=2, i=4

Step 5: i=4, nums[4]=12 (swap with index=2)
[1, 3, 12, 0, 0]
          ↑
index=3, done!

Result: [1, 3, 12, 0, 0] ✓
```

---

## Dry Run

### Input: `[0, 1, 0, 3, 12]`

```
i=0: nums[0]=0 → skip
     index=0, i=1

i=1: nums[1]=1 → swap(nums[1], nums[0])
     [1,0,0,3,12], index=1, i=2

i=2: nums[2]=0 → skip
     index=1, i=3

i=3: nums[3]=3 → swap(nums[3], nums[1])
     [1,3,0,0,12], index=2, i=4

i=4: nums[4]=12 → swap(nums[4], nums[2])
     [1,3,12,0,0], index=3, i=5

Result: [1, 3, 12, 0, 0] ✓
```

---

## Why It Works?

```
index = "Where next non-zero should go"
i = "What I'm looking at"

Non-zero found → Place at index, index++
Zero found → Skip, index stays same

Zeroes automatically shift right!
```

---

## Complexity

**Time:** O(n) - Single pass  
**Space:** O(1) - In-place, only 2 pointers

---

## Edge Cases

```
All zeros: [0,0,0] → [0,0,0]
No zeros:  [1,2,3] → [1,2,3]
Single:    [0] → [0], [5] → [5]
```

---

## Common Mistake

```cpp
// ❌ Wrong - Extra space
vector<int> result;
for (int x : nums) if (x != 0) result.push_back(x);
// Not in-place!

// ✅ Correct - In-place swap
if (nums[i] != 0) swap(nums[i], nums[index++]);
```

---

## Key Takeaway

| Aspect | Value |
|--------|-------|
| **Approach** | Two pointers |
| **Time** | O(n) |
| **Space** | O(1) |
| **Trick** | Swap non-zeros to front |
