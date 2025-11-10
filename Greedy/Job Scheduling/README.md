Tumhare paas n jobs hain. Har job ka:
- **id** - Job ka unique identifier
- **deadline** - Kitne din mein complete karna hai
- **profit** - Job complete karne pe kitna profit milega

**Rules:**
- Har job ko complete hone mein **1 unit time** lagta hai
- Ek time pe sirf **ek job** kar sakte ho
- Job ko uski **deadline se pehle ya deadline pe** complete karna hai

**Goal:** Maximum profit kaise kamaya jaye aur kitni jobs complete hongi?

---

## Example

```
Jobs:
Job 1: deadline=4, profit=20
Job 2: deadline=1, profit=10
Job 3: deadline=1, profit=40
Job 4: deadline=1, profit=30
```

**Optimal Solution:**
- Day 1: Job 3 karo (profit=40, deadline=1)
- Day 2: Job 1 karo (profit=20, deadline=4)
- Job 2 aur Job 4 skip (kyunki deadline 1 thi aur slot occupied hai)

**Answer:** 2 jobs, Total profit = 60

---

## Solution - Greedy Approach

### Code (Java):
```java
class Solution {
    int[] JobScheduling(Job arr[], int n) {
        // Step 1: Sort by profit (descending - highest profit first)
        Arrays.sort(arr, (a, b) -> b.profit - a.profit);
        
        // Step 2: Track which time slots are occupied
        boolean[] done = new boolean[n];
        
        // Step 3: Schedule jobs greedily
        int jobCount = 0;
        int totalProfit = 0;
        
        for (int i = 0; i < n; i++) {
            // Try to schedule job as late as possible before deadline
            for (int j = Math.min(n - 1, arr[i].dead - 1); j >= 0; j--) {
                if (!done[j]) {
                    // Slot available, schedule this job
                    jobCount++;
                    totalProfit += arr[i].profit;
                    done[j] = true;
                    break;
                }
            }
        }
        
        return new int[]{jobCount, totalProfit};
    }
}
```

### Code (C++):
```cpp
class Solution {
    public:
    static bool comp(Job a, Job b) {
        return a.profit > b.profit;
    }
    
    vector<int> JobScheduling(Job arr[], int n) {
        // Sort by profit (highest first)
        sort(arr, arr+n, comp);
        
        // Track occupied slots
        bool done[n] = {0};
        
        int day = 0, profit = 0;
        
        for (int i = 0; i < n; i++) {
            // Try to fit job in latest possible slot before deadline
            for (int j = min(n, arr[i].dead - 1); j >= 0; j--) {
                if (done[j] == false) {
                    day += 1;
                    profit += arr[i].profit;
                    done[j] = true;
                    break;
                }
            }
        }
        
        return {day, profit};
    }
};
```

---

## Step-by-Step Explanation

### Step 1: Sort by Profit (Descending Order)

```java
Arrays.sort(arr, (a, b) -> b.profit - a.profit);
```

**Logic:** Idhr aisa krenge toh sabse zyada profit wali job pehle schedule karenge. Greedy choice hai - maximize profit.

**Real-world analogy:** Agar tumhe multiple freelance projects mile hain, toh sabse zyada payment wala project pehle complete karoge.

**Example:**
```
Before sorting:
Job 1: deadline=4, profit=20
Job 2: deadline=1, profit=10
Job 3: deadline=1, profit=40
Job 4: deadline=1, profit=30

After sorting by profit:
Job 3: deadline=1, profit=40  (highest)
Job 4: deadline=1, profit=30
Job 1: deadline=4, profit=20
Job 2: deadline=1, profit=10  (lowest)
```

---

### Step 2: Track Occupied Time Slots

```java
boolean[] done = new boolean[n];
```

**Logic:** Idhr aisa krenge toh har time slot ko track karenge - occupied hai ya free.

**Array indexing:**
- `done[0]` = Day 1 ka slot
- `done[1]` = Day 2 ka slot
- `done[j]` = Day (j+1) ka slot

**Real-world analogy:** Calendar jaisa - har din pe ek hi job schedule kar sakte ho.

---

### Step 3: Schedule Jobs Greedily

```java
for (int i = 0; i < n; i++) {
    // Try to schedule job as late as possible before deadline
    for (int j = Math.min(n - 1, arr[i].dead - 1); j >= 0; j--) {
        if (!done[j]) {
            jobCount++;
            totalProfit += arr[i].profit;
            done[j] = true;
            break;
        }
    }
}
```

