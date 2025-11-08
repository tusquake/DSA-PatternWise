Railway station pe trains aati hain aur jaati hain.

Tumhe do arrays diye gaye hain:
- **arr[]** - Arrival times (trains kab aati hain)
- **dep[]** - Departure times (trains kab jaati hain)

**Goal:** Minimum kitne platforms chahiye taaki **koi bhi train wait na kare**?

**Important:** Agar ek train ki departure time aur dusri train ki arrival time same hai, toh platform share ho sakta hai (ek train jaate hi dusri aa sakti hai).

---

## Example

```
arr[] = [900, 940, 950, 1100, 1500, 1800]
dep[] = [910, 1200, 1120, 1130, 1900, 2000]
```

**Explanation:**
- 9:00 - Train 1 arrives → Platform 1
- 9:10 - Train 1 departs → Platform 1 free
- 9:40 - Train 2 arrives → Platform 1
- 9:50 - Train 3 arrives → Platform 2 (Train 2 abhi hai)
- 11:00 - Train 4 arrives → Platform 3 (Train 2, 3 dono hai)
- 11:20 - Train 3 departs → Platform 2 free
- 11:30 - Train 4 departs → Platform 3 free
- 12:00 - Train 2 departs → Platform 1 free

**Maximum platforms needed at any time:** 3

---

## Solution - Greedy Approach

### Code (Java):
```java
class Solution {
    int findPlatform(int arr[], int dep[], int n) {
        // Step 1: Sort both arrays
        Arrays.sort(arr);
        Arrays.sort(dep);
        
        // Step 2: Two pointers approach
        int i = 1, j = 0;  // i for arrivals, j for departures
        int platformNeeded = 1;  // Pehli train ke liye 1 platform
        int result = 1;  // Minimum result 1 hoga
        
        // Step 3: Process all events
        while (i < n && j < n) {
            if (arr[i] <= dep[j]) {
                // Train aa rahi hai, platform chahiye
                platformNeeded++;
                i++;
            } else {
                // Train ja rahi hai, platform free ho raha hai
                platformNeeded--;
                j++;
            }
            
            // Track maximum platforms needed
            result = Math.max(result, platformNeeded);
        }
        
        return result;
    }
}
```

### Code (C++):
```cpp
class Solution {
    public:
    int findPlatform(int arr[], int dep[], int n) {
        sort(arr, arr+n);
        sort(dep, dep+n);
        
        int i = 1, j = 0;
        int platform_needed = 1, result = 1;
        
        while (i < n && j < n) {
            if (arr[i] <= dep[j]) {
                platform_needed++;
                i++;
            } else {
                platform_needed--;
                j++;
            }
            result = max(result, platform_needed);
        }
        
        return result;
    }
};
```

---

## Step-by-Step Explanation

### Step 1: Dono Arrays Ko Sort Karo

```java
Arrays.sort(arr);
Arrays.sort(dep);
```

**Logic:** Idhr aisa krenge toh chronological order mein events process kar payenge - pehle jo event hoga (arrival ya departure) woh pehle handle karenge.

**Real-world analogy:** Railway station ke time table ko time ke order mein dekho, tab samajh aayega ki kab-kab rush hai.

**Example:**
```
Before sorting:
arr[] = [900, 940, 950, 1100, 1500, 1800]
dep[] = [910, 1200, 1120, 1130, 1900, 2000]

After sorting:
arr[] = [900, 940, 950, 1100, 1500, 1800]
dep[] = [910, 1120, 1130, 1200, 1900, 2000]
```

---

### Step 2: Two Pointers Initialize Karo

```java
int i = 1, j = 0;  // i for next arrival, j for next departure
int platformNeeded = 1;  // Pehli train already aa gayi
int result = 1;  // At least 1 platform chahiye hi
```

**Logic:** 
- Idhr aisa krenge toh `i` pointer next arrival track karega
- `j` pointer next departure track karega
- Pehli train (arr[0]) already platform le chuki hai, isliye i=1 se start

---

### Step 3: Events Process Karo

