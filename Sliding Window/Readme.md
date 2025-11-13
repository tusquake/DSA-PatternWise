## Problem Kya Hai?

Ek string diya gaya hai. Tumhe **longest substring** find karna hai jisme **koi character repeat nahi** ho.

**Substring:** Continuous sequence of characters (not subsequence!)

**Goal:** Maximum length return karo of such substring.

---

## Example

### Example 1:
```
Input: s = "abcabcbb"
Output: 3

Explanation:
Substrings without repeating chars:
- "a" (length 1)
- "ab" (length 2)
- "abc" (length 3) ✅ Longest
- "b" (length 1)
- "bc" (length 2)
- "bca" (length 3) ✅ Longest
- ...

Answer: 3
```

### Example 2:
```
Input: s = "bbbbb"
Output: 1

Explanation:
All characters same hain
Longest unique substring: "b" (length 1)
```

### Example 3:
```
Input: s = "pwwkew"
Output: 3

Explanation:
- "pw" (length 2)
- "wke" (length 3) ✅ Longest
- "kew" (length 3) ✅ Longest

Answer: 3
Note: "pwke" is subsequence not substring (not continuous)
```

### Example 4:
```
Input: s = ""
Output: 0

Explanation: Empty string
```

---

## Solution - Sliding Window Approach

### Code (Java):
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int ans = 0;
        Map<Character, Integer> map = new HashMap<>();
        int start = 0;
        
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
            
            // Shrink window if duplicate found
            while (map.size() < i - start + 1) {
                char leftChar = s.charAt(start);
                map.put(leftChar, map.get(leftChar) - 1);
                if (map.get(leftChar) == 0) {
                    map.remove(leftChar);
                }
                start++;
            }
            
            // Update answer if current window is valid
            if (map.size() == i - start + 1) {
                ans = Math.max(ans, i - start + 1);
            }
        }
        
        return ans;
    }
}
```

### Code (C++):
```cpp
int lengthOfLongestSubstring(const string& s) {
    int ans = 0;
    unordered_map<char, int> mp;
    int start = 0;
    
    for (int i = 0; i < s.size(); i++) {
        mp[s[i]]++;
        
        // Shrink window while duplicates exist
        while (mp.size() < i - start + 1) {
            mp[s[start]]--;
            if (mp[s[start]] == 0)
                mp.erase(s[start]);
            start++;
        }
        
        // Update max length
        if (mp.size() == i - start + 1)
            ans = max(ans, i - start + 1);
    }
    
    return ans;
}
```

---

## Sliding Window Concept

**Sliding Window:** Do pointers use karte hain - `start` aur `i` (end)

```
Window: [start ... i]
        left     right