**Outer Loop:** Har job ko consider karo (profit order mein)

**Inner Loop:** Job ko deadline se pehle **latest possible slot** mein fit karo

---

### Why Latest Possible Slot?

```java
for (int j = Math.min(n - 1, arr[i].dead - 1); j >= 0; j--)
```

**Logic:** Idhr aisa krenge toh job ko deadline ke jitna paas ho sake schedule karenge, taaki early slots baaki jobs ke liye available rahein.

**Example:**
```
Job: deadline=4, profit=50

Slots available: [0, 1, 2, 3] (Days 1, 2, 3, 4)

Try: slot 3 (Day 4) → Available? Yes! Schedule here
Why? Kyunki Day 1, 2, 3 ab baaki jobs ke liye free hain
```

**Real-world analogy:** Assignment submission jaisa - agar deadline 5 days hai, toh 5th day ko submit karoge taaki baaki kaam bhi kar sako.

---

### Breaking Down the Inner Loop

```java
for (int j = Math.min(n - 1, arr[i].dead - 1); j >= 0; j--)
```

**Starting point:** `min(n-1, arr[i].dead - 1)`
- `arr[i].dead - 1` = Job ki deadline se ek din pehle (0-indexed)
- `n - 1` = Maximum possible slot
- Dono mein se chhota wala lo

**Why min?**
- Agar deadline = 10 but sirf 5 jobs hain
- Toh slot 9 kaam ka nahi (out of bounds)
- Isliye min(4, 9) = 4

**Direction:** `j >= 0` (right to left, deadline se backwards)
- Latest slot se start karo
- Pehla free slot mila toh schedule kar do

---

### Scheduling Logic

```java
if (!done[j]) {
    jobCount++;
    totalProfit += arr[i].profit;
    done[j] = true;
    break;
}
```

**Agar slot free hai:**
1. Job count badhao
2. Profit add karo
3. Slot ko occupied mark karo
4. Break - is job ka kaam ho gaya

**Agar koi slot nahi mila?** Job skip ho jayegi (profit nahi milega)

---

## Dry Run Example

**Input:**
```
Job A: deadline=4, profit=20
Job B: deadline=1, profit=10
Job C: deadline=1, profit=40
Job D: deadline=1, profit=30
```

### Step 1: Sort by profit
```
Job C: deadline=1, profit=40
Job D: deadline=1, profit=30
Job A: deadline=4, profit=20
Job B: deadline=1, profit=10
```

### Step 2: Initialize
```
done = [false, false, false, false]  (4 slots: Day 1, 2, 3, 4)
jobCount = 0, totalProfit = 0
```

### Step 3: Process jobs

**Job C (deadline=1, profit=40):**
```
Try slots: min(3, 1-1) = 0 to 0
j=0: done[0]=false → Schedule here!
done = [true, false, false, false]
jobCount = 1, totalProfit = 40
```

**Job D (deadline=1, profit=30):**
```
Try slots: min(3, 1-1) = 0 to 0
j=0: done[0]=true → Occupied!
No slot available → Skip this job
done = [true, false, false, false]
jobCount = 1, totalProfit = 40
```

**Job A (deadline=4, profit=20):**
```
Try slots: min(3, 4-1) = 3 to 0
j=3: done[3]=false → Schedule here!
done = [true, false, false, true]
jobCount = 2, totalProfit = 60
```

**Job B (deadline=1, profit=10):**
```
Try slots: min(3, 1-1) = 0 to 0
j=0: done[0]=true → Occupied!
No slot available → Skip this job
done = [true, false, false, true]
jobCount = 2, totalProfit = 60
```

**Final Answer:** [2, 60]
- 2 jobs completed
- Total profit = 60

---

## Visual Timeline

```
Days:     1     2     3     4
Slots:   [0]   [1]   [2]   [3]

Job C (dl=1, p=40): ████ at Day 1 [slot 0]
Job D (dl=1, p=30): ❌ No slot (Day 1 occupied)
Job A (dl=4, p=20):                   ████ at Day 4 [slot 3]
Job B (dl=1, p=10): ❌ No slot (Day 1 occupied)

Final Schedule:
Day 1: Job C (profit=40)
Day 2: Free
Day 3: Free
Day 4: Job A (profit=20)

Total: 2 jobs, 60 profit
```

