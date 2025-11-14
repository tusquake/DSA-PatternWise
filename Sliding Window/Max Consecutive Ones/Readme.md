## Problem Kya Hai?

Ek binary array diya gaya hai (sirf 0 aur 1) aur ek integer `k`.

Tum **maximum k zeros** ko **1 mein flip** kar sakte ho.

**Goal:** Maximum kitne **consecutive 1's** ban sakte hain after flipping?

---

## Example

### Example 1:
```
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6

Explanation:
Original:  [1,1,1,0,0,0,1,1,1,1,0]
Flip 2 zeros:     ↑ ↑ (flip these two)
Result:    [1,1,1,0,0,1,1,1,1,1,1]
                     └────────┘
                   6 consecutive 1's

Subarray: [0,0,1,1,1,1,1,1] (index 3 to 10)
After flipping 2 zeros at positions 3,4: length = 6
```

### Example 2:
```
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10

Explanation:
Flip 3 zeros to get maximum consecutive 1's
Best window: [1,1,0,0,1,1,1,0,1,1] (flip 3 zeros)
Length: 10
```

### Example 3:
```
Input: nums = [1,1,1,1], k = 0
Output: 4

Explanation:
No flips needed, already 4 consecutive 1's
```

---

## Understanding the Problem

**Key insight:** Flip ka matlab actual array change nahi karna, sirf **imagine** karo ki k zeros ko 1 bana sakte ho.

**Reframe the problem:**
```
Original: Find longest subarray with at most k zeros
Equivalent: Find longest subarray where we can flip k zeros to make all 1's
```

**Real-world analogy:** Exam mein k questions skip kar sakte ho aur baki sab solve karne hain. Consecutive questions ka maximum group kitna lamba ho sakta hai jisme maximum k skip allowed hain?

---

## Solution - Sliding Window Approach

### Code (Java):
```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int zeroCnt = 0;  // Current window mein kitne zeros hain
        int start = 0;    // Window ka left pointer
        int ans = 0;      // Maximum length
        
        for (int i = 0; i < nums.length; i++) {
            // Expand window: right pointer (i) aage badhao
            if (nums[i] == 0) {
                zeroCnt++;
            }
            
            // Shrink window: jab zeros > k ho jaye
            while (zeroCnt > k) {
                if (nums[start] == 0) {
                    zeroCnt--;
                }
                start++;
            }
            
            // Update answer: valid window ki length
            if (zeroCnt <= k) {
                ans = Math.max(ans, i - start + 1);
            }
        }
        
        return ans;
    }
}
```

### Code (C++):
```cpp
int longestOnes(vector<int>& nums, int k) {
    int zeroCnt = 0, start = 0, ans = 0;
    
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] == 0)
            zeroCnt++;
        
        while (zeroCnt > k) {
            if (nums[start] == 0)
                zeroCnt--;
            start++;
        }
        
        if (zeroCnt <= k)
            ans = max(ans, i - start + 1);
    }
    
    return ans;
}
```

---

## Step-by-Step Explanation

### Variables

```java
int zeroCnt = 0;   // Window mein kitne zeros hain (flips needed)
int start = 0;     // Window ka left boundary
int i;             // Window ka right boundary (loop mein)
int ans = 0;       // Maximum valid window length
```

---

### Step 1: Expand Window (Add Element)

```java
for (int i = 0; i < nums.length; i++) {
    if (nums[i] == 0) {
        zeroCnt++;
    }
```

**Logic:** Right pointer `i` ko aage badhate hain. Agar current element 0 hai, toh zero count badhao (ek aur flip needed).

**Example:**
```
nums = [1,1,0,1,0]
i=0: nums[0]=1 → zeroCnt=0
i=1: nums[1]=1 → zeroCnt=0
i=2: nums[2]=0 → zeroCnt=1 (one zero found)
i=3: nums[3]=1 → zeroCnt=1
i=4: nums[4]=0 → zeroCnt=2 (two zeros found)
```

---

### Step 2: Shrink Window (Remove Invalid Elements)

```java
while (zeroCnt > k) {
    if (nums[start] == 0) {
        zeroCnt--;
    }
    start++;
}
```

**Condition:** `zeroCnt > k`

**Meaning:** Current window mein **k se zyada zeros** hain. Matlab hum itne flips nahi kar sakte.

**Logic:** Left pointer `start` ko aage badhao jab tak zeros ki count k ke andar na aa jaye.

**Example:**
```
Window: [0,0,0,1,1], k=2, zeroCnt=3
3 > 2 → Invalid! Too many zeros

Shrink:
Remove nums[start]=0: zeroCnt=2, start=1
Now window: [0,0,1,1]
2 <= 2 → Valid! ✅
```

---

### Step 3: Update Answer