Expand: i++ (right pointer aage badhao)
Shrink: start++ (left pointer aage badhao)
```

**Real-world analogy:** Train ki window se bahar dekh rahe ho. Jaise jaise train move karti hai, view change hota hai. Kabhi window badi ho jati hai (expand), kabhi chhoti (shrink).

---

## Step-by-Step Explanation

### Variables

```java
int ans = 0;                    // Maximum length track karne ke liye
Map<Character, Integer> map;    // Character frequency store karne ke liye
int start = 0;                  // Window ka left pointer
int i;                          // Window ka right pointer (loop mein)
```

---

### Step 1: Expand Window (Add Character)

```java
for (int i = 0; i < s.length(); i++) {
    char c = s.charAt(i);
    map.put(c, map.getOrDefault(c, 0) + 1);
```

**Logic:** Right pointer `i` ko aage badhate hain aur current character ko window mein add karte hain. Map mein frequency store karte hain.

**Example:**
```
s = "abca"
i = 0: Add 'a' → map = {a:1}
i = 1: Add 'b' → map = {a:1, b:1}
i = 2: Add 'c' → map = {a:1, b:1, c:1}
i = 3: Add 'a' → map = {a:2, b:1, c:1} (duplicate!)
```

---

### Step 2: Shrink Window (Remove Duplicates)

```java
while (map.size() < i - start + 1) {
    char leftChar = s.charAt(start);
    map.put(leftChar, map.get(leftChar) - 1);
    if (map.get(leftChar) == 0) {
        map.remove(leftChar);
    }
    start++;
}
```

**Condition:** `map.size() < i - start + 1`

**Meaning:**
- `map.size()` = Unique characters in current window
- `i - start + 1` = Window size (total characters)
- Agar unique < total, matlab **duplicate hai**!

**Logic:** Jab tak duplicate hai, left pointer `start` ko aage badhao aur characters ko window se remove karo.

**Example:**
```
s = "abca", window = "abca"
map = {a:2, b:1, c:1}
map.size() = 3, window_size = 4
3 < 4 → Duplicate exists!

Remove 'a': map = {a:1, b:1, c:1}, start = 1
Now window = "bca"
map.size() = 3, window_size = 3
3 == 3 → No duplicate ✅
```

---

### Step 3: Update Answer

```java
if (map.size() == i - start + 1) {
    ans = Math.max(ans, i - start + 1);
}
```

**Condition:** `map.size() == i - start + 1`

**Meaning:** Current window mein sab characters unique hain (no duplicates).

**Logic:** Agar valid window hai, toh maximum length update karo.

---

## Window Size Calculation

**Formula:** `window_size = i - start + 1`

**Example:**
```
s = "abcde"
     01234

Window from index 1 to 3: "bcd"
start = 1, i = 3
window_size = 3 - 1 + 1 = 3 ✅
```

**Why +1?** Because indexing 0-based hai. Indices 1 to 3 mein 3 characters hain (not 2).

---

## Dry Run Example

**Input:** `s = "abcabcbb"`

### Iteration by iteration:

**i = 0: 'a'**
```
Window: "a"
map = {a:1}
map.size() = 1, window_size = 0-0+1 = 1
1 == 1 → Valid ✅
ans = max(0, 1) = 1
```

**i = 1: 'b'**
```
Add 'b'
Window: "ab"
map = {a:1, b:1}
map.size() = 2, window_size = 1-0+1 = 2
2 == 2 → Valid ✅
ans = max(1, 2) = 2
```

**i = 2: 'c'**
```
Add 'c'
Window: "abc"
map = {a:1, b:1, c:1}
map.size() = 3, window_size = 2-0+1 = 3
3 == 3 → Valid ✅
ans = max(2, 3) = 3
```

**i = 3: 'a'**
```
Add 'a'
Window: "abca"
map = {a:2, b:1, c:1}
map.size() = 3, window_size = 3-0+1 = 4
3 < 4 → Duplicate! ❌

Shrink:
  Remove s[0]='a': map = {a:1, b:1, c:1}, start = 1
  Window: "bca"
  map.size() = 3, window_size = 3-1+1 = 3
  3 == 3 → Valid ✅
  
ans = max(3, 3) = 3
```

**i = 4: 'b'**
```
Add 'b'
Window: "bcab"
map = {a:1, b:2, c:1}
map.size() = 3, window_size = 4-1+1 = 4
3 < 4 → Duplicate! ❌

Shrink:
  Remove s[1]='b': map = {a:1, b:1, c:1}, start = 2
  Window: "cab"
  map.size() = 3, window_size = 4-2+1 = 3
  3 == 3 → Valid ✅
  
ans = max(3, 3) = 3
```

**i = 5: 'c'**
```
Add 'c'
Window: "cabc"
map = {a:1, b:1, c:2}
map.size() = 3, window_size = 5-2+1 = 4
3 < 4 → Duplicate! ❌

Shrink:
  Remove s[2]='c': map = {a:1, b:1, c:1}, start = 3
  Window: "abc"
  map.size() = 3, window_size = 5-3+1 = 3
  3 == 3 → Valid ✅
  
ans = max(3, 3) = 3
```

**i = 6: 'b'**
```
Add 'b'
Window: "abcb"
map = {a:1, b:2, c:1}
map.size() = 3, window_size = 6-3+1 = 4
3 < 4 → Duplicate! ❌

Shrink:
  Remove s[3]='a': map = {b:2, c:1}, start = 4
  map.size() = 2, window_size = 6-4+1 = 3
  2 < 3 → Still duplicate! ❌
  
  Remove s[4]='b': map = {b:1, c:1}, start = 5
  Window: "cb"
  map.size() = 2, window_size = 6-5+1 = 2
  2 == 2 → Valid ✅
  
ans = max(3, 2) = 3
```

**i = 7: 'b'**
```
Add 'b'
Window: "cbb"
map = {b:2, c:1}
map.size() = 2, window_size = 7-5+1 = 3
2 < 3 → Duplicate! ❌

Shrink:
  Remove s[5]='c': map = {b:2}, start = 6
  map.size() = 1, window_size = 7-6+1 = 2
  1 < 2 → Still duplicate! ❌
  
  Remove s[6]='b': map = {b:1}, start = 7
  Window: "b"
  map.size() = 1, window_size = 7-7+1 = 1
  1 == 1 → Valid ✅
  
ans = max(3, 1) = 3
```

**Final Answer:** `3`

---

## Visual Representation

```
s = "a b c a b c b b"
     0 1 2 3 4 5 6 7

Step-by-step windows:

i=0: [a]           → length=1
i=1: [a b]         → length=2
i=2: [a b c]       → length=3 ✅ Max so far
i=3: [a b c a]     → Duplicate 'a', shrink
     Shrink → [b c a] → length=3
i=4: [b c a b]     → Duplicate 'b', shrink
     Shrink → [c a b] → length=3
i=5: [c a b c]     → Duplicate 'c', shrink
     Shrink → [a b c] → length=3
i=6: [a b c b]     → Duplicate 'b', shrink multiple times
     Shrink → [c b] → length=2
i=7: [c b b]       → Duplicate 'b', shrink multiple times
     Shrink → [b] → length=1

Maximum length: 3
```

---

## Why This Approach Works?

**Sliding Window Benefits:**

1. **Efficient:** Har character ko maximum 2 baar process karte hain (once when added, once when removed)

2. **No backtracking:** Left pointer kabhi peeche nahi jata, always forward

3. **Optimal:** O(n) time mein answer mil jata hai

**Key Insight:** Jab duplicate milta hai, toh start ko tab tak aage badhao jab tak duplicate remove na ho jaye. Window mein hamesha unique characters honge.

---

## Alternative Approach (Optimized)

```java
int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> map = new HashMap<>();
    int ans = 0;
    int start = 0;
    
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        
        // If character already in window, move start
        if (map.containsKey(c) && map.get(c) >= start) {
            start = map.get(c) + 1;
        }
        
        map.put(c, i);  // Store index instead of frequency
        ans = Math.max(ans, i - start + 1);
    }
    
    return ans;
}
```

**Difference:** Store index instead of frequency. Directly jump start to last occurrence + 1.

**Advantage:** No while loop needed, slightly faster.

---

## Complexity Analysis

### Time Complexity: O(n)

**Explanation:**
- Outer loop: O(n) - traverse entire string
- Inner while loop: Each character removed at most once
- Total operations: 2n (add + remove) = O(n)

### Space Complexity: O(min(n, m))

**Explanation:**
- Map stores unique characters
- Worst case: All unique → O(n)
- Best case (limited charset): O(m) where m = alphabet size
- For lowercase English: O(26) = O(1)

---

## Edge Cases

1. **Empty string:**
   ```
   s = ""
   Output: 0
   ```

2. **Single character:**
   ```
   s = "a"
   Output: 1
   ```

3. **All same characters:**
   ```
   s = "aaaa"
   Output: 1
   ```

4. **All unique characters:**
   ```
   s = "abcdef"
   Output: 6 (entire string)
   ```

5. **Two character pattern:**
   ```
   s = "abba"
   Output: 2 ("ab" or "ba")
   ```

---

## Interview Tips

1. **Sliding window explain karo:** "Do pointers use karke window maintain karte hain - expand karo right se, shrink karo left se jab duplicate mile"

2. **Map ka purpose:** "Frequency track karne ke liye use kar rahe hain taaki duplicate detect kar sakein"

3. **Time complexity justify karo:** "O(n) hai kyunki har character maximum 2 baar process hota hai"

4. **Alternative approach mention karo:** "Index-based approach bhi hai jo slightly faster hai"

5. **Follow-up questions:**
   - At most K distinct characters? (Similar approach with different condition)
   - Longest substring with at most 2 distinct? (Same logic)
   - Return the substring itself? (Track start and max length separately)

---

## Common Mistakes

1. **Window size calculation galat:** `i - start + 1` bhulna (0-indexed hai)

2. **Map.size() vs frequency confusion:** Map size unique chars count deta hai, not total chars

3. **While loop condition:** `map.size() < window_size` condition samajhna important

4. **Erase from map:** Frequency 0 hone pe map se remove karna bhulna

5. **Start pointer update:** start++ karna bhulna shrink karte time

---

## Key Takeaway

**Problem:** Find longest substring without repeating characters.

**Approach:** Sliding window with two pointers
- Expand window by adding characters (i++)
- Shrink window when duplicate found (start++)
- Track maximum valid window size

**Core Logic:**
```
Valid window = map.size() == window_size (all unique)
Invalid window = map.size() < window_size (duplicates exist)
```

**Formula:**
```
window_size = i - start + 1
valid = all characters in [start, i] are unique
```
