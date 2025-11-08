Tumhare paas:
- **Ek knapsack (bag)** hai jiska **maximum capacity W** hai
- **n items** hain, har item ka:
    - **value** (kitna valuable hai)
    - **weight** (kitna bhari hai)

**Goal:** Maximum value kitni le ja sakte ho knapsack mein?

**TWIST:** Items ko **fraction mein** (tukdo mein) le ja sakte ho!

Example: Agar 10kg ka item hai, toh 5kg ya 7.5kg bhi le sakte ho!

---

## Solution - Greedy Approach

### Code:
```java
class Solution {
    double fractionalKnapsack(int W, Item arr[], int n) {
        // Step 1: Calculate value per weight ratio
        double[][] items = new double[n][3];  // [ratio, value, weight]
        
        for (int i = 0; i < n; i++) {
            double ratio = (double) arr[i].value / arr[i].weight;
            items[i][0] = ratio;
            items[i][1] = arr[i].value;
            items[i][2] = arr[i].weight;
        }
        
        // Step 2: Sort by ratio (descending - highest pehle)
        Arrays.sort(items, (a, b) -> Double.compare(b[0], a[0]));
        
        // Step 3: Fill knapsack greedily
        double totalValue = 0.0;
        
        for (int i = 0; i < n; i++) {
            double ratio = items[i][0];
            double value = items[i][1];
            double weight = items[i][2];
            
            if (weight <= W) {
                // Pura item le lo
                totalValue += value;
                W -= weight;
            } else {
                // Fraction le lo (jo capacity bachi hai)
                totalValue += (ratio * W);
                W = 0;
                break;  // Bag full!
            }
        }
        
        return totalValue;
    }
}
```

---

## Step-by-Step Explanation

### Step 1: Value Per Weight Ratio Calculate Karo

```java
double[][] items = new double[n][3];  // [ratio, value, weight]

for (int i = 0; i < n; i++) {
    double ratio = (double) arr[i].value / arr[i].weight;
    items[i][0] = ratio;
    items[i][1] = arr[i].value;
    items[i][2] = arr[i].weight;
}
```

**Logic:** Har item ka **value/weight ratio** nikalo - ye batata hai ki **1 kg weight mein kitni value milegi**

**Example:**
```
Item 1: value=60, weight=10  → ratio = 60/10 = 6.0
Item 2: value=100, weight=20 → ratio = 100/20 = 5.0
Item 3: value=120, weight=30 → ratio = 120/30 = 4.0
```

**Higher ratio = Better item!** (per kg zyada value milegi)

---

### Step 2: Sort by Ratio (Descending Order)

```java
Arrays.sort(items, (a, b) -> Double.compare(b[0], a[0]));
```

**Logic:** Sabse **best ratio wale items** pehle lenge!

**After sorting:**
```
items = [[6.0, 60, 10],   // Best ratio
         [5.0, 100, 20],  // Second best
         [4.0, 120, 30]]  // Third best
```

---

### Step 3: Greedily Fill Knapsack

```java
double totalValue = 0.0;

for (int i = 0; i < n; i++) {
    if (weight <= W) {
        // Pura item fit ho jata hai
        totalValue += value;
        W -= weight;
    } else {
        // Sirf fraction fit hota hai
        totalValue += (ratio * W);
        W = 0;
        break;  // Bag full ho gaya
    }
}
```

**Logic:**

**Case 1: Pura item fit hota hai** (`weight <= W`)
- Item ki puri value add karo
- Remaining capacity update karo: `W = W - weight`
- Next item check karo

**Case 2: Item pura fit nahi hota** (`weight > W`)
- Jo capacity bachi hai, utna fraction le lo
- Value = `ratio × remaining_capacity`
- Bag full! Break kar do

---

## Dry Run Example

**Input:**
```
W = 50 (knapsack capacity)
Items:
  Item 1: value=60, weight=10
  Item 2: value=100, weight=20
  Item 3: value=120, weight=30
```

### Step 1: Calculate ratios
```
Item 1: ratio = 60/10 = 6.0
Item 2: ratio = 100/20 = 5.0
Item 3: ratio = 120/30 = 4.0
```

### Step 2: Sort by ratio (descending)
```
items = [[6.0, 60, 10],   // Item 1
         [5.0, 100, 20],  // Item 2
         [4.0, 120, 30]]  // Item 3
```

### Step 3: Fill knapsack

| i | Ratio | Value | Weight | W (before) | Action | totalValue | W (after) |
|---|-------|-------|--------|------------|--------|------------|-----------|
| 0 | 6.0 | 60 | 10 | 50 | 10 ≤ 50 ✅ Full item | 0 + 60 = 60 | 40 |
| 1 | 5.0 | 100 | 20 | 40 | 20 ≤ 40 ✅ Full item | 60 + 100 = 160 | 20 |
| 2 | 4.0 | 120 | 30 | 20 | 30 > 20 ❌ Fraction | 160 + (4.0×20) = 240 | 0 |

