# Implement Trie (Prefix Tree) - Interview Revision Notes

## Problem Kya Hai?

**Trie (Prefix Tree)** implement karna hai jo teen operations support kare:

1. **insert(word)** - Ek word ko trie mein add karna
2. **search(word)** - Check karna ki word trie mein exist karta hai ya nahi
3. **startsWith(prefix)** - Check karna ki koi word given prefix se start hota hai ya nahi

**Use case:** Dictionary, autocomplete, spell checker jaise applications mein use hota hai.

---

## What is a Trie?

**Trie** ek tree-based data structure hai jisme:
- Har node ek character represent karta hai
- Root node empty hota hai
- Har path root se leaf tak ek word represent karta hai
- Common prefixes share karte hain same nodes

**Real-world analogy:** Phone directory jaisa - jaise "Raj", "Rajesh", "Raman" sab "Ra" share karte hain, trie mein bhi common prefix share hoti hai.

---

## Trie Structure

```
Example: Words - "cat", "car", "card", "dog"

              root
             /    \
            c      d
            |      |
            a      o
           / \     |
          t   r    g
              |
              d
```

**Observations:**
- "cat" aur "car" mein "ca" common hai
- "car" aur "card" mein "car" common hai
- Common prefixes ek hi path use karte hain

---

## Solution - Trie Implementation

### Code (Java):
```java
class TrieNode {
    TrieNode[] child;
    boolean isWord;
    
    TrieNode() {
        child = new TrieNode[26];  // a-z ke liye
        isWord = false;
    }
}

class Trie {
    TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode curr = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word.charAt(i) - 'a';
            if (curr.child[index] == null) {
                curr.child[index] = new TrieNode();
            }
            curr = curr.child[index];
        }
        curr.isWord = true;
    }
    
    public boolean search(String word) {
        TrieNode curr = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word.charAt(i) - 'a';
            if (curr.child[index] == null) {
                return false;
            }
            curr = curr.child[index];
        }
        return curr != null && curr.isWord;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode curr = root;
        for (int i = 0; i < prefix.length(); i++) {
            int index = prefix.charAt(i) - 'a';
            if (curr.child[index] == null) {
                return false;
            }
            curr = curr.child[index];
        }
        return curr != null;
    }
}
```

### Code (C++):
```cpp
class TrieNode {
public:
    TrieNode* child[26];
    bool isWord;
    
    TrieNode() {
        isWord = false;
        for (int i = 0; i < 26; i++) {
            child[i] = NULL;
        }
    }
};

class Trie {
public:
    TrieNode* root;
    
    Trie() {
        root = new TrieNode();
    }
    
    void insert(string word) {
        TrieNode* curr = root;
        for (int i = 0; i < word.size(); i++) {
            int ltr = word[i] - 'a';
            if (!curr->child[ltr]) {
                curr->child[ltr] = new TrieNode();
            }
            curr = curr->child[ltr];
        }
        curr->isWord = true;
    }
    
    bool search(string word) {
        TrieNode* curr = root;
        for (int i = 0; i < word.size(); i++) {
            int ltr = word[i] - 'a';
            if (!curr->child[ltr]) return false;
            curr = curr->child[ltr];
        }
        return curr && curr->isWord;
    }
    
    bool startsWith(string prefix) {
        TrieNode* curr = root;
        for (int i = 0; i < prefix.size(); i++) {
            int ltr = prefix[i] - 'a';
            if (!curr->child[ltr]) return false;
            curr = curr->child[ltr];
        }
        return curr;
    }
};
```

---

## Component 1: TrieNode Class

```java
class TrieNode {
    TrieNode[] child;
    boolean isWord;
    
    TrieNode() {
        child = new TrieNode[26];  // a-z
        isWord = false;
    }
}
```

### Structure Explanation

**child array:**
```
child[0] = 'a' ka child
child[1] = 'b' ka child
...
child[25] = 'z' ka child
```

**Logic:** Har index ek letter represent karta hai. Agar `child[0]` not null hai, matlab 'a' character ka path exist karta hai.

**isWord flag:**
- `true` = Is node pe koi word complete hota hai
- `false` = Ye sirf intermediate node hai, word yahan complete nahi hota

**Real-world analogy:** File system jaisa - har folder (node) mein 26 possible subfolders (a-z) ho sakte hain, aur ek flag hai ki ye folder ek complete path hai ya nahi.

---

## Component 2: Trie Class

```java
class Trie {
    TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
}
```

**Logic:** Root node empty hota hai (koi character represent nahi karta). Saare words root se start hote hain.

---

## Operation 1: Insert

```java
public void insert(String word) {
    TrieNode curr = root;
    for (int i = 0; i < word.length(); i++) {
        int index = word.charAt(i) - 'a';
        if (curr.child[index] == null) {
            curr.child[index] = new TrieNode();
        }
        curr = curr.child[index];
    }
    curr.isWord = true;
}
```

