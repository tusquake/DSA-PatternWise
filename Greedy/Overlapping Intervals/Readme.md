## Problem Kya Hai?

Tumhe ek array of intervals diya gaya hai.

**Goal:** **Minimum kitne intervals remove** karne padenge taaki baki sab intervals **non-overlapping** ho jayein?

**Input:** `intervals[][]` - Array of intervals [start, end]

**Output:** Minimum number of intervals to remove

---

## Example

### Example 1:
```
Input: intervals = [[1,2], [2,3], [3,4], [1,3]]
Output: 1

Explanation:
[1,3] ko remove karo, baki sab non-overlapping hain:
[1,2], [2,3], [3,4] - ye sab overlap nahi karte
```

### Example 2:
```
Input: intervals = [[1,2], [1,2], [1,2]]
Output: 2

Explanation:
Sirf ek [1,2] rakh sakte ho, baki 2 remove karne padenge
```

### Example 3:
```
Input: intervals = [[1,2], [2,3]]
Output: 0

Explanation:
Already non-overlapping hain, koi remove nahi karna
```

---

## Solution - Greedy Approach (Activity Selection)

### Code (Java):
```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length < 2) return 0;
        
        // Sort by end time (ascending)
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
        
        int count = 0;
        int end = intervals[0][1];  // First interval ka end time
        
        for (int i = 1; i < intervals.length; i++) {
            // Overlap hai
            if (intervals[i][0] < end) {
                count++;  // Current interval remove karna padega
            }
            // No overlap
            else {
                end = intervals[i][1];  // Update end time
            }
        }
        
        return count;
    }
}
```

### Code (C++):
```cpp
bool comp(vector<int>& a, vector<int>& b) {
    return a[1] < b[1];
}

int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if (intervals.size() < 2) return 0;
    
    sort(intervals.begin(), intervals.end(), comp);
    
    int cnt = 0, end = intervals[0][1];
    
    for (int i = 1; i < intervals.size(); i++) {
        if (intervals[i][0] < end) {
            cnt++;
        } else {
            end = intervals[i][1];
        }
    }
    
    return cnt;
}
```

---

## Step-by-Step Explanation

### Step 1: Sort by End Time

```java
Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
```

**Logic:** Idhr aisa krenge toh intervals ko **end time** ke basis pe sort karenge (ascending order - jo pehle khatam ho woh pehle).

**Why end time?** Kyunki jo interval **jaldi khatam** hoga, woh baaki intervals ke liye zyada jagah chhod dega!

**Real-world analogy:** Meetings schedule karte time jo meeting pehle khatam ho, use pehle karo. Isse baad mein zyada meetings fit ho jayengi.

**Example:**
```
Before sorting:
[[1,2], [2,3], [3,4], [1,3]]

After sorting by end time:
[[1,2], [2,3], [1,3], [3,4]]
  end=2  end=3  end=3  end=4
```

---

### Step 2: Initialize Variables

```java
int count = 0;          // Kitne intervals remove karne hain
int end = intervals[0][1];  // Pehle interval ka end time
```

**Logic:** 
- Idhr aisa krenge toh `count` se track karenge kitne intervals remove kiye
- `end` mein last **selected** interval ka end time store karenge
- Pehla interval hamesha select hoga (kyunki sabse pehle khatam hota hai after sorting)

---

### Step 3: Check Overlaps

```java
for (int i = 1; i < intervals.length; i++) {
    if (intervals[i][0] < end) {
        count++;  // Overlap hai, remove karo
    } else {
        end = intervals[i][1];  // No overlap, select karo
    }
}
```

**Case 1: Overlap Hai**
```java
if (intervals[i][0] < end) {
    count++;
}
```

**Condition:** Current interval ka **start** < last selected interval ka **end**

**Logic:** Idhr aisa krenge toh overlap detect hua. Current interval ko **remove** karna padega.

**Visual:**
```
Selected:  [1====2]
Current:       [1.5====3]
               ^
               Start (1.5) < End (2) → Overlap!
               
Remove current, keep selected (kyunki woh pehle khatam hota hai)
```