**Answer:** `240.0` 🎉

**Explanation:**
- Item 1: Pura liya (60 value)
- Item 2: Pura liya (100 value)
- Item 3: 20kg liya out of 30kg → `(120/30) × 20 = 80` value
- **Total:** 60 + 100 + 80 = **240**

---

## Visual Representation

```
Knapsack Capacity: 50 kg
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Item 1 (ratio=6.0): ████████████ (10kg, value=60)  ✅ Full
Item 2 (ratio=5.0): ████████████████████████ (20kg, value=100) ✅ Full
Item 3 (ratio=4.0): ████████████████████ (20/30kg, value=80) ⚠️ Partial

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total: 50kg used, Total Value = 240
```

---

## Why Greedy Works Here?

**Greedy Choice:** Sabse **best ratio** (value/weight) wala item pehle lo!

**Proof:**
- Agar hum koi kam ratio wala item pehle lete
- Toh baad mein high ratio items ke liye jagah nahi bachti
- Result: Total value kam ho jati

**Example:**
```
W = 50
Item A: v=120, w=30, ratio=4.0
Item B: v=60, w=10, ratio=6.0

Wrong order (A first):
  Take A (30kg) → value=120, remaining=20kg
  Take B (10kg) → value=60, remaining=10kg
  Total = 180 ❌

Right order (B first):
  Take B (10kg) → value=60, remaining=40kg
  Take A (30kg) → value=120, remaining=10kg
  Total = 180... wait same! 🤔
  
But with 3 items, greedy gives optimal!
```

**Key insight:** Fraction allowed hai, toh **per unit value maximize karna** optimal strategy hai! 📊

---

## Fractional vs 0/1 Knapsack

| Feature | Fractional Knapsack | 0/1 Knapsack |
|---------|-------------------|--------------|
| **Can break items?** | ✅ YES (fraction le sakte) | ❌ NO (pura ya nothing) |
| **Approach** | ✅ Greedy works | ❌ Greedy fails, need DP |
| **Time Complexity** | O(n log n) | O(n × W) |
| **Example** | Gold dust (tol ke le sakte) | Laptop (tukda nahi kar sakte) |

**Why greedy works in fractional?**
- Fraction allowed → best ratio always optimal
- No need to worry about future choices

**Why greedy fails in 0/1?**
- Can't break items → best ratio might not fit
- Need to consider all combinations (DP)

---

## Complexity Analysis

- **Time Complexity:** `O(n log n)`
    - Calculate ratios: `O(n)`
    - Sorting: `O(n log n)`
    - Fill knapsack: `O(n)`
    - Total: `O(n log n)` (sorting dominates)

- **Space Complexity:** `O(n)`
    - 2D array for storing items with ratios

---

## Edge Cases

1. **Knapsack bahut badi hai:**
   ```
   W = 1000, total weight of items = 50
   ```
   → Sab items pura le lo → Return sum of all values ✅

2. **Knapsack bahut chhoti hai:**
   ```
   W = 5, smallest item weight = 10
   ```
   → Sabse best ratio wale ka fraction lo ✅

3. **Sab items same ratio:**
   ```
   All items: ratio = 5.0
   ```
   → Koi bhi order se le lo, answer same ✅

4. **Single item:**
   ```
   n = 1, W = 50, item: v=100, w=30
   ```
   → Pura item le lo (fits completely) → Return 100 ✅

---

## Interview Tips 💡

1. **Greedy choice explain karo clearly:** "Best value/weight ratio wale items pehle lenge kyunki per kg maximum value milegi"

2. **0/1 vs Fractional difference:** Interview mein ye zaroor mention karo ki fractional mein greedy works but 0/1 mein nahi

3. **Formula yaad rakho:**
    - Full item: `totalValue += value`
    - Partial item: `totalValue += (ratio × remaining_capacity)`

4. **Edge case mention karo:** Sab items fit ho jaye toh sab le lenge

5. **Follow-up questions:**
    - 0/1 Knapsack kaise solve karoge? (DP)
    - Items ki priority kaise decide karoge? (Value/weight ratio)
    - Multiple knapsacks? (Different problem - bin packing)

6. **Real-world example:** Gold/silver/diamond trading - fractional items, best per gram value pehle becho!

---

## Common Mistakes ❌

1. **Sort by value only:** Wrong! High value but heavy item wasteful ho sakta hai
2. **Sort by weight only:** Wrong! Light but low value items useless
3. **Ratio descending order bhulna:** Ascending sort karoge toh worst items pehle aayenge
4. **Integer division:** `value/weight` ko `(double)` cast karna zaruri!
5. **0/1 Knapsack samjhna:** Fractional mein greedy works, 0/1 mein DP chahiye

---
