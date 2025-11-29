# Isomorphic Strings

## Problem Kya Hai?

Do strings check karo - kya one-to-one character mapping possible hai?

**Rules:**
- Har character consistently map hona chahiye
- Ek character sirf ek character ko map ho sakta hai
- Do characters same target ko map nahi ho sakte

---

## Examples

```
s = "egg", t = "add"
e→a, g→d, g→d ✓
Result: true

s = "foo", t = "bar"  
f→b, o→a, o→r ❌ (o already mapped to a)
Result: false

s = "ab", t = "aa"
a→a, b→a ❌ (both mapping to same 'a')
Result: false
```

---

## Approach

**Two Maps Needed:** Bidirectional checking

```cpp
mps: s → t (forward mapping)
mpt: t → s (reverse mapping)
```

**Why Two Maps?**
- `mps` ensures: one `s[i]` → one `t[i]` only
- `mpt` ensures: one `t[i]` ← one `s[i]` only

**Example showing why:**
```
s = "ab", t = "aa"

Only mps: Would pass ❌ (both a,b → a)
With mpt: Detects conflict ✓ (a already used)
```

---

## Code

```cpp
bool isIsomorphic(string s, string t) {
    unordered_map<char, char> mps;  // s → t
    unordered_map<char, char> mpt;  // t → s
    
    for (int i = 0; i < s.size(); i++) {
        // Both characters new → create mapping
        if (mps.find(s[i]) == mps.end() && mpt.find(t[i]) == mpt.end()) {
            mps[s[i]] = t[i];
            mpt[t[i]] = s[i];
        } 
        // Check if existing mapping matches
        else if (mps[s[i]] != t[i] || mpt[t[i]] != s[i]) {
            return false;  // Conflict!
        }
    }
    
    return true;
}
```

---

## Step-by-Step

### Step 1: Check if Both New
```cpp
if (mps.find(s[i]) == mps.end() && mpt.find(t[i]) == mpt.end())
```
- `s[i]` not mapped yet
- `t[i]` not used as target yet
- Create bidirectional mapping

### Step 2: Validate Existing Mapping
```cpp
else if (mps[s[i]] != t[i] || mpt[t[i]] != s[i])
```
- Check forward: Does `s[i]` map to correct `t[i]`?
- Check reverse: Does `t[i]` come from correct `s[i]`?
- If either fails → conflict!

### Step 3: Return
```cpp
return true;  // All mappings consistent
```

---

## Dry Run

### Input: `s = "egg", t = "add"`

```
i=0: e→a, a→e
  mps={'e':'a'}, mpt={'a':'e'}

i=1: g→d, d→g
  mps={'e':'a', 'g':'d'}, mpt={'a':'e', 'd':'g'}

i=2: g→?, expect d
  mps['g'] = 'd' ✓
  mpt['d'] = 'g' ✓
  Continue

Result: true
```

### Input: `s = "foo", t = "bar"`

```
i=0: f→b
  mps={'f':'b'}, mpt={'b':'f'}

i=1: o→a
  mps={'f':'b', 'o':'a'}, mpt={'b':'f', 'a':'o'}

i=2: o→?, expect a, but got r
  mps['o'] = 'a', but t[2] = 'r'
  'a' != 'r' ❌

Result: false
```

### Input: `s = "ab", t = "aa"`

```
i=0: a→a
  mps={'a':'a'}, mpt={'a':'a'}

i=1: b→?, got a
  'b' not in mps ✓
  'a' already in mpt ✗ (mapped from 'a')
  mpt['a'] = 'a', but s[1] = 'b'
  'a' != 'b' ❌

Result: false
```

---

## Why Two Maps?

**Without `mpt` (Only forward check):**
```
s = "ab", t = "aa"
mps['a'] = 'a' ✓
mps['b'] = 'a' ✓ (would pass!)
❌ Both 'a' and 'b' mapping to 'a'
```

**With `mpt` (Reverse check):**
```
s = "ab", t = "aa"
mps['a'] = 'a', mpt['a'] = 'a' ✓
mps['b'] = ?, but mpt['a'] already exists
mpt['a'] = 'a' ≠ s[1] = 'b' ❌
✓ Correctly rejects!
```

---

## Complexity

**Time:** O(n) - Single pass  
**Space:** O(1) - Max 26 characters (or 128 ASCII)

---

## Common Mistakes

### 1. Using Only One Map
```cpp
// ❌ Wrong - Misses reverse conflicts
unordered_map<char, char> mp;
if (mp[s[i]] != t[i]) return false;
// s="ab", t="aa" would pass incorrectly!
```

### 2. Missing Reverse Check
```cpp
// ❌ Wrong - Only forward check
if (mps[s[i]] != t[i]) return false;
// Need: mpt[t[i]] != s[i] also!
```

---

## Edge Cases

```
s = "", t = "" → true (empty)
s = "a", t = "a" → true (identity)
s = "aaa", t = "bbb" → true (consistent)
s = "aaa", t = "abc" → false (one to many)
```

---

## Interview Tips

**Key Point:** Need bidirectional mapping for one-to-one check

**Quick Test:**
```
Root appears first → Preorder
If one map fails: s="ab", t="aa" (show this example!)
```

**Follow-up:**
- Can we optimize? → Use arrays (256 size)
- Time complexity? → O(n), single pass

---

## Key Takeaways

| Aspect | Value |
|--------|-------|
| **Core Idea** | One-to-one mapping (bijection) |
| **Solution** | Two maps (bidirectional) |
| **Time** | O(n) |
| **Space** | O(1) - fixed alphabet |
| **Critical** | Check both `mps[s[i]]` and `mpt[t[i]]` |