### Step-by-Step Insert Logic

**Step 1: Start from root**
```java
TrieNode curr = root;
```

**Step 2: Traverse har character**
```java
for (int i = 0; i < word.length(); i++)
```

**Step 3: Calculate index**
```java
int index = word.charAt(i) - 'a';
```

**Logic:** Character ko index mein convert karna:
- 'a' - 'a' = 0
- 'b' - 'a' = 1
- 'z' - 'a' = 25

**Step 4: Create node if needed**
```java
if (curr.child[index] == null) {
    curr.child[index] = new TrieNode();
}
```

**Logic:** Agar path exist nahi karta, toh naya node create karo.

**Step 5: Move to child**
```java
curr = curr.child[index];
```

**Logic:** Next character ke liye child node pe move karo.

**Step 6: Mark word end**
```java
curr.isWord = true;
```

**Logic:** Last character ke node pe flag set karo ki yahan ek word complete hota hai.

---

### Insert Example: "cat"

```
Initial state: root (empty)

Insert 'c':
  root -> child[2] = new TrieNode (for 'c')
  curr moves to child[2]

Insert 'a':
  curr -> child[0] = new TrieNode (for 'a')
  curr moves to child[0]

Insert 't':
  curr -> child[19] = new TrieNode (for 't')
  curr moves to child[19]

Mark end:
  curr.isWord = true

Final structure:
  root
    |
    c (child[2])
    |
    a (child[0])
    |
    t (child[19], isWord=true)
```

---

### Insert Multiple Words Example

```
Insert: "cat", "car", "card"

After "cat":
  root -> c -> a -> t (isWord=true)

After "car":
  root -> c -> a -> t (isWord=true)
              |
              r (isWord=true)

After "card":
  root -> c -> a -> t (isWord=true)
              |
              r (isWord=true)
              |
              d (isWord=true)

Common prefix "ca" shared by all three words!
```

---

## Operation 2: Search

```java
public boolean search(String word) {
    TrieNode curr = root;
    for (int i = 0; i < word.length(); i++) {
        int index = word.charAt(i) - 'a';
        if (curr.child[index] == null) {
            return false;
        }
        curr = curr.child[index];
    }
    return curr != null && curr.isWord;
}
```

### Search Logic

**Step 1: Traverse path**
```java
for (int i = 0; i < word.length(); i++)
```

**Logic:** Word ke har character ka path follow karo.

**Step 2: Check if path exists**
```java
if (curr.child[index] == null) {
    return false;
}
```

**Logic:** Agar kisi bhi character ka path nahi mila, matlab word exist nahi karta.

**Step 3: Verify word end**
```java
return curr != null && curr.isWord;
```

**Logic:** Path complete hone ke baad check karo ki yahan koi word end hota hai ya nahi.

**Important:** Path exist kar sakta hai but word complete nahi hota (intermediate prefix ho sakta hai).

---

### Search Example

```
Trie contains: "cat", "car", "card"

Search "car":
  c -> found (child[2])
  a -> found (child[0])
  r -> found (child[17])
  curr.isWord = true ✅
  Return true

Search "ca":
  c -> found (child[2])
  a -> found (child[0])
  curr.isWord = false ❌
  Return false (prefix hai but complete word nahi)

Search "cab":
  c -> found (child[2])
  a -> found (child[0])
  b -> not found (child[1] = null) ❌
  Return false
```

---

## Operation 3: StartsWith

```java
public boolean startsWith(String prefix) {
    TrieNode curr = root;
    for (int i = 0; i < prefix.length(); i++) {
        int index = prefix.charAt(i) - 'a';
        if (curr.child[index] == null) {
            return false;
        }
        curr = curr.child[index];
    }
    return curr != null;
}
```

### StartsWith Logic

**Difference from search:** `isWord` check nahi karte, sirf path existence check karte hain.

**Logic:** Agar prefix ka path exist karta hai, matlab koi word us prefix se start hota hai.

---

### StartsWith Example

```
Trie contains: "cat", "car", "card"

startsWith("ca"):
  c -> found (child[2])
  a -> found (child[0])
  Return true ✅ ("cat", "car", "card" sab "ca" se start hote hain)

startsWith("car"):
  c -> found (child[2])
  a -> found (child[0])
  r -> found (child[17])
  Return true ✅ ("car", "card" start hote hain)

startsWith("dog"):
  d -> found? Check child[3]
  If null -> Return false ❌
```

---

## Comparison: Search vs StartsWith

| Operation | Path Check | isWord Check | Use Case |
|-----------|------------|--------------|----------|
| **search** | ✅ Yes | ✅ Yes | Exact word match |
| **startsWith** | ✅ Yes | ❌ No | Prefix match |

**Example:**
```
Trie: "apple"

search("app") = false (path exists but isWord=false)
startsWith("app") = true (path exists, word nahi chahiye)
```

---

## Complete Dry Run

