# Fruit Into Baskets - Interview Revision Notes

## Problem Kya Hai?

Ek farm pe ek row mein fruit trees hain. Har tree ek specific type ka fruit produce karta hai.

**Rules:**
1. Tumhare paas **2 baskets** hain
2. Har basket mein **sirf ek type ka fruit** rakh sakte ho
3. Ek basket mein **unlimited quantity** rakh sakte ho
4. Kisi bhi tree se start karo, **right direction** mein move karo
5. Har tree se **exactly one fruit** pick karo
6. Jab **third type** ka fruit mile, toh **stop** karna padega

**Goal:** Maximum kitne fruits collect kar sakte ho?

---

## Problem Reframe

**Original:** Two baskets mein maximum fruits collect karo

**Reframe:** Longest **contiguous subarray** find karo jisme **at most 2 distinct** elements hain

**Real-world analogy:** Shopping mein sirf 2 types ki items le sakte ho (budget constraint), aur continuously aage badhte rehna hai. Maximum kitni items le sakte ho?

---

## Example

### Example 1:
```
Input: N = 3, fruits = [2, 1, 2]
Output: 3

Explanation:
All fruits: 2, 1, 2
Distinct types: 2 (types: 1 and 2)
2 <= 2 baskets ✅
Pick all 3 fruits

Basket 1: [2, 2] (type 2)
Basket 2: [1] (type 1)
```

### Example 2:
```
Input: fruits = [0, 1, 2, 2]
Output: 3

Explanation:
Start from index 1: [1, 2, 2]
Distinct types: 2 (types: 1 and 2)
Pick 3 fruits

Can't pick all 4 because 3 types: 0, 1, 2
```

### Example 3:
```
Input: fruits = [1, 2, 1]
Output: 3

Explanation:
All fruits: [1, 2, 1]
Distinct types: 2 (types: 1 and 2)
Pick all 3 fruits
```

### Example 4:
```
Input: fruits = [3, 3, 3, 1, 2, 1, 1, 2, 3, 3, 4]
Output: 5

Explanation:
Best subarray: [1, 2, 1, 1, 2] (index 3 to 7)
Distinct types: 2 (types: 1 and 2)
Length: 5
```

---

## Solution - Sliding Window Approach

### Code (Java):
```java
class Solution {
    public int totalFruit(int[] fruits) {
        Map<Integer, Integer> map = new HashMap<>();
        int ans = 0;
        int start = 0;
        
        for (int i = 0; i < fruits.length; i++) {
            // Expand window: add current fruit
            map.put(fruits[i], map.getOrDefault(fruits[i], 0) + 1);
            
            // Shrink window: if more than 2 types
            while (map.size() > 2) {
                map.put(fruits[start], map.get(fruits[start]) - 1);
                if (map.get(fruits[start]) == 0) {
                    map.remove(fruits[start]);
                }
                start++;
            }
            
            // Update answer: valid window length
            if (map.size() <= 2) {
                ans = Math.max(ans, i - start + 1);
            }
        }
        
        return ans;
    }
}
```

### Code (C++):
```cpp
int totalFruits(int N, vector<int>& fruits) {
    unordered_map<int, int> mp;
    int ans = 0, start = 0;
    
    for (int i = 0; i < fruits.size(); i++) {
        mp[fruits[i]]++;
        
        while (mp.size() > 2) {
            mp[fruits[start]]--;
            if (mp[fruits[start]] == 0)
                mp.erase(fruits[start]);
            start++;
        }
        
        if (mp.size() <= 2)
            ans = max(ans, i - start + 1);
    }
    
    return ans;
}
```

---

## Step-by-Step Explanation

### Variables

```java
Map<Integer, Integer> map;  // Fruit type → frequency
int ans = 0;                // Maximum fruits collected
int start = 0;              // Window ka left pointer
int i;                      // Window ka right pointer (loop mein)
```

**Map purpose:** Track karo ki current window mein kitne distinct fruit types hain aur har type ki frequency kya hai.

---

### Step 1: Expand Window (Add Fruit)

```java
for (int i = 0; i < fruits.length; i++) {
    map.put(fruits[i], map.getOrDefault(fruits[i], 0) + 1);
```

**Logic:** Right pointer `i` ko aage badhao aur current fruit ko window mein add karo. Map mein frequency increment karo.

**Example:**
```
fruits = [1, 2, 1, 3]

i=0: Add 1 → map = {1:1}
i=1: Add 2 → map = {1:1, 2:1}
i=2: Add 1 → map = {1:2, 2:1}
i=3: Add 3 → map = {1:2, 2:1, 3:1} (3 types! Invalid)
```

---

### Step 2: Shrink Window (Remove Invalid Types)