```java
while (i < n && j < n) {
    if (arr[i] <= dep[j]) {
        // New train aa rahi hai before any train leaves
        platformNeeded++;
        i++;
    } else {
        // Koi train ja rahi hai before next train arrives
        platformNeeded--;
        j++;
    }
    
    result = Math.max(result, platformNeeded);
}
```

**Case 1: Arrival pehle hai (arr[i] <= dep[j])**
```java
if (arr[i] <= dep[j]) {
    platformNeeded++;
    i++;
}
```

**Logic:** Idhr aisa krenge toh agar next train ki arrival, next departure se pehle ya same time pe hai, matlab **ek aur platform chahiye**. Platform count badhao aur next arrival check karo.

**Example:**
```
arr[i] = 950, dep[j] = 910
950 > 910 → Wait, this goes to else!

arr[i] = 940, dep[j] = 910
940 > 910 → else case

arr[i] = 950, dep[j] = 1120
950 <= 1120 → Platform needed++
```

---

**Case 2: Departure pehle hai (arr[i] > dep[j])**
```java
else {
    platformNeeded--;
    j++;
}
```

**Logic:** Idhr aisa krenge toh agar next departure, next arrival se pehle hai, matlab **ek platform free ho raha hai**. Platform count ghatao aur next departure check karo.

---

**Track Maximum Platforms:**
```java
result = Math.max(result, platformNeeded);
```

**Logic:** Har step pe check karo ki current platforms needed, ab tak ka maximum se zyada hai ya nahi. Maximum wala value hi final answer hoga.

---

## Dry Run Example

**Input:**
```
arr[] = [900, 940, 950, 1100, 1500, 1800]
dep[] = [910, 1200, 1120, 1130, 1900, 2000]
```

### After Sorting:
```
arr[] = [900, 940, 950, 1100, 1500, 1800]
dep[] = [910, 1120, 1130, 1200, 1900, 2000]
```

### Processing:

| Step | i | j | arr[i] | dep[j] | Condition | platformNeeded | result | Explanation |
|------|---|---|--------|--------|-----------|----------------|--------|-------------|
| Init | 1 | 0 | 940 | 910 | - | 1 | 1 | Pehli train already arrived |
| 1 | 1 | 0 | 940 | 910 | 940 > 910 | 0 | 1 | Train departs (j++, platform--) |
| 2 | 1 | 1 | 940 | 1120 | 940 <= 1120 | 1 | 1 | Train arrives (i++, platform++) |
| 3 | 2 | 1 | 950 | 1120 | 950 <= 1120 | 2 | 2 | Train arrives (i++, platform++) |
| 4 | 3 | 1 | 1100 | 1120 | 1100 <= 1120 | 3 | 3 | Train arrives (i++, platform++) |
| 5 | 4 | 1 | 1500 | 1120 | 1500 > 1120 | 2 | 3 | Train departs (j++, platform--) |
| 6 | 4 | 2 | 1500 | 1130 | 1500 > 1130 | 1 | 3 | Train departs (j++, platform--) |
| 7 | 4 | 3 | 1500 | 1200 | 1500 > 1200 | 0 | 3 | Train departs (j++, platform--) |
| 8 | 4 | 4 | 1500 | 1900 | 1500 <= 1900 | 1 | 3 | Train arrives (i++, platform++) |
| 9 | 5 | 4 | 1800 | 1900 | 1800 <= 1900 | 2 | 3 | Train arrives (i++, platform++) |

**Loop ends (i >= n)**

**Answer:** `result = 3` platforms needed

---

## Visual Timeline

```
Time:     900   940   950  1100  1120  1130  1200  1500  1800  1900  2000
          |     |     |    |     |     |     |     |     |     |     |

Platform 1: [====Train1====]
                   [============Train2=================]

Platform 2:              [========Train3========]

Platform 3:                   [====Train4====]

Platform 1:                                         [======Train5========]

Platform 2:                                                [====Train6=====]

Peak time: Around 1100, 3 platforms occupied simultaneously!
```

---

## Why This Approach Works?

