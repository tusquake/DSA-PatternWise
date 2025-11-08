# N Meetings in One Room - Interview Revision Notes

## Problem Kya Hai?

Tumhare paas **ek hi meeting room** hai aur **n meetings** schedule karni hain.

Har meeting ka:
- **Start time** hai
- **End time** hai

**Goal:** Maximum kitni meetings kar sakte ho ek room mein bina overlap kiye?

**Note:** Ek time pe sirf **ek hi meeting** ho sakti hai room mein!

---

## Solution - Activity Selection (Greedy)

### Code:
```java
class Solution {
    public int maxMeetings(int start[], int end[], int n) {
        // Step 1: Create array of pairs (meetings)
        int[][] meetings = new int[n][2];
        for (int i = 0; i < n; i++) {
            meetings[i][0] = start[i];  // start time
            meetings[i][1] = end[i];    // end time
        }
        
        // Step 2: Sort by end time
        Arrays.sort(meetings, (a, b) -> a[1] - b[1]);
        
        // Step 3: Count maximum meetings
        int count = 1;  // Pehli meeting toh hogi hi
        int ansEnd = meetings[0][1];  // Last meeting ka end time
        
        for (int i = 1; i < n; i++) {
            // Agar current meeting ka start > last meeting ka end
            if (meetings[i][0] > ansEnd) {
                count++;
                ansEnd = meetings[i][1];  // Update end time
            }
        }
        
        return count;
    }
}
```

---

## Step-by-Step Explanation

### Step 1: Meetings Ko Store Karo

```java
int[][] meetings = new int[n][2];
for (int i = 0; i < n; i++) {
    meetings[i][0] = start[i];  // start time
    meetings[i][1] = end[i];    // end time
}
```

**Logic:** Har meeting ke start aur end time ko ek saath rakhna hai taaki sorting ke baad bhi connection ban sake.

**Example:**
```
start = [1, 3, 0, 5]
end   = [2, 4, 6, 9]

meetings = [[1,2], [3,4], [0,6], [5,9]]
```

---

### Step 2: Sort by End Time (CRITICAL!)

```java
Arrays.sort(meetings, (a, b) -> a[1] - b[1]);
```

**Logic:** Jo meeting **jaldi khatam** hogi, use **pehle** select karenge!

**Why?** Kyunki jaldi khatam hone se baaki meetings ke liye zyada time bachega!

**After sorting:**
```
meetings = [[1,2], [3,4], [0,6], [5,9]]
```
(Already sorted by end time in this example)

---

### Step 3: Greedy Selection

```java
int count = 1;  // Pehli meeting (jo sabse pehle end hoti hai) select kar li
int ansEnd = meetings[0][1];  // Pehli meeting ka end time track karo
```

**Logic:** Sabse pehle ending wali meeting toh select karni hi karni hai! Starting count = 1

---

### Step 4: Remaining Meetings Check Karo

```java
for (int i = 1; i < n; i++) {
    if (meetings[i][0] > ansEnd) {  // No overlap?
        count++;
        ansEnd = meetings[i][1];  // Update last meeting ka end time
    }
}
```

**Logic:**
- Agar **current meeting ka start time** > **last selected meeting ka end time**
- Matlab **no overlap** hai! ✅
- Toh is meeting ko select karo aur count badhao
- `ansEnd` update karo new meeting ke end time se

**Agar overlap hai?**
- Simply skip karo (greedy choice - jo pehle khatam hui woh better hai)

---

## Dry Run Example

**Input:**
```
start = [1, 3, 0, 5, 8, 5]
end   = [2, 4, 6, 7, 9, 9]
```

### Step 1: Create meetings array
```
meetings = [[1,2], [3,4], [0,6], [5,7], [8,9], [5,9]]
```

### Step 2: Sort by end time
```
meetings = [[1,2], [3,4], [0,6], [5,7], [8,9], [5,9]]
                                  ↑ (sorted)
Sorted:    [[1,2], [3,4], [0,6], [5,7], [8,9], [5,9]]
```

### Step 3 & 4: Select meetings

| i | Meeting | Start | End | ansEnd | Start > ansEnd? | Action |
|---|---------|-------|-----|--------|-----------------|--------|
| 0 | [1,2] | 1 | 2 | 2 | - | Selected (count=1) |
| 1 | [3,4] | 3 | 4 | 2 | 3 > 2 ✅ | Selected (count=2, ansEnd=4) |
| 2 | [0,6] | 0 | 6 | 4 | 0 > 4 ❌ | Skip (overlap) |
| 3 | [5,7] | 5 | 7 | 4 | 5 > 4 ✅ | Selected (count=3, ansEnd=7) |
| 4 | [8,9] | 8 | 9 | 7 | 8 > 7 ✅ | Selected (count=4, ansEnd=9) |
| 5 | [5,9] | 5 | 9 | 9 | 5 > 9 ❌ | Skip (overlap) |

**Answer:** `count = 4` meetings! 🎉

**Selected meetings:** [1,2], [3,4], [5,7], [8,9]

---

## Visual Timeline

```
Time:    0  1  2  3  4  5  6  7  8  9
         |--|--|--|--|--|--|--|--|--|
[1,2]       ✅✅                         Selected
[3,4]             ✅✅                   Selected
[0,6]    ❌❌❌❌❌❌❌                 Overlaps with [1,2]
[5,7]                   ✅✅✅           Selected
[8,9]                          ✅✅      Selected
[5,9]                   ❌❌❌❌❌      Overlaps with [5,7]
```

---

## Why Greedy Works Here?

**Greedy Choice:** Jo meeting **sabse jaldi khatam** ho rahi hai, use select karo

**Proof:**
1. Agar hum koi aur meeting select karte (jo late end hoti)
2. Toh baaki meetings ke liye kam time bachta
3. Result: Kam meetings ho paati

**Example:**
```
Meeting A: [1, 5]
Meeting B: [1, 3]
Meeting C: [4, 6]

Agar A select kiya → C nahi ho sakti (overlap)
Agar B select kiya → C bhi ho sakti hai! (B pehle end hoti hai)
```

**Conclusion:** Jo **jaldi khatam** ho, woh **optimal** hai!

---

## Complexity Analysis

- **Time Complexity:** `O(n log n)`
    - Sorting: `O(n log n)`
    - Loop: `O(n)`
    - Total: `O(n log n)` (sorting dominates)

- **Space Complexity:** `O(n)`
    - 2D array banaya meetings store karne ke liye

---

## Edge Cases

1. **Ek hi meeting:**
   ```
   start = [1], end = [2]
   ```
   → Answer: `1` ✅

2. **Koi bhi meeting overlap nahi:**
   ```
   start = [1, 3, 5]
   end   = [2, 4, 6]
   ```
   → Answer: `3` (sabhi select ho jayengi) ✅

3. **Sab meetings same time:**
   ```
   start = [1, 1, 1]
   end   = [2, 2, 2]
   ```
   → Answer: `1` (sirf ek hi ho sakti hai) ✅

4. **Nested meetings:**
   ```
   start = [1, 2, 3]
   end   = [10, 5, 4]
   ```
   → Sort by end: [[3,4], [2,5], [1,10]]
   → Answer: `2` ([3,4] and [2,5] but wait... overlap!)
   → Actually: `1` (sirf [3,4]) ✅

---