```java
while (map.size() > 2) {
    map.put(fruits[start], map.get(fruits[start]) - 1);
    if (map.get(fruits[start]) == 0) {
        map.remove(fruits[start]);
    }
    start++;
}
```

**Condition:** `map.size() > 2`

**Meaning:** Current window mein **2 se zyada fruit types** hain. Matlab 3rd type aa gaya hai, invalid window!

**Logic:** Left pointer `start` ko aage badhao aur fruits ko window se remove karo jab tak distinct types <= 2 na ho jaye.

**Important:** Jab frequency 0 ho jaye, toh map se completely remove kar do. Isliye `map.size()` accurately distinct types count karega.

**Example:**
```
Window: [1, 2, 1, 3], map = {1:2, 2:1, 3:1}
map.size() = 3 > 2 ❌ Invalid!

Shrink:
Remove fruits[start]=1: map = {1:1, 2:1, 3:1}, start=1
map.size() = 3 > 2 (still invalid)

Remove fruits[start]=2: map = {1:1, 3:1}, start=2
map.size() = 2 <= 2 ✅ Valid!

New window: [1, 3]
```

---

### Step 3: Update Answer

```java
if (map.size() <= 2) {
    ans = Math.max(ans, i - start + 1);
}
```

**Condition:** `map.size() <= 2`

**Meaning:** Current window **valid** hai - at most 2 fruit types hain.

**Logic:** Valid window ki length se answer update karo.

**Window length:** `i - start + 1`

---

## Dry Run Example

**Input:** `fruits = [1, 2, 1, 2, 3]`

### Iteration by iteration:

**i=0: fruits[0]=1**
```
Add 1
Window: [1]
map = {1:1}
map.size() = 1 <= 2 ✅ Valid
ans = max(0, 0-0+1) = 1
```

**i=1: fruits[1]=2**
```
Add 2
Window: [1, 2]
map = {1:1, 2:1}
map.size() = 2 <= 2 ✅ Valid
ans = max(1, 1-0+1) = 2
```

**i=2: fruits[2]=1**
```
Add 1
Window: [1, 2, 1]
map = {1:2, 2:1}
map.size() = 2 <= 2 ✅ Valid
ans = max(2, 2-0+1) = 3
```

**i=3: fruits[3]=2**
```
Add 2
Window: [1, 2, 1, 2]
map = {1:2, 2:2}
map.size() = 2 <= 2 ✅ Valid
ans = max(3, 3-0+1) = 4
```

**i=4: fruits[4]=3**
```
Add 3
Window: [1, 2, 1, 2, 3]
map = {1:2, 2:2, 3:1}
map.size() = 3 > 2 ❌ Invalid! Too many types

Shrink:
Remove fruits[0]=1: map = {1:1, 2:2, 3:1}, start=1
map.size() = 3 > 2 (still invalid)

Remove fruits[1]=2: map = {1:1, 2:1, 3:1}, start=2
map.size() = 3 > 2 (still invalid)

Remove fruits[2]=1: map = {2:1, 3:1}, start=3
map.size() = 2 <= 2 ✅ Valid!

New window: [2, 3]
ans = max(4, 4-3+1) = max(4, 2) = 4
```

**Final Answer:** `4`

**Best window:** `[1, 2, 1, 2]` (indices 0 to 3)
- Basket 1: [1, 1] (type 1)
- Basket 2: [2, 2] (type 2)
- Total: 4 fruits

---

## Visual Representation

```
fruits = [3, 3, 3, 1, 2, 1, 1, 2, 3, 3, 4]
Index:    0  1  2  3  4  5  6  7  8  9  10

Window progression:

i=0-2:  [3 3 3]              types={3} ✅ length=3
i=3:    [3 3 3 1]            types={3,1} ✅ length=4
i=4:    [3 3 3 1 2]          types={3,1,2} ❌ Shrink!
        Shrink → [1 2]       types={1,2} ✅ length=2
i=5:    [1 2 1]              types={1,2} ✅ length=3
i=6:    [1 2 1 1]            types={1,2} ✅ length=4
i=7:    [1 2 1 1 2]          types={1,2} ✅ length=5  MAX!
i=8:    [1 2 1 1 2 3]        types={1,2,3} ❌ Shrink!
        Shrink → [2 3]       types={2,3} ✅ length=2
i=9:    [2 3 3]              types={2,3} ✅ length=3
i=10:   [2 3 3 4]            types={2,3,4} ❌ Shrink!
        Shrink → [3 4]       types={3,4} ✅ length=2

Maximum length: 5
Best subarray: [1, 2, 1, 1, 2] (indices 3-7)
Basket 1: [1, 1, 1] (type 1, count=3)
Basket 2: [2, 2] (type 2, count=2)
```

---

## Why This Approach Works?

**Sliding Window Benefits:**

