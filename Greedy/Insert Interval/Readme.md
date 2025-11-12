 # Insert Interval - Interview Revision Notes

## Problem Kya Hai?

Tumhe ek sorted array of intervals diya gaya hai jisme **koi overlap nahi hai**.

Ek **naya interval** insert karna hai taaki:
1. Array **sorted** rahe (start time ke basis pe)
2. Koi **overlap na ho** (agar overlap ho toh merge kar do)

**Input:**
- `intervals[][]` - Non-overlapping sorted intervals
- `newInterval[]` - Naya interval jo insert karna hai

**Output:** Updated intervals array with newInterval merged properly

---

## Example

### Example 1:
```
Input: 
intervals = [[1,2], [3,5], [6,7], [8,10], [12,16]]
newInterval = [4,8]

Output: [[1,2], [3,10], [12,16]]

Explanation:
newInterval [4,8] overlaps with [3,5], [6,7], [8,10]
Sab ko merge karke [3,10] ban gaya
```

### Example 2:
```
Input:
intervals = [[1,3], [6,9]]
newInterval = [2,5]

Output: [[1,5], [6,9]]

Explanation:
newInterval [2,5] overlaps with [1,3]
Merge karke [1,5] ban gaya
```

---

## Solution - Three Cases Approach

### Code (Java):
```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> result = new ArrayList<>();
        int n = intervals.length;
        
        for (int i = 0; i < n; i++) {
            // Case 1: newInterval is after current interval (no overlap)
            if (intervals[i][1] < newInterval[0]) {
                result.add(intervals[i]);
            }
            // Case 2: newInterval is before current interval (no overlap)
            else if (newInterval[1] < intervals[i][0]) {
                result.add(newInterval);
                newInterval = intervals[i];  // Update for next iterations
            }
            // Case 3: newInterval overlaps with current interval
            else {
                newInterval[0] = Math.min(intervals[i][0], newInterval[0]);
                newInterval[1] = Math.max(intervals[i][1], newInterval[1]);
            }
        }
        
        // Add the last interval (either merged or original newInterval)
        result.add(newInterval);
        
        return result.toArray(new int[result.size()][]);
    }
}
```

### Code (C++):
```cpp
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
    vector<vector<int>> ans;
    int n = intervals.size();
    
    for (int i = 0; i < n; i++) {
        // Case 1: newInterval is after the current interval
        if (intervals[i][1] < newInterval[0]) {
            ans.push_back(intervals[i]);
        }
        // Case 2: newInterval is before the current interval
        else if (newInterval[1] < intervals[i][0]) {
            ans.push_back(newInterval);
            newInterval = intervals[i];
        }
        // Case 3: newInterval overlaps with the current interval
        else {
            newInterval[0] = min(intervals[i][0], newInterval[0]);
            newInterval[1] = max(intervals[i][1], newInterval[1]);
        }
    }
    
    ans.push_back(newInterval);
    return ans;
}
```

---

## Step-by-Step Explanation

### Three Cases to Handle

Har interval ke liye teen cases possible hain:

```
Case 1: Current interval newInterval se pehle hai (no overlap)
   [----current----]          [----new----]

Case 2: Current interval newInterval ke baad hai (no overlap)
                [----new----]          [----current----]

Case 3: Overlap hai (merge needed)
        [----current----]
              [----new----]
```

---

### Case 1: Current Interval Pehle Khatam Ho Gaya

```java
if (intervals[i][1] < newInterval[0]) {
    result.add(intervals[i]);
}
```

**Condition:** Current interval ka **end** < newInterval ka **start**

**Logic:** Idhr aisa krenge toh current interval newInterval se pehle complete ho gaya hai, koi overlap nahi. Seedha result mein add kar do.

**Visual:**
```
Current:  [1, 2]
New:              [5, 7]

Timeline: [1===2]      [5===7]
          No overlap! Current ko add kar do.
```

**Real-world analogy:** Meeting 1 (9-10am) aur Meeting 2 (2-3pm) - completely separate hain, dono schedule kar sakte ho.

---

### Case 2: Current Interval Baad Mein Hai

```java
else if (newInterval[1] < intervals[i][0]) {
    result.add(newInterval);
    newInterval = intervals[i];
}
```