**Important:** Hum current ko remove karte hain, selected ko nahi! Kyunki selected pehle khatam ho raha hai (sorted hai end time se).

**Real-world analogy:** Do overlapping meetings hain - ek 9-10am, dusri 9:30-11am. Pehli wali chhoti hai toh use rakho, lambi wali ko cancel karo.

---

**Case 2: No Overlap**
```java
else {
    end = intervals[i][1];
}
```

**Logic:** Idhr aisa krenge toh current interval ko **select** kar lenge aur `end` update kar denge.

**Visual:**
```
Selected:  [1====2]
Current:           [3====4]
                   ^
                   Start (3) >= End (2) → No overlap!
                   
Select current, update end to 4
```

---

## Dry Run Example

**Input:** `intervals = [[1,2], [2,3], [3,4], [1,3]]`

### Step 1: Sort by end time
```
[[1,2], [2,3], [1,3], [3,4]]
  end=2  end=3  end=3  end=4
```

### Step 2: Initialize
```
count = 0
end = 2  (intervals[0][1])
```

### Step 3: Process intervals

**Iteration 1: [2,3]**
```
Check: intervals[1][0] < end → 2 < 2? ❌
No overlap! Select this interval
end = 3
count = 0
```

**Iteration 2: [1,3]**
```
Check: intervals[2][0] < end → 1 < 3? ✅
Overlap! Remove this interval
count = 1
end = 3 (unchanged)
```

**Iteration 3: [3,4]**
```
Check: intervals[3][0] < end → 3 < 3? ❌
No overlap! Select this interval
end = 4
count = 1
```

**Final Answer:** `count = 1`

**Explanation:** 
- Selected: [1,2], [2,3], [3,4]
- Removed: [1,3]

---

## Visual Timeline

```
Original intervals (sorted by end time):
[1===2]  [2===3]  [1=======3]  [3===4]

Processing:

Step 1: Select [1,2]
[1===2]✅

Step 2: Check [2,3]
[1===2]✅ [2===3]✅
Start (2) >= End (2) → Select!

Step 3: Check [1,3]
[1===2]✅ [2===3]✅
         [1=======3]❌
Start (1) < End (3) → Remove!

Step 4: Check [3,4]
[1===2]✅ [2===3]✅ [3===4]✅
Start (3) >= End (3) → Select!

Final: 1 interval removed
Selected: [1===2] [2===3] [3===4]
```

---

## Why This Greedy Approach Works?

**Greedy Choice:** Sort by end time aur jo interval pehle khatam ho use select karo.

**Proof:**
1. Jo interval **jaldi khatam** hota hai, use select karne se baaki intervals ke liye zyada space bachta hai
2. Agar lambe interval ko select karoge toh chote intervals overlap kar jayenge
3. Result: Zyada intervals remove karne padenge

**Example:**
```
Wrong choice (select longer):
[1=======5]
  [2=3] [4=5]  ← Dono remove karne padenge (2 removals)

Right choice (select shorter):
  [2=3]✅ [4=5]✅
[1=======5]❌  ← Sirf ye remove karna padega (1 removal)
```

**Real-world analogy:** Movie marathon mein chhoti movies pehle dekho, tab ek din mein zyada movies fit ho jayengi!

---

## Why Sort by End Time, Not Start Time?

**Question:** Start time se sort kyun nahi karte?

**Answer:** Kyunki end time se sort karne pe jo **jaldi khatam** ho woh pehle aayega, aur woh optimal choice hai.

**Counter-example with start time sorting:**
```
Intervals: [[1,10], [2,3], [4,5]]

Sort by start time:
[[1,10], [2,3], [4,5]]

Process:
- Select [1,10]
- [2,3] overlaps → Remove (count=1)
- [4,5] overlaps → Remove (count=2)
Total: 2 removals ❌

Sort by end time:
[[2,3], [4,5], [1,10]]

Process:
- Select [2,3]
- [4,5] no overlap → Select
- [1,10] overlaps → Remove (count=1)
Total: 1 removal ✅ (Optimal!)
```