```java
if (zeroCnt <= k) {
    ans = Math.max(ans, i - start + 1);
}
```

**Condition:** `zeroCnt <= k`

**Meaning:** Current window **valid** hai - maximum k zeros hain jo hum flip kar sakte hain.

**Logic:** Valid window ki length se answer update karo.

**Window length:** `i - start + 1`

---

## Dry Run Example

**Input:** `nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2`

### Iteration by iteration:

**i=0: nums[0]=1**
```
Window: [1]
zeroCnt = 0
0 <= 2 ✅ Valid
ans = max(0, 0-0+1) = 1
```

**i=1: nums[1]=1**
```
Window: [1,1]
zeroCnt = 0
0 <= 2 ✅ Valid
ans = max(1, 1-0+1) = 2
```

**i=2: nums[2]=1**
```
Window: [1,1,1]
zeroCnt = 0
0 <= 2 ✅ Valid
ans = max(2, 2-0+1) = 3
```

**i=3: nums[3]=0**
```
Add 0
Window: [1,1,1,0]
zeroCnt = 1
1 <= 2 ✅ Valid
ans = max(3, 3-0+1) = 4
```

**i=4: nums[4]=0**
```
Add 0
Window: [1,1,1,0,0]
zeroCnt = 2
2 <= 2 ✅ Valid
ans = max(4, 4-0+1) = 5
```

**i=5: nums[5]=0**
```
Add 0
Window: [1,1,1,0,0,0]
zeroCnt = 3
3 > 2 ❌ Invalid! Too many zeros

Shrink:
Remove nums[0]=1: start=1
zeroCnt = 3 (still > 2)

Remove nums[1]=1: start=2
zeroCnt = 3 (still > 2)

Remove nums[2]=1: start=3
zeroCnt = 3 (still > 2)

Remove nums[3]=0: start=4, zeroCnt=2
Now window: [0,0,0] (indices 4,5,6 will come)
2 <= 2 ✅ Valid
ans = max(5, 5-4+1) = max(5, 2) = 5
```

**i=6: nums[6]=1**
```
Window: [0,0,1]
zeroCnt = 2
2 <= 2 ✅ Valid
ans = max(5, 6-4+1) = max(5, 3) = 5
```

**i=7: nums[7]=1**
```
Window: [0,0,1,1]
zeroCnt = 2
2 <= 2 ✅ Valid
ans = max(5, 7-4+1) = max(5, 4) = 5
```

**i=8: nums[8]=1**
```
Window: [0,0,1,1,1]
zeroCnt = 2
2 <= 2 ✅ Valid
ans = max(5, 8-4+1) = max(5, 5) = 5
```

**i=9: nums[9]=1**
```
Window: [0,0,1,1,1,1]
zeroCnt = 2
2 <= 2 ✅ Valid
ans = max(5, 9-4+1) = max(5, 6) = 6 ✅
```

**i=10: nums[10]=0**
```
Add 0
Window: [0,0,1,1,1,1,0]
zeroCnt = 3
3 > 2 ❌ Invalid!

Shrink:
Remove nums[4]=0: start=5, zeroCnt=2
Now window: [0,1,1,1,1,0]
2 <= 2 ✅ Valid
ans = max(6, 10-5+1) = max(6, 6) = 6
```

**Final Answer:** `6`

**Best window:** `[0,0,1,1,1,1]` at indices 4-9, flip 2 zeros to get 6 consecutive 1's

---

## Visual Representation

```
nums = [1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 0]
Index:  0  1  2  3  4  5  6  7  8  9  10
k = 2 (can flip at most 2 zeros)

Window progression:

i=0-2:  [1 1 1]              zeros=0 ✅ length=3
i=3:    [1 1 1 0]            zeros=1 ✅ length=4
i=4:    [1 1 1 0 0]          zeros=2 ✅ length=5
i=5:    [1 1 1 0 0 0]        zeros=3 ❌ Shrink!
        Shrink → [0 0 0]     Start moves to index 4
i=6:    [0 0 1]              zeros=2 ✅ length=3
i=7:    [0 0 1 1]            zeros=2 ✅ length=4
i=8:    [0 0 1 1 1]          zeros=2 ✅ length=5
i=9:    [0 0 1 1 1 1]        zeros=2 ✅ length=6 🎯 MAX!
i=10:   [0 0 1 1 1 1 0]      zeros=3 ❌ Shrink!
        Shrink → [0 1 1 1 1 0] zeros=2 ✅ length=6

Maximum length: 6
Best subarray: indices 4-9 → [0,0,1,1,1,1]
After flipping 2 zeros → [1,1,1,1,1,1] ✅
```

---

## Why This Approach Works?

**Sliding Window Logic:**

