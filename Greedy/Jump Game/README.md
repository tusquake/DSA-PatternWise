Tumhe ek array diya gaya hai jisme **har index pe ek number** hai.

Tum **index 0 se start** karte ho aur **last index tak** pahunchna hai.

Har index pe jo number hai, woh **maximum kitne steps jump kar sakte ho** woh batata hai.

**Goal:** Check karo ki last index tak **pahunch sakte ho ya nahi**?

---

## Example

```
nums = [2, 3, 1, 1, 4]
Index:  0  1  2  3  4
```

**Explanation:**
- Index 0 pe ho → nums[0] = 2 → Max 2 steps jump kar sakte ho
- Jump to index 1 or 2
- Index 1 pe → nums[1] = 3 → Max 3 steps jump kar sakte ho
- Jump to index 2, 3, or 4 ✅
- **Last index (4) tak pahunch gaye!** ✅

---

## Solution - Greedy Approach

### Code (Java):
```java
class Solution {
    public boolean canJump(int[] nums) {
        int farthest = 0;  // Ab tak kitna door ja sakte hain
        
        for (int i = 0; i < nums.length; i++) {
            // Agar current index tak nahi pahunch sakte
            if (farthest < i) {
                return false;  // Stuck ho gaye!
            }
            
            // Update farthest position
            farthest = Math.max(farthest, nums[i] + i);
        }
        
        return true;  // Last tak pahunch gaye!
    }
}
```

### Code (C++):
```cpp
bool canJump(vector<int>& nums) {
    int farthest = 0;
    
    for (int i = 0; i < nums.size(); i++) {
        if (farthest < i)
            return false;
        farthest = max(farthest, nums[i] + i);
    }
    
    return true;
}
```

---

## Step-by-Step Explanation

### Variable Setup
```java
int farthest = 0;  // Ab tak maximum kitna door jump kar sakte hain
```

**Logic:** Ye track karega ki tumhare current position se **maximum kahan tak pahunch sakte ho**.

---

### Main Loop Logic

```java
for (int i = 0; i < nums.length; i++) {
    if (farthest < i) {
        return false;  // Current index tak hi nahi pahunch sakte!
    }
    
    farthest = Math.max(farthest, nums[i] + i);
}
```

### Check 1: Kya Current Index Reachable Hai?
```java
if (farthest < i) {
    return false;
}
```

**Logic:** 
- Agar `farthest < i` matlab current index `i` tak **pahunch hi nahi sakte**
- Matlab beech mein **stuck** ho gaye
- Directly `false` return karo ❌

**Example:**
```
nums = [1, 0, 1, 0]
Index:  0  1  2  3

i=0: farthest = 0+1 = 1
i=1: farthest = max(1, 1+0) = 1
i=2: farthest (1) < i (2) → STUCK! Return false ❌
```

---

### Update Farthest Position
```java
farthest = Math.max(farthest, nums[i] + i);
```

**Logic:**
- Current index `i` se tum **maximum `nums[i]` steps** jump kar sakte ho
- Toh tum **index `i + nums[i]` tak** pahunch sakte ho
- **Farthest** ko update karo: jo bhi zyada ho, woh rakh lo

**Formula:** `new_position = current_index + jump_power`

**Example:**
```
i = 2, nums[2] = 5
Current position: 2
Jump power: 5
Farthest reach: 2 + 5 = 7
```

---

## Dry Run Example 1 (Success Case)

**Input:** `nums = [2, 3, 1, 1, 4]`

| i | nums[i] | farthest (before) | i + nums[i] | farthest < i? | farthest (after) | Notes |
|---|---------|-------------------|-------------|---------------|------------------|-------|
| 0 | 2 | 0 | 0+2=2 | 0<0? ❌ | max(0,2)=2 | Can reach index 2 |
| 1 | 3 | 2 | 1+3=4 | 2<1? ❌ | max(2,4)=4 | Can reach index 4! |
| 2 | 1 | 4 | 2+1=3 | 4<2? ❌ | max(4,3)=4 | Still 4 |
| 3 | 1 | 4 | 3+1=4 | 4<3? ❌ | max(4,4)=4 | Still 4 |
| 4 | 4 | 4 | 4+4=8 | 4<4? ❌ | max(4,8)=8 | Last index! |

**Loop ends → Return `true`** ✅

**Explanation:** 
- Index 1 se hi last index (4) tak pahunch sakte the!
- `farthest` kabhi bhi current index se peeche nahi raha

---

## Dry Run Example 2 (Failure Case)

**Input:** `nums = [3, 2, 1, 0, 4]`