Idhr aisa krenge toh end time sorting optimal answer deti hai!

---

## Complexity Analysis

- **Time Complexity:** `O(n log n)` 
  - Sorting: O(n log n)
  - Single pass through intervals: O(n)
  - Total: O(n log n) (sorting dominates)
  
- **Space Complexity:** `O(1)` 
  - Only constant extra variables
  - Sorting in-place (ignoring sort space)

---

## Edge Cases

1. **Less than 2 intervals:**
   ```
   intervals = [[1,2]]
   Output: 0 (Already non-overlapping)
   ```

2. **All intervals identical:**
   ```
   intervals = [[1,2], [1,2], [1,2]]
   Output: 2 (Keep one, remove rest)
   ```

3. **No overlaps:**
   ```
   intervals = [[1,2], [3,4], [5,6]]
   Output: 0
   ```

4. **Complete overlaps:**
   ```
   intervals = [[1,5], [2,3], [3,4]]
   Output: 1 (Remove [1,5] or keep [1,5] and remove both others)
   
   After sorting by end: [[2,3], [3,4], [1,5]]
   Select [2,3], [3,4], remove [1,5] → 1 removal ✅
   ```

5. **Touching intervals:**
   ```
   intervals = [[1,2], [2,3]]
   Output: 0 (Not overlapping - end of one equals start of other)
   ```
   **Note:** `intervals[i][0] < end` condition doesn't include equality, so touching is allowed!

---

## Interview Tips

1. **Greedy strategy explain karo:** "Idhr aisa krenge toh end time se sort karke jo jaldi khatam ho use select karenge, isse minimum removals honge"

2. **Why end time sorting:** "Jo interval pehle khatam hoga use select karne se baaki intervals ke liye zyada jagah bachegi"

3. **Activity selection connection:** "Ye activity selection problem ka variation hai - maximum activities select karo, unke alawa sab remove karna padega"

4. **Touching intervals:** "Agar ek ka end dusre ka start hai toh overlap nahi, allowed hai"

5. **Alternative approaches:**
   - Interval merging: Count merged intervals, subtract from total (more complex)
   - DP: Possible but O(n²), greedy is better
   - Start time sorting: Wrong! Counter-example se prove kar sakte ho

6. **Follow-up questions:**
   - Which intervals to remove? (Track indices during process)
   - Maximum non-overlapping intervals? (Same logic, but return selected count)
   - Weighted intervals? (Different problem - DP needed)

---

## Relation to Other Problems

**Similar Problems:**

1. **Activity Selection:** Maximum activities select karo
   - Same approach - sort by end time
   - Return selected count instead of removed count

2. **Meeting Rooms:** Minimum rooms chahiye?
   - Different problem - uses arrival/departure arrays

3. **Merge Intervals:** Overlapping intervals ko merge karo
   - Sort by start time, then merge
   - Different from this problem

**Formula:**
```
Activities to remove = Total activities - Maximum non-overlapping activities
```

---

## Common Mistakes

1. **Start time se sort karna:** Wrong approach, counter-example se disprove ho jata hai

2. **Touching intervals ko overlap maan lena:** `[1,2]` aur `[2,3]` overlap nahi hain (condition `<` hai, `<=` nahi)

3. **Selected interval track na karna:** End variable update karna bhulte hain no-overlap case mein

4. **Edge case miss karna:** Single interval ya empty array check karo

5. **Greedy justification:** Interview mein explain karo ki why greedy works (smaller end time = more space for others)

---

## Key Takeaway

**Greedy Strategy:** Sort by **end time** (jo pehle khatam ho), then greedily select non-overlapping intervals. Baki sab remove karne padenge.

**Core Logic:**
```
1. Sort by end time (ascending)
2. Select first interval (ends earliest)
3. For remaining intervals:
   - If overlaps with selected → Remove (count++)
   - Else → Select (update end)
4. Return count (removed intervals)
```