---

## Why Greedy Works Here?

**Greedy Choice:** Sabse zyada profit wali job pehle schedule karo, deadline ke paas wale slot mein.

**Why optimal?**
1. High profit jobs ko priority do
2. Late slots use karo taaki early slots available rahein
3. Low profit jobs automatically filter out ho jayengi agar slots na mile

**Proof by contradiction:**
- Agar low profit job ko pehle schedule karoge
- Toh high profit job ke liye slot nahi milega
- Total profit kam ho jayega

**Real-world analogy:** Company mein important projects pehle complete karte hain, less important baad mein ya skip kar dete hain.

---

## Complexity Analysis

- **Time Complexity:** `O(n^2)`
    - Sorting: O(n log n)
    - Outer loop: O(n)
    - Inner loop: O(n) worst case (har job ke liye n slots check)
    - Total: O(n^2)

- **Space Complexity:** `O(n)`
    - done[] array for tracking slots

**Optimization possible:** Disjoint Set Union (DSU) use karke O(n log n) mein kar sakte ho, but complexity badhta hai.

---

## Edge Cases

1. **Single job:**
   ```
   Job 1: deadline=1, profit=100
   ```
   → Answer: [1, 100]

2. **All jobs same deadline:**
   ```
   Job 1: deadline=1, profit=20
   Job 2: deadline=1, profit=30
   Job 3: deadline=1, profit=40
   ```
   → Only highest profit job (Job 3) scheduled → [1, 40]

3. **No deadline constraint (all deadlines >= n):**
   ```
   3 jobs, all deadlines = 5
   ```
   → Sab jobs schedule ho jayengi

4. **Zero profit jobs:**
   ```
   Job 1: deadline=2, profit=0
   ```
   → Still count hoga but profit 0

5. **Large deadlines:**
   ```
   Job 1: deadline=100, profit=50
   But only 3 jobs total
   ```
   → min(n-1, deadline-1) = min(2, 99) = 2 (slot 2 use hoga)

---

## Interview Tips

1. **Sorting strategy explain karo:** "Idhr aisa krenge toh sabse zyada profit wali jobs pehle consider karenge, greedy approach hai"

2. **Latest slot logic:** "Job ko deadline ke jitna paas schedule karenge taaki early slots baaki jobs ke liye free rahein"

3. **Array indexing:** "done[j] array mein j=0 means Day 1, j=1 means Day 2 (0-indexed)"

4. **Time complexity trade-off:** "O(n^2) solution simple hai, DSU se O(n log n) possible but implementation complex"

5. **Alternative approaches:**
    - Disjoint Set Union (DSU): Find next available slot efficiently
    - Priority Queue: Different approach, similar complexity
    - Dynamic Programming: Overkill for this problem

6. **Follow-up questions:**
    - Jobs ko actual schedule bhi return karna hai? (Track job IDs in done array)
    - Multiple machines available hain? (Different problem - parallel job scheduling)
    - Dependencies between jobs? (Topological sort needed)

---

## Common Mistakes

1. **Sorting by deadline:** Wrong! Profit se sort karna hai, deadline constraint baad mein check karni hai

2. **Earliest slot choose karna:** Wrong! Latest available slot choose karo taaki flexibility rahe

3. **Array size:** done[n] size enough hai usually, but agar max deadline > n toh problem ho sakti hai

4. **Index confusion:** deadline=1 means slot 0 (0-indexed), deadline=4 means slot 3

5. **Break statement bhulna:** Ek slot mila toh break karo, warna duplicate counting ho jayegi

6. **min() function purpose:** Deadline aur array bounds dono check karne ke liye

---

## Real-World Applications

1. **Task scheduling in OS:** High priority tasks pehle schedule karo

2. **Project management:** High profit projects deadline se pehle complete karo

3. **Resource allocation:** Limited resources ko maximum profit ke liye use karo

4. **Interview scheduling:** Multiple candidates, limited time slots

5. **Delivery scheduling:** High value orders pehle deliver karo

---

## Key Takeaway

**Greedy Strategy:** Sort by profit (highest first), schedule in latest available slot before deadline.

**Core Logic:**
```
1. Sort jobs by profit (descending)
2. For each job (highest profit first):
   - Try to fit in latest slot before deadline
   - If slot found: schedule and mark occupied
   - Else: skip this job
3. Return jobs count and total profit
```