1. **Efficient:** O(n) time - har fruit maximum 2 baar process hota hai
2. **Simple:** Window expand aur shrink logic straightforward hai
3. **Optimal:** No redundant calculations

**Key Insight:** 
- 2 baskets = at most 2 distinct types
- Longest valid window = maximum fruits with <= 2 types
- Sliding window naturally maintains this constraint

**Real-world analogy:** Buffet mein 2 plates hain, har plate mein ek type ka food. Line mein aage badhte jao, jab 3rd type mile toh pehle waale ko chhod do.

---

## Comparison with Similar Problems

| Problem | Constraint | Solution |
|---------|-----------|----------|
| **Fruit Into Baskets** | At most 2 distinct | Sliding window with map.size() <= 2 |
| Longest Substring (K Distinct) | At most K distinct | Same logic, map.size() <= K |
| Longest Substring (No Repeat) | All distinct | map.size() == window_size |
| Max Consecutive Ones III | At most K zeros | Count zeros instead |

**Pattern:** Sliding window with constraint on distinct elements/values.

---

## Complexity Analysis

### Time Complexity: O(N)

**Explanation:**
- Outer loop: O(N) - traverse entire array
- Inner while loop: Har element maximum ek baar remove hota hai
- Total operations: Add (N) + Remove (N) = 2N = O(N)

**Important:** `start` pointer monotonically increasing hai, kabhi decrease nahi hota.

### Space Complexity: O(1) or O(2)

**Explanation:**
- Map mein maximum 2 distinct fruit types store honge
- Actually O(2) = O(1) constant space
- Map size never exceeds 2 (we shrink when > 2)

---

## Edge Cases

1. **All same fruits:**
   ```
   fruits = [1, 1, 1, 1]
   Output: 4 (all fruits, only 1 type)
   ```

2. **All different fruits:**
   ```
   fruits = [1, 2, 3, 4]
   Output: 2 (any consecutive 2)
   ```

3. **Two types only:**
   ```
   fruits = [1, 2, 1, 2, 1]
   Output: 5 (all fruits)
   ```

4. **Single fruit:**
   ```
   fruits = [5]
   Output: 1
   ```

5. **Empty array:**
   ```
   fruits = []
   Output: 0
   ```

---

## Interview Tips

1. **Problem reframe karo:** "At most 2 distinct fruit types wala longest subarray find karna hai"

2. **Basket analogy use karo:** "Do baskets hain, har basket ek fruit type hold kar sakta hai"

3. **Map.size() explain karo:** "Map size distinct types count karta hai, isliye frequency 0 hone pe remove karte hain"

4. **Time complexity justify karo:** "O(N) hai kyunki har element maximum 2 baar touch hota hai (add aur remove)"

5. **Follow-up questions:**
   - K baskets allowed? (Change condition to map.size() <= K)
   - Return the actual fruits? (Track start and max length indices)
   - Minimize fruits? (Different problem - minimize window)

---

## Alternative Approach (Optimized - Last Index Tracking)

```java
int totalFruit(int[] fruits) {
    Map<Integer, Integer> lastIndex = new HashMap<>();
    int start = 0, ans = 0;
    
    for (int i = 0; i < fruits.length; i++) {
        lastIndex.put(fruits[i], i);
        
        if (lastIndex.size() > 2) {
            // Find leftmost fruit type and remove
            int minIndex = Collections.min(lastIndex.values());
            lastIndex.remove(fruits[minIndex]);
            start = minIndex + 1;
        }
        
        ans = Math.max(ans, i - start + 1);
    }
    
    return ans;
}
```

**Difference:** Store last index instead of frequency. When 3rd type comes, remove the fruit type with smallest last index (leftmost).

**Advantage:** No while loop, direct jump to valid window.

---

## Common Mistakes

1. **Map.size() vs frequency confusion:** Size gives distinct types count, not total fruits

2. **Not removing from map:** Frequency 0 hone pe map se erase karna bhulte hain

3. **While vs if for shrinking:** Multiple elements remove karne pad sakte hain, isliye while use karo

4. **Answer update condition:** `map.size() <= 2` check karna important

5. **Window size calculation:** `i - start + 1` formula yaad rakhna

---

## Key Takeaway

**Problem:** Collect maximum fruits with only 2 baskets (each holds one type).

**Reframe:** Find longest contiguous subarray with at most 2 distinct elements.

**Approach:** Sliding window
- Expand: Add fruits, track types in map
- Shrink: Remove fruits when types > 2
- Track: Maximum valid window length

**Core Logic:**
```
Valid window = map.size() <= 2 (at most 2 types)
Invalid window = map.size() > 2 (3rd type appeared)
Shrink until valid: Remove from left
```

**Formula:**
```
window_length = i - start + 1
distinct_types = map.size()
valid = distinct_types <= 2
```

**Real constraint:** 2 baskets = at most 2 distinct fruit types