1. **Expand:** Right pointer aage badhao, zeros count karo
2. **Shrink:** Jab zeros > k ho, left pointer aage badhao
3. **Track:** Valid window (zeros <= k) ki maximum length store karo

**Key Insight:** Tumhe actual flips nahi karne, sirf **count** karna hai ki kitne zeros hain. Agar zeros <= k, matlab us window ko valid bana sakte ho by flipping.

**Real-world analogy:** Movie theatre mein seats book kar rahe ho. K seats already occupied hain (zeros), tum unhe adjust kar sakte ho (flip). Maximum continuous seats ki row kaunsi hai?

---

## Why Not Brute Force?

**Brute force approach:**
```
For each starting position:
  For each ending position:
    Count zeros in subarray
    If zeros <= k, update answer
Time: O(n²) or O(n³)
```

**Sliding window advantage:**
- Single pass: O(n)
- No redundant counting
- Efficient shrinking mechanism

---

## Complexity Analysis

### Time Complexity: O(n)

**Explanation:**
- Outer loop: O(n) - traverse entire array
- Inner while loop: Har element maximum ek baar remove hota hai
- Total: Har element ko add kiya (outer loop) aur remove kiya (inner loop)
- Operations: 2n = O(n)

**Important:** `start` pointer kabhi peeche nahi jata, hamesha forward. Isliye amortized O(n) hai.

### Space Complexity: O(1)

**Explanation:**
- Constant variables: `zeroCnt`, `start`, `ans`, `i`
- No extra data structures
- Input array modify nahi kar rahe

---

## Edge Cases

1. **k = 0 (no flips allowed):**
   ```
   nums = [1,1,0,1,1], k = 0
   Output: 2 (longest consecutive 1's without flipping)
   ```

2. **All zeros:**
   ```
   nums = [0,0,0,0], k = 2
   Output: 2 (flip 2 zeros)
   ```

3. **All ones:**
   ```
   nums = [1,1,1,1], k = 2
   Output: 4 (no flips needed)
   ```

4. **k >= total zeros:**
   ```
   nums = [1,0,1,0,1], k = 5
   Output: 5 (flip all zeros)
   ```

5. **Single element:**
   ```
   nums = [0], k = 1
   Output: 1 (flip the zero)
   ```

---

## Interview Tips

1. **Problem reframe karo:** "At most k zeros wale longest subarray find karna hai"

2. **Sliding window justify karo:** "Window expand karte hain right se, shrink karte hain left se jab condition violate ho"

3. **Time complexity explain karo:** "O(n) hai kyunki har element maximum 2 baar process hota hai"

4. **Why not brute force:** "O(n²) approach slow hai, sliding window O(n) mein karta hai"

5. **Follow-up questions:**
   - What if we need to return the actual subarray? (Track start and max length)
   - What if array has multiple types of numbers? (Count specific element to flip)
   - Minimum flips needed? (Different problem - count transitions)

---

## Related Problems

1. **Max Consecutive Ones I**
   - No flips allowed (k = 0)
   - Simpler version

2. **Max Consecutive Ones II**
   - Exactly one flip allowed (k = 1)
   - Similar approach

3. **Longest Substring with At Most K Distinct Characters**
   - Same sliding window pattern
   - Different validation condition

4. **Longest Repeating Character Replacement**
   - Similar concept with character replacement

---

## Common Mistakes

1. **zeroCnt update bhulna:** 0 milne pe count badhana bhulte hain

2. **Shrink condition galat:** `zeroCnt > k` pe shrink karna hai, `>=` nahi

3. **Window size calculation:** `i - start + 1` formula yaad rakhna

4. **Answer update condition:** `zeroCnt <= k` check karna important

5. **While vs If confusion:** While loop use karo shrinking ke liye, multiple elements remove karne pad sakte hain

---

## Alternative Approach (Without If Check)

```java
int longestOnes(int[] nums, int k) {
    int start = 0, ans = 0;
    
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) k--;
        
        while (k < 0) {
            if (nums[start] == 0) k++;
            start++;
        }
        
        ans = Math.max(ans, i - start + 1);
    }
    
    return ans;
}
```

**Difference:** Decrement k instead of incrementing zeroCnt. Same logic, different implementation.

---

## Key Takeaway

**Problem:** Find longest subarray of consecutive 1's after flipping at most k zeros.

**Reframe:** Find longest subarray with at most k zeros.

**Approach:** Sliding window
- Expand: Add elements, count zeros
- Shrink: Remove elements when zeros > k
- Track: Maximum valid window length

**Core Logic:**
```
Valid window = zeroCnt <= k (can flip all zeros)
Invalid window = zeroCnt > k (too many zeros to flip)
Shrink until valid again
```

**Formula:**
```
window_length = i - start + 1
valid = zeros in window <= k
```

Classic sliding window problem - interview mein bahut common hai especially for binary array questions!