| i | nums[i] | farthest (before) | i + nums[i] | farthest < i? | farthest (after) | Notes |
|---|---------|-------------------|-------------|---------------|------------------|-------|
| 0 | 3 | 0 | 0+3=3 | 0<0? ❌ | max(0,3)=3 | Can reach index 3 |
| 1 | 2 | 3 | 1+2=3 | 3<1? ❌ | max(3,3)=3 | Still 3 |
| 2 | 1 | 3 | 2+1=3 | 3<2? ❌ | max(3,3)=3 | Still 3 |
| 3 | 0 | 3 | 3+0=3 | 3<3? ❌ | max(3,3)=3 | Can't go further! |
| 4 | 4 | 3 | - | **3<4? ✅** | - | **STUCK!** |

**Return `false` at i=4** ❌

**Explanation:**
- Index 3 pe **0** hai → aage jump nahi kar sakte
- Index 4 tak pahunchna impossible!
- `farthest` (3) < current index (4)

---

## Visual Representation

### Success Case:
```
nums = [2, 3, 1, 1, 4]
Index:  0  1  2  3  4

Step 0: [●]──────────────  farthest = 2
        Can reach: 0,1,2

Step 1:  0 [●]────────────  farthest = 4
           Can reach: 0,1,2,3,4 ✅ (GOAL REACHED!)

✅ Last index reachable!
```

### Failure Case:
```
nums = [3, 2, 1, 0, 4]
Index:  0  1  2  3  4

Step 0: [●]──────────────  farthest = 3
        Can reach: 0,1,2,3

Step 3:  0  1  2 [●] | 4   farthest = 3
                 WALL! Can't cross!

❌ Last index NOT reachable!
```

---

## Why Greedy Works Here?

**Greedy Choice:** Har step pe **maximum reach** track karo, agar stuck ho toh false, warna continue!

**Why optimal?**
- Tumhe actual path find karne ki zarurat nahi
- Bas ye check karna hai ki **reachable hai ya nahi**
- Agar kisi bhi index se last tak pahunch sakte ho → answer YES
- Agar beech mein stuck ho gaye → answer NO

**Key insight:** Tumhe best path choose nahi karna, bas **feasibility check** karna hai! 🎯

---

## Complexity Analysis

- **Time Complexity:** `O(n)` 
  - Single pass through array
  - Har element ko ek baar visit kiya
  
- **Space Complexity:** `O(1)` 
  - Sirf ek variable `farthest` use kiya
  - No extra space needed!

---

## Edge Cases

1. **Single element:**
   ```
   nums = [0]
   ```
   → Already at last index → `true` ✅

2. **First element is 0:**
   ```
   nums = [0, 1, 2]
   ```
   → Can't move from index 0 → `false` ❌

3. **All zeros except first:**
   ```
   nums = [1, 0, 0, 0]
   ```
   → Can only reach index 1 → `false` (if n>2) ❌

4. **Large jumps:**
   ```
   nums = [10, 0, 0, 0, 0]
   ```
   → First jump covers everything → `true` ✅

5. **Already at destination:**
   ```
   nums = [5] (single element)
   ```
   → `true` ✅

---

## Interview Tips 💡

1. **Greedy vs DP mention karo:**
   - Greedy: O(n) time, O(1) space ✅
   - DP/BFS: O(n²) time, O(n) space (overkill!)

2. **Key insight explain karo:** "Hume actual jumps nahi chahiye, bas check karna hai ki reachable hai ya nahi. Isliye maximum reach track karna kaafi hai."

3. **Visualization:** "Imagine a rope that extends as we move - if rope breaks (farthest < i), we can't proceed."

4. **Alternative approach:** BFS/DFS possible but unnecessary complexity

5. **Follow-up questions:**
   - Minimum jumps chahiye? (Jump Game II - different problem, DP needed)
   - Print the path? (Need to track actual jumps, backtracking)
   - Multiple starting points? (Run algorithm from each point)

6. **Edge case mention karo:** First element 0 hai toh directly false

---

## Related Problems

1. **Jump Game II:** Minimum jumps needed (harder, needs DP or greedy with BFS)
2. **Jump Game III:** Can reach value 0? (DFS/BFS)
3. **Jump Game IV:** Jump to same values (Graph problem)

---

## Common Mistakes ❌

1. **DP use karna:** Overkill hai, greedy sufficient hai
2. **Actual path track karna:** Problem sirf YES/NO puchta hai
3. **farthest update bhulna:** Har step pe update karna zaruri
4. **Index out of bounds:** Loop condition check karo properly
5. **Return statement placement:** Loop ke andar check karo (early termination)

---
