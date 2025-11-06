Tumhare paas do arrays hain:
- `g[]` - Bacchon ki **greed factor** (har baccha kitna bada cookie chahta hai)
- `s[]` - **Cookie sizes** (tumhare paas kitni badi cookies hain)

**Goal:** Maximum kitne bacchon ko satisfy kar sakte ho?

**Rule:** Ek baccha tab hi satisfied hoga jab usko uski greed ke barabar ya badi cookie milegi.

---

## Solution - Greedy Algorithm

### Code:
```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int j=0;
        for(int i=0;i<s.length && j<g.length;i++){
            if(s[i]>=g[j]) j++;
        }
        return j;
    }
}
```

---

## Step-by-Step Explanation

### Step 1: Sorting Kyun Kari?
```java
Arrays.sort(g);  // Bacchon ko greed ke hisaab se sort karo
Arrays.sort(s);  // Cookies ko size ke hisaab se sort karo
```

**Logic:** Idhr aisa krenge toh sabse chhote greed wale bacche ko sabse chhoti available cookie dene ki koshish karenge. Isse maximum bacche satisfy honge!

**Example:**
- `g = [1, 2, 3]` - Bacchon ki greed (sorted)
- `s = [1, 1, 2]` - Cookie sizes (sorted)

---

### Step 2: Two Pointer Approach

```java
int j = 0;  // Bacchon ke liye pointer (g array pe)
```

**Loop chala rahe hain cookies pe:**
```java
for(int i=0; i<s.length && j<g.length; i++)
```

- `i` pointer cookies array pe chalega
- `j` pointer bacchon array pe chalega
- Jab tak cookies ya bacche khatam nahi hote, loop chalega

---

### Step 3: Cookie Assign Karo

```java
if(s[i] >= g[j]) {
    j++;
}
```

**Logic:** 
- Idhr aisa krenge toh agar current cookie (`s[i]`) current bacche ki greed (`g[j]`) se badi ya equal hai, toh use cookie de denge aur next bacche pe move karenge (`j++`)
- Agar cookie chhoti hai? Toh simply skip karo aur next cookie check karo (i++ automatically ho jayega loop mein)

---

### Step 4: Return Answer

```java
return j;
```

`j` ki value batayegi kitne bacchon ko successfully cookie mili!

---

## Dry Run Example

**Input:**
- `g = [1, 2, 3]` (bacchon ki greed)
- `s = [1, 1]` (cookie sizes)


## Why Greedy Works Here?

Idhr aisa krenge toh:
1. **Sabse chhote greed wale bacche ko pehle satisfy karenge** → Badi cookies bade greed wale bacchon ke liye bach jayengi
2. **Sorting se optimal matching milti hai** → Wastage nahi hoti cookies ki
3. **Ek pass mein solution mil jata hai** → O(n log n) time complexity

---

## Complexity Analysis

- **Time Complexity:** `O(n log n + m log m)` 
  - Sorting dono arrays ki: `O(n log n)` + `O(m log m)`
  - Loop: `O(max(n, m))`
  
- **Space Complexity:** `O(1)` 
  - Extra space nahi use kar rahe (sorting in-place hoti hai)