**Greedy Logic:** Idhr aisa krenge toh events ko chronologically process karenge aur current platforms track karenge.

**Key Insight:**
- Arrivals aur departures ko separately sort karne se hum efficiently events process kar sakte hain
- Arrival matlab platform occupied
- Departure matlab platform free
- Jo bhi event pehle ho (arrival ya departure), use pehle handle karo

**Real-world analogy:** Station master ki duty jaisa - jab train aati hai toh platform assign karo, jab jaati hai toh free kar do. Peak time pe maximum platforms used honge, wahi answer hai.

---

## Why Sort Separately?

**Question:** Dono arrays alag kyun sort karte hain? Ek saath pair ke saath kyun nahi?

**Answer:** Kyunki hume actual train ki mapping nahi chahiye, sirf **events ka sequence** chahiye.

**Example:**
```
Train A: arr=900, dep=1200
Train B: arr=940, dep=910 (Wait, dep before arr? No!)

Actually:
Train A: arr=900, dep=1200 (stays long)
Train B: arr=940, dep=910 (leaves quickly - maybe typo in problem)

But in algorithm:
We only care: At 900 arrival, at 910 departure, at 940 arrival, at 1200 departure
Train identity doesn't matter!
```

Idhr aisa krenge toh events ko independent treat karke efficiently count kar sakte hain.

---

## Complexity Analysis

- **Time Complexity:** `O(n log n)` 
  - Sorting arr[]: O(n log n)
  - Sorting dep[]: O(n log n)
  - Two pointer traversal: O(n)
  - Total: O(n log n)
  
- **Space Complexity:** `O(1)` 
  - In-place sorting (ignoring sorting space)
  - Only constant extra variables

---

## Edge Cases

1. **Single train:**
   ```
   arr[] = [900], dep[] = [910]
   ```
   → Answer: 1 platform

2. **All trains same time:**
   ```
   arr[] = [900, 900, 900], dep[] = [910, 910, 910]
   ```
   → Answer: 3 platforms (sab ek saath)

3. **No overlap:**
   ```
   arr[] = [900, 1000, 1100], dep[] = [950, 1050, 1150]
   ```
   → Answer: 1 platform (trains sequential hain)

4. **Complete overlap:**
   ```
   arr[] = [900, 910, 920], dep[] = [2000, 2010, 2020]
   ```
   → Answer: 3 platforms (sab trains lambe time tak rukti hain)

5. **Arrival equals departure:**
   ```
   arr[i] = 900, dep[j] = 900
   ```
   → arr[i] <= dep[j] condition true, platform needed++ (conservative approach)

---

## Interview Tips

1. **Sorting explain karo:** "Idhr aisa krenge toh dono arrays ko separately sort karke chronological order mein events process karenge"

2. **Two pointer logic:** "Arrival matlab platform chahiye, departure matlab platform free. Jo event pehle ho use pehle handle karo"

3. **Why not pair sorting:** "Train identity important nahi hai, sirf events ka sequence chahiye"

4. **Edge case mention:** "Agar arrival aur departure same time hai toh platform share ho sakta hai, but algorithm mein conservative approach le rahe hain"

5. **Alternative approaches:**
   - Brute force: Har time point pe count karo kitni trains present hain - O(n × max_time)
   - Events array: Arrival ko +1 aur departure ko -1 mark karke sort - similar approach
   - Interval merging: Overlapping intervals count karo - more complex

6. **Follow-up questions:**
   - Platform assignment bhi return karni hai? (Need to track train IDs)
   - Multiple stations? (Different problem - interval scheduling)
   - Platform preference? (Add priority logic)

---

## Common Mistakes

1. **Pairs ke saath sort karna:** Train mapping maintain karne ki zarurat nahi, events separately sort karo

2. **i aur j initialization galat:** i=1 se start (pehli train already counted), j=0 se start

3. **Condition galat:** arr[i] <= dep[j] (equal case important - same time pe platform share ho sakta hai)

4. **Result update bhulna:** Har iteration pe max() se result update karo

5. **Loop condition:** while (i < n && j < n) - dono pointers ke bounds check karo

---