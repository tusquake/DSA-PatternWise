Tum lemonade bech rahe ho aur **har lemonade ki price $5 hai**.

Customers tumhe **$5, $10, ya $20** ki bills dete hain. Tumhe **correct change** wapas dena hai.

**Starting mein:** Tumhare paas koi change nahi hai (empty cash register)

**Goal:** Check karo ki **har customer ko correct change de sakte ho ya nahi**?

---

## Solution - Greedy Approach

### Code:
```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int f = 0, te = 0;  // f = $5 notes, te = $10 notes
        for (int bill : bills) {
            if (bill == 5) {
                f++;
            } 
            else if (bill == 10) {
                if (f == 0) return false;
                f--;
                te++;
            } 
            else {    // bill == 20
                if (te > 0 && f > 0) { 
                    te--;
                    f--;
                } else if (f >= 3) {
                    f -= 3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
}
```

---

## Step-by-Step Explanation

### Variables Setup
```java
int f = 0;   // $5 notes ki count
int te = 0;  // $10 notes ki count
```

**Note:** $20 notes ko track karne ki zarurat nahi kyunki woh change dene mein use nahi hoti!

---

### Case 1: Customer Deta Hai $5 Bill

```java
if (bill == 5) {
    f++;
}
```

**Logic:** Isme koi change nahi dena, seedha $5 note rakh lo cash register mein.

**Example:**
- Customer pays: $5
- Change needed: $0
- Action: `f++` (ab hamare paas ek $5 note hai)

---

### Case 2: Customer Deta Hai $10 Bill

```java
else if (bill == 10) {
    if (f == 0) return false;  // Agar $5 note nahi hai toh change nahi de sakte
    f--;    // Ek $5 note change mein de diya
    te++;   // $10 note rakh liya
}
```

**Logic:** 
- Price: $5
- Customer diya: $10
- Change chahiye: **$5**
- Agar hamare paas $5 note nahi hai → **return false** ❌
- Warna: Ek $5 de do aur $10 rakh lo ✅

**Example:**
- Customer pays: $10
- Change needed: $5
- Have $5 notes? YES → Give one, keep the $10

---

### Case 3: Customer Deta Hai $20 Bill (TRICKY!)

```java
else {    // bill == 20
    if (te > 0 && f > 0) {      // Strategy 1: Use 1×$10 + 1×$5
        te--;
        f--;
    } else if (f >= 3) {         // Strategy 2: Use 3×$5
        f -= 3;
    } else {
        return false;            // Change nahi de sakte
    }
}
```

**Logic:** 
- Price: $5
- Customer diya: $20
- Change chahiye: **$15**

**Two ways to give $15 change:**
1. **1×$10 + 1×$5 = $15** (PREFER THIS!)
2. **3×$5 = $15** (backup option)

### Why Prefer Strategy 1?

**$5 notes zyada valuable hain!** Kyunki:
- $10 change dene mein sirf $5 use hota hai
- $20 change dene mein bhi $5 chahiye
- **$5 ko conserve karna smart move hai**

**Example:**
```
bills = [5, 5, 10, 20]
```
- After [5,5,10]: f=1, te=1
- For bill=20: Use 1×$10 + 1×$5 → f=0, te=0 ✅
- Agar 3×$5 use karte toh f=-2 ho jata (FAIL!) ❌

---

## Dry Run Example

**Input:** `bills = [5, 5, 10, 10, 20]`

| Bill | Action | f | te | Status |
|------|--------|---|----|----|
| Start | - | 0 | 0 | - |
| 5 | f++ | 1 | 0 | ✅ |
| 5 | f++ | 2 | 0 | ✅ |
| 10 | f--, te++ | 1 | 1 | ✅ (gave $5) |
| 10 | f--, te++ | 0 | 2 | ✅ (gave $5) |
| 20 | te--, f-- | ? | 1 | ❌ f=0, can't give change! |

**Answer:** `false` - 5th customer ko change nahi de sakte!

---

## Another Example (Success Case)

**Input:** `bills = [5, 5, 5, 10, 20]`

| Bill | Action | f | te | Status |
|------|--------|---|----|----|
| Start | - | 0 | 0 | - |
| 5 | f++ | 1 | 0 | ✅ |
| 5 | f++ | 2 | 0 | ✅ |
| 5 | f++ | 3 | 0 | ✅ |
| 10 | f--, te++ | 2 | 1 | ✅ (gave $5) |
| 20 | te--, f-- | 1 | 0 | ✅ (gave $10+$5) |

**Answer:** `true` - Sab customers ko change mil gaya!

---

## Why Greedy Works Here?

Greedy choice hai: **$20 ke liye pehle 1×$10 + 1×$5 use karo, then 3×$5**

**Reason:** 
- $5 notes universal hain (har change mein use ho sakte hain)
- $10 notes limited use ke hain (sirf $20 change mein)
- $5 ko save karna = future customers ko bhi change de payenge

---

## Complexity Analysis

- **Time Complexity:** `O(n)` 
  - Bas ek loop chalaya hai array pe
  
- **Space Complexity:** `O(1)` 
  - Sirf 2 variables use kiye (f, te)

---

## Edge Cases

1. **Pehla customer hi $10 ya $20 deta hai:**
   ```
   bills = [10, 5, 5]
   ```
   → Pehle customer ko change nahi de sakte → `false`

2. **Saare customers $5 dete hain:**
   ```
   bills = [5, 5, 5, 5]
   ```
   → Kisi ko change nahi dena → `true`

3. **$20 ke liye sirf 3×$5 option hai:**
   ```
   bills = [5, 5, 5, 5, 20]
   ```
   → f=4, use 3×$5 → f=1 → `true`


---

**Greedy strategy:** Jo resource zyada versatile hai (like $5 notes), use conserve karo!

Simple simulation problem hai - bas track karo kitne notes hai aur har transaction pe check karo ki change de sakte ho ya nahi!