```
Operations:
1. insert("apple")
2. search("apple")
3. search("app")
4. startsWith("app")
5. insert("app")
6. search("app")
```

**After insert("apple"):**
```
root -> a -> p -> p -> l -> e (isWord=true)
```

**search("apple"):**
```
Traverse: a -> p -> p -> l -> e
isWord = true
Return true ✅
```

**search("app"):**
```
Traverse: a -> p -> p
isWord = false (word "app" inserted nahi hai)
Return false ❌
```

**startsWith("app"):**
```
Traverse: a -> p -> p
Path exists!
Return true ✅
```

**After insert("app"):**
```
root -> a -> p -> p (isWord=true) -> l -> e (isWord=true)
```

**search("app"):**
```
Traverse: a -> p -> p
isWord = true (ab "app" bhi complete word hai)
Return true ✅
```

---

## Visual Representation

```
After inserting: "cat", "car", "card", "dog"

                root
               /    \
              c      d
              |      |
              a      o
             / \     |
            t   r    g*
            *   |
                d*

* = isWord flag set (word ends here)

Observations:
- "cat" ends at 't' (isWord=true)
- "car" ends at 'r' (isWord=true)
- "card" ends at 'd' (isWord=true)
- "dog" ends at 'g' (isWord=true)
- 'c' and 'a' nodes have isWord=false (intermediate nodes)
```

---

## Complexity Analysis

### Time Complexity

**Insert:** O(L)
- L = length of word
- Har character ke liye ek node traverse/create

**Search:** O(L)
- L = length of word
- Har character ke liye path check

**StartsWith:** O(L)
- L = length of prefix
- Har character ke liye path check

### Space Complexity

**Overall:** O(N × M × 26)
- N = number of words
- M = average length of words
- 26 = size of child array per node

**Optimization:** Compressed tries ya hash maps use kar sakte ho child storage ke liye.

---

## Edge Cases

1. **Empty string:**
   ```
   insert("")
   search("")
   ```
   Handle karna padega (usually root pe isWord set kar do)

2. **Single character:**
   ```
   insert("a")
   search("a")
   ```
   Normal case, works fine

3. **Overlapping words:**
   ```
   insert("test")
   insert("testing")
   ```
   Both words stored, "test" node has isWord=true, "testing" node has isWord=true

4. **Prefix but not word:**
   ```
   insert("testing")
   search("test") = false
   startsWith("test") = true
   ```

5. **Case sensitivity:**
   Current implementation lowercase only (a-z)
   Uppercase ke liye separate handling chahiye

---

## Interview Tips

1. **Structure explain karo clearly:** "Har node mein 26 children hain (a-z ke liye) aur ek isWord flag hai"

2. **isWord importance:** "Ye differentiate karta hai complete word aur prefix ke beech"

3. **Space optimization mention karo:** "26-size array space consuming hai, hash map use kar sakte hain optimization ke liye"

4. **Use cases bataao:** "Autocomplete, spell checker, IP routing mein use hota hai"

5. **Time complexity justification:** "O(L) hai kyunki word ki length ke equal operations hain, tree height pe depend nahi karta"

6. **Follow-up questions:**
   - Delete operation kaise implement karoge? (Backtrack aur nodes delete if no children)
   - Wildcard search? ('.' character can be any letter - DFS needed)
   - Count words with prefix? (DFS traversal from prefix node)
   - Memory optimization? (Hash map instead of array, compressed trie)

---

## Alternative Implementations

### HashMap-based Children

```java
class TrieNode {
    Map<Character, TrieNode> children;
    boolean isWord;
    
    TrieNode() {
        children = new HashMap<>();
        isWord = false;
    }
}
```

**Advantages:**
- Less memory (only store existing characters)
- Can handle any character set

**Disadvantages:**
- Slightly slower (hash computation)
- More complex implementation

---

## Common Mistakes

1. **isWord flag bhulna:** Search mein path check ke saath isWord check bhi zaruri

2. **Null check na karna:** child[index] null check karna important

3. **Character to index conversion:** `char - 'a'` formula galat lagana

4. **Root initialization:** Constructor mein root node create karna bhulte hain

5. **startsWith mein isWord check:** startsWith mein isWord check nahi karna chahiye

---

## Real-World Applications

1. **Autocomplete:** Google search suggestions - prefix se words suggest karna

2. **Spell Checker:** Dictionary mein word exist karta hai ya nahi

3. **IP Routing:** Network routers mein longest prefix matching

4. **Contact Search:** Phone contacts mein name search by prefix

5. **Text Editor:** Auto-completion aur word suggestions

---

## Key Takeaway

**Trie Structure:** Tree where each path represents a word, common prefixes share nodes.

**Core Operations:**
```
Insert: Create path for each character, mark last node
Search: Traverse path, check if isWord flag set
StartsWith: Traverse path, ignore isWord flag
```

**Key Insight:** Trie efficiently stores words with common prefixes. Space trade-off ke badle fast operations milti hain.