**Condition:** newInterval ka **end** < current interval ka **start**

**Logic:** Idhr aisa krenge toh newInterval current se pehle hai, koi overlap nahi.
- Pehle **newInterval** ko result mein add karo
- Phir **current interval** ko newInterval mein store kar lo (next iterations ke liye)

**Visual:**
```
New:      [2, 3]
Current:          [5, 7]

Timeline: [2===3]   [5===7]
          No overlap! New ko add karo, current ko next ke liye rakh lo.
```

**Important:** Ek baar newInterval add ho gaya, toh baki sare intervals ko bhi add karna hai. Isliye `newInterval = intervals[i]` se current ko next iteration ke liye set karte hain.

**Real-world analogy:** Tum 9-10am ki meeting schedule kar rahe ho, aur 2-3pm wali already scheduled hai. Dono alag hain.

---

### Case 3: Overlap Hai - Merge Karo

```java
else {
    newInterval[0] = Math.min(intervals[i][0], newInterval[0]);
    newInterval[1] = Math.max(intervals[i][1], newInterval[1]);
}
```

**Condition:** Overlap hai (neither Case 1 nor Case 2)

**Logic:** Idhr aisa krenge toh dono intervals ko merge kar denge:
- **Start:** Dono mein se jo chhota ho
- **End:** Dono mein se jo bada ho

**Visual:**
```
Current:  [3, 5]
New:          [4, 8]

Merged:   [3, 8]

Timeline: 
Before:   [3===5]
              [4======8]
After:    [3=========8]
```

**Important:** newInterval update ho jata hai merged interval se. Agar next interval bhi overlap kare toh woh bhi merge ho jayega!

**Real-world analogy:** Do overlapping meetings ko combine karke ek badi meeting bana rahe ho.

---

### Final Step: Last Interval Add Karo

```java
result.add(newInterval);
```

**Logic:** Loop khatam hone ke baad jo bhi `newInterval` hai (merged ya original), use result mein add karo.

**Why?** Kyunki loop mein sirf overlap check kar rahe the, last interval always pending rahega.

---

## Dry Run Example

**Input:**
```
intervals = [[1,2], [3,5], [6,7], [8,10], [12,16]]
newInterval = [4,8]
```

### Processing:

**Iteration 1: [1,2]**
```
Current: [1,2], New: [4,8]
Check: intervals[0][1] < newInterval[0] → 2 < 4 ✅
Case 1: Add [1,2] to result
result = [[1,2]]
newInterval = [4,8] (unchanged)
```

**Iteration 2: [3,5]**
```
Current: [3,5], New: [4,8]
Check: 5 < 4? ❌, 8 < 3? ❌ → Overlap!
Case 3: Merge
newInterval[0] = min(3, 4) = 3
newInterval[1] = max(5, 8) = 8
newInterval = [3,8]
result = [[1,2]]
```

**Iteration 3: [6,7]**
```
Current: [6,7], New: [3,8]
Check: 7 < 3? ❌, 8 < 6? ❌ → Overlap!
Case 3: Merge
newInterval[0] = min(6, 3) = 3
newInterval[1] = max(7, 8) = 8
newInterval = [3,8] (unchanged)
result = [[1,2]]
```

**Iteration 4: [8,10]**
```
Current: [8,10], New: [3,8]
Check: 10 < 3? ❌, 8 < 8? ❌ → Overlap! (8 == 8)
Case 3: Merge
newInterval[0] = min(8, 3) = 3
newInterval[1] = max(10, 8) = 10
newInterval = [3,10]
result = [[1,2]]
```

**Iteration 5: [12,16]**
```
Current: [12,16], New: [3,10]
Check: 16 < 3? ❌, 10 < 12? ✅
Case 2: Add [3,10] to result, update newInterval
result = [[1,2], [3,10]]
newInterval = [12,16]
```

**After Loop:**
```
Add newInterval [12,16] to result
result = [[1,2], [3,10], [12,16]]
```

**Final Answer:** `[[1,2], [3,10], [12,16]]`

---

## Visual Timeline

```
Original intervals:
[1=2]  [3==5] [6=7] [8==10]      [12====16]

New interval to insert:
           [4============8]

Step by step merge:
[1=2]  [3==5]                    [12====16]  ← [1,2] added
       [3======8]                [12====16]  ← [3,5] + [4,8] merged
       [3======8]                [12====16]  ← [6,7] merged into [3,8]
       [3=========10]            [12====16]  ← [8,10] merged into [3,8]
       [3=========10]  [12====16]            ← [12,16] added as separate

Final result:
[1=2]  [3=========10]  [12====16]
```

---

## Why This Approach Works?

**Key Insight:** Intervals already sorted hain, toh single pass mein sab cases handle ho jayenge.

**Three cases cover all possibilities:**
1. Current pehle khatam → No overlap, add kar do
2. New pehle khatam → No overlap, new add kar do aur current ko next ke liye rakh lo
3. Overlap hai → Merge kar do aur aage check karo

**Greedy approach:** Har step pe decision le rahe hain - merge ya add. Ek hi pass mein answer mil jata hai.

**Real-world analogy:** Calendar mein meetings schedule karna - agar overlap hai toh combine kar lo, nahi toh separate rakh lo.

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
    - Single pass through all intervals
    - Har interval ko ek baar process kiya

- **Space Complexity:** `O(n)`
    - Result array banane ke liye
    - Input ko modify nahi kar rahe (good practice)

---

## Edge Cases

1. **newInterval sabse pehle hai:**
   ```
   intervals = [[3,5], [7,9]]
   newInterval = [1,2]
   Output: [[1,2], [3,5], [7,9]]
   ```

2. **newInterval sabse end mein hai:**
   ```
   intervals = [[1,2], [3,5]]
   newInterval = [7,9]
   Output: [[1,2], [3,5], [7,9]]
   ```

3. **newInterval sab ko cover karta hai:**
   ```
   intervals = [[3,5], [6,7]]
   newInterval = [1,10]
   Output: [[1,10]]
   ```

4. **Empty intervals:**
   ```
   intervals = []
   newInterval = [5,7]
   Output: [[5,7]]
   ```

5. **Single interval overlap:**
   ```
   intervals = [[1,5]]
   newInterval = [2,3]
   Output: [[1,5]]  (newInterval completely inside)
   ```

6. **Touch but not overlap:**
   ```
   intervals = [[1,2], [3,5]]
   newInterval = [2,3]
   Output: [[1,5]]  (merge because touching)
   ```

---

## Interview Tips

1. **Three cases clearly explain karo:** "Idhr aisa krenge toh teen cases handle karenge - before, after, aur overlap"

2. **Sorted array advantage:** "Kyunki already sorted hai, ek hi pass mein sab handle ho jayega"

3. **Why update newInterval:** "Overlap case mein newInterval ko update karte hain taaki agle intervals ke saath bhi merge check ho sake"

4. **Last interval important:** "Loop ke baad jo bhi newInterval hai use add karna zaruri hai"

5. **Alternative approaches:**
    - Binary search to find position, then merge: More complex
    - Separate loops for before, overlap, after: More code
    - Current approach optimal hai - O(n) time, simple logic

6. **Follow-up questions:**
    - Agar intervals sorted nahi hote? (Pehle sort karna padta O(n log n))
    - Multiple intervals insert karne hote? (Ek ek karke insert karo)
    - In-place modification allowed? (Tricky, extra space avoid ho sakti hai)

---

## Common Mistakes

1. **Last interval add karna bhulna:** Loop ke baad `result.add(newInterval)` zaruri hai

2. **Case 2 mein update bhulna:** `newInterval = intervals[i]` karna bhulte hain

3. **Overlap condition galat:** `<=` ya `<` confusion - carefully check karo

4. **Edge case miss karna:** Empty array, single interval - test karo

5. **Merge logic galat:** Start mein min aur end mein max lena hai, ulta nahi

---

## Key Takeaway

**Approach:** Sorted array mein single pass karke teen cases handle karo - before, after, overlap.

**Core Logic:**
```
For each interval:
  - If current ends before new starts → Add current
  - If new ends before current starts → Add new, update new to current
  - If overlap → Merge (min start, max end)
Finally add the last interval
```
