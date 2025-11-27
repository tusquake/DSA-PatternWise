# Construct Binary Tree from Level Order Array

## Problem Kya Hai?

Tumhe ek array diya gaya hai jisme **7 integers** hain. Yeh array ek binary tree ki **level order traversal** represent karta hai.

**Input:**
- `nodes[]` array with 7 integers
- `root` node with value `nodes[0]`

**Task:** Remaining 6 nodes ke liye binary tree construct karo

**Goal:** Array se complete binary tree banana hai

---

## Real-world Analogy

**Building a Family Tree:**
- Tumhe 7 logo ki list di gayi hai
- Pehla person root (grandparent)
- Baki log level-by-level arrange karne hain
- Har parent ke 2 children (left aur right)

**Example:** [1, 2, 3, 4, 5, 6, 7]
- 1 is grandparent
- 2 and 3 are children of 1
- 4, 5 are children of 2
- 6, 7 are children of 3

---

## Level Order Traversal Kya Hai?

**Level Order:** Tree ko level-by-level left to right traverse karo

```
Array: [1, 2, 3, 4, 5, 6, 7]

Tree Structure:
       1           ← Level 0 (index 0)
      / \
     2   3         ← Level 1 (index 1, 2)
    / \ / \
   4 5 6  7        ← Level 2 (index 3, 4, 5, 6)
```

**Traversal Order:** 1 → 2 → 3 → 4 → 5 → 6 → 7

---

## Array Index Pattern

**Parent-Child Relationship:**

For any node at index `i`:
- **Left child** = `2*i + 1`
- **Right child** = `2*i + 2`
- **Parent** = `(i-1) / 2`

### Example Mapping:

```
Index: 0  1  2  3  4  5  6
Array: [1, 2, 3, 4, 5, 6, 7]

Node at index 0 (value=1):
  - Left child  = 2*0+1 = 1 (value=2)
  - Right child = 2*0+2 = 2 (value=3)

Node at index 1 (value=2):
  - Left child  = 2*1+1 = 3 (value=4)
  - Right child = 2*1+2 = 4 (value=5)

Node at index 2 (value=3):
  - Left child  = 2*2+1 = 5 (value=6)
  - Right child = 2*2+2 = 6 (value=7)
```

---

## Visual Representation

```
Array: [10, 20, 30, 40, 50, 60, 70]

Index:  0   1   2   3   4   5   6

Construction Process:

Step 1: Create root (index 0)
       10

Step 2: Add children of 10 (index 1, 2)
       10
      /  \
    20    30

Step 3: Add children of 20 (index 3, 4)
       10
      /  \
    20    30
   /  \
  40   50

Step 4: Add children of 30 (index 5, 6)
       10
      /  \
    20    30
   /  \  /  \
  40 50 60  70

Final Tree Complete! ✅
```

---

## Code Explanation

### Structure Definition:
```cpp
struct node {
    int data;       // Node ki value
    node* left;     // Left child pointer
    node* right;    // Right child pointer
};
```

**Purpose:** Binary tree ka basic structure define karta hai

---

### Helper Function: newNode()
```cpp
node* newNode(int data) {
    node* newNode = new node();
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}
```

**Purpose:** Naya node create karta hai with given data

**Steps:**
1. Memory allocate karo (`new node()`)
2. Data set karo
3. Left aur right pointers ko NULL initialize karo
4. Node return karo

---

### Main Recursive Function: solve()
```cpp
node* solve(vector<int>& vec, int i) {
    if(i >= 7) {
        return NULL;
    }
    
    node* root = newNode(vec[i]);
    i = 2 * i;
    root->left = solve(vec, i + 1);
    root->right = solve(vec, i + 2);
    
    return root;
}
```

---

## Step-by-Step Breakdown

### Variables
```cpp
vector<int>& vec;  // Input array (7 elements)
int i;             // Current index in array
node* root;        // Current node being created
```

---

### Step 1: Base Case - Boundary Check
```cpp
if(i >= 7) {
    return NULL;
}
```

**Condition:** `i >= 7`

**Meaning:** 
- Agar index 7 ya usse zyada hai
- Matlab array ke bahar chale gaye
- No more nodes to create

**Logic:** NULL return karo (no child exists)

**Example:**
```
Array size = 7 (indices 0-6)
If i = 7: Out of bounds → return NULL
If i = 8: Out of bounds → return NULL
```

**Why Important?**
- Prevents array out of bounds access
- Terminates recursion properly
- Handles case jab child nodes nahi hain

---

### Step 2: Create Current Node
```cpp
node* root = newNode(vec[i]);
```

**Logic:** Current index `i` pe jo value hai, usse naya node banao

**Example:**
```
vec = [1, 2, 3, 4, 5, 6, 7]
i = 0 → Create node with value 1
i = 1 → Create node with value 2
i = 2 → Create node with value 3
```

---

### Step 3: Calculate Child Indices
```cpp
i = 2 * i;
```

**Purpose:** Parent index se child indices calculate karne ke liye base value

**Formula Derivation:**
- Left child index = `2*i + 1`
- Right child index = `2*i + 2`
- Common part = `2*i`

**Example:**
```
Parent at i=0: 2*0 = 0
  Left:  0 + 1 = 1
  Right: 0 + 2 = 2

Parent at i=1: 2*1 = 2
  Left:  2 + 1 = 3
  Right: 2 + 2 = 4

Parent at i=2: 2*2 = 4
  Left:  4 + 1 = 5
  Right: 4 + 2 = 6
```

---

### Step 4: Recursively Create Left Child
```cpp
root->left = solve(vec, i + 1);
```

**Logic:** Left child ka index `2*i + 1` hai, recursively create karo

**Recursive Call:**
- New index = `i + 1` (where `i = 2*original_i`)
- Actually calling = `solve(vec, 2*original_i + 1)`
- Returns left subtree ka root

**Example:**
```
Node at index 0:
  i = 2*0 = 0
  Left child = solve(vec, 0+1) = solve(vec, 1)
  Creates node with vec[1] = 2
```

---

### Step 5: Recursively Create Right Child
```cpp
root->right = solve(vec, i + 2);
```

**Logic:** Right child ka index `2*i + 2` hai, recursively create karo

**Recursive Call:**
- New index = `i + 2` (where `i = 2*original_i`)
- Actually calling = `solve(vec, 2*original_i + 2)`
- Returns right subtree ka root

**Example:**
```
Node at index 0:
  i = 2*0 = 0
  Right child = solve(vec, 0+2) = solve(vec, 2)
  Creates node with vec[2] = 3
```

---

### Step 6: Return Constructed Subtree
```cpp
return root;
```

**Logic:** Current node with its left aur right subtrees return karo

**Purpose:** Parent call ko complete subtree milta hai

---

### Wrapper Function: create_tree()
```cpp
void create_tree(node*& root0, vector<int>& vec) {
    root0 = solve(vec, 0);
}
```

**Purpose:** 
- Entry point function
- Index 0 se start karta hai (root)
- Result ko `root0` pointer mein store karta hai

**Why Needed?**
- Clean interface provide karta hai
- User ko index management nahi karni padti
- Reference se root pointer update hota hai

---

## Dry Run Example

### Input:
```cpp
vec = [1, 2, 3, 4, 5, 6, 7]
```

### Execution Trace:

```
Call 1: solve(vec, 0)
  ├─ i=0, vec[0]=1
  ├─ Create node(1)
  ├─ i = 2*0 = 0
  ├─ Left:  solve(vec, 0+1) = solve(vec, 1)
  │   │
  │   Call 2: solve(vec, 1)
  │   ├─ i=1, vec[1]=2
  │   ├─ Create node(2)
  │   ├─ i = 2*1 = 2
  │   ├─ Left:  solve(vec, 2+1) = solve(vec, 3)
  │   │   │
  │   │   Call 3: solve(vec, 3)
  │   │   ├─ i=3, vec[3]=4
  │   │   ├─ Create node(4)
  │   │   ├─ i = 2*3 = 6
  │   │   ├─ Left:  solve(vec, 6+1) = solve(vec, 7)
  │   │   │   │
  │   │   │   Call 4: solve(vec, 7)
  │   │   │   └─ i>=7 → return NULL
  │   │   │
  │   │   ├─ Right: solve(vec, 6+2) = solve(vec, 8)
  │   │   │   │
  │   │   │   Call 5: solve(vec, 8)
  │   │   │   └─ i>=7 → return NULL
  │   │   │
  │   │   └─ Return node(4) with left=NULL, right=NULL
  │   │
  │   ├─ Right: solve(vec, 2+2) = solve(vec, 4)
  │   │   │
  │   │   Call 6: solve(vec, 4)
  │   │   ├─ i=4, vec[4]=5
  │   │   ├─ Create node(5)
  │   │   ├─ i = 2*4 = 8
  │   │   ├─ Left:  solve(vec, 8+1) → NULL
  │   │   ├─ Right: solve(vec, 8+2) → NULL
  │   │   └─ Return node(5) with left=NULL, right=NULL
  │   │
  │   └─ Return node(2) with left=node(4), right=node(5)
  │
  ├─ Right: solve(vec, 0+2) = solve(vec, 2)
  │   │
  │   Call 7: solve(vec, 2)
  │   ├─ i=2, vec[2]=3
  │   ├─ Create node(3)
  │   ├─ i = 2*2 = 4
  │   ├─ Left:  solve(vec, 4+1) = solve(vec, 5)
  │   │   │
  │   │   Call 8: solve(vec, 5)
  │   │   ├─ i=5, vec[5]=6
  │   │   ├─ Create node(6)
  │   │   ├─ i = 2*5 = 10
  │   │   ├─ Left:  solve(vec, 11) → NULL
  │   │   ├─ Right: solve(vec, 12) → NULL
  │   │   └─ Return node(6) with left=NULL, right=NULL
  │   │
  │   ├─ Right: solve(vec, 4+2) = solve(vec, 6)
  │   │   │
  │   │   Call 9: solve(vec, 6)
  │   │   ├─ i=6, vec[6]=7
  │   │   ├─ Create node(7)
  │   │   ├─ i = 2*6 = 12
  │   │   ├─ Left:  solve(vec, 13) → NULL
  │   │   ├─ Right: solve(vec, 14) → NULL
  │   │   └─ Return node(7) with left=NULL, right=NULL
  │   │
  │   └─ Return node(3) with left=node(6), right=node(7)
  │
  └─ Return node(1) with left=node(2), right=node(3)
```

### Final Tree:
```
       1
      / \
     2   3
    / \ / \
   4 5 6  7
```

---

## Detailed Example with Different Input

### Input:
```cpp
vec = [10, 20, 30, 40, 50, 60, 70]
```

### Step-by-Step Construction:

**Call Stack Visualization:**

```
solve(vec, 0) → Creates 10
│
├─ left: solve(vec, 1) → Creates 20
│  │
│  ├─ left: solve(vec, 3) → Creates 40
│  │  ├─ left: solve(vec, 7) → NULL
│  │  └─ right: solve(vec, 8) → NULL
│  │
│  └─ right: solve(vec, 4) → Creates 50
│     ├─ left: solve(vec, 9) → NULL
│     └─ right: solve(vec, 10) → NULL
│
└─ right: solve(vec, 2) → Creates 30
   │
   ├─ left: solve(vec, 5) → Creates 60
   │  ├─ left: solve(vec, 11) → NULL
   │  └─ right: solve(vec, 12) → NULL
   │
   └─ right: solve(vec, 6) → Creates 70
      ├─ left: solve(vec, 13) → NULL
      └─ right: solve(vec, 14) → NULL
```

### Tree Structure:
```
        10
       /  \
     20    30
    /  \  /  \
   40 50 60  70
```

---

## Index Calculation Table

| Parent Index | Parent Value | Left Index | Left Value | Right Index | Right Value |
|--------------|--------------|------------|------------|-------------|-------------|
| 0            | 1            | 1          | 2          | 2           | 3           |
| 1            | 2            | 3          | 4          | 4           | 5           |
| 2            | 3            | 5          | 6          | 6           | 7           |
| 3            | 4            | 7          | NULL       | 8           | NULL        |
| 4            | 5            | 9          | NULL       | 10          | NULL        |
| 5            | 6            | 11         | NULL       | 12          | NULL        |
| 6            | 7            | 13         | NULL       | 14          | NULL        |

---

## Recursion Tree

```
                solve(0)
                /      \
           solve(1)   solve(2)
           /     \     /      \
      solve(3) solve(4) solve(5) solve(6)
       /  \     /  \     /  \     /  \
     NULL NULL NULL NULL NULL NULL NULL NULL
```

**Total Function Calls:** 15
- 7 successful node creations
- 8 NULL returns (base case)

---

## Why This Approach Works?

### Mathematical Foundation:

**Complete Binary Tree Property:**
- For node at index `i`:
  - Left child at `2*i + 1`
  - Right child at `2*i + 2`

**Level Order Storage:**
- Array naturally stores level order
- Index formula directly maps parent-child relationship
- No need for explicit level tracking

**Recursion Benefits:**
- Automatically handles tree construction
- Each call handles one node
- Returns complete subtree

---

## Complexity Analysis

### Time Complexity: **O(N)**

**Explanation:**
- N = 7 nodes in this problem
- Har node exactly ek baar process hota hai
- Har node ke liye:
  - Create node: O(1)
  - Recursive calls: 2 (left + right)
- Total operations: 7 node creations = O(7) = O(N)

**Call Count:**
- Successful calls: 7 (for each node)
- NULL returns: 8 (base case)
- Total recursive calls: 15

**Why O(N)?**
```
T(N) = 1 + T(left) + T(right)
T(N) = 1 + T(N/2) + T(N/2)
T(N) = O(N)
```

---

### Space Complexity: **O(N)** or **O(log N)**

**Two Components:**

1. **Tree Storage: O(N)**
   - 7 nodes created
   - Each node takes constant space
   - Total: O(7) = O(N)

2. **Recursion Stack: O(log N)** or **O(H)**
   - H = height of tree
   - For complete binary tree: H = log₂(N)
   - Maximum depth = 3 (for 7 nodes)
   - Stack frames: O(log₂7) ≈ O(3) = O(log N)

**Recursion Stack Example:**
```
Max Stack Depth = Height + 1

For 7 nodes:
Level 0: 1 node  (depth 0)
Level 1: 2 nodes (depth 1)
Level 2: 4 nodes (depth 2)

Max recursion depth = 3
Stack space = O(3) = O(log 7)
```

**Overall:** O(N) for tree + O(log N) for stack = **O(N)**

---

## Edge Cases

### Case 1: Root Only
```cpp
vec = [5, 0, 0, 0, 0, 0, 0]  // Assuming 0 means no node

Tree:
   5

Only index 0 has valid node
Indices 1-6 either NULL or handled differently
```

### Case 2: All Same Values
```cpp
vec = [1, 1, 1, 1, 1, 1, 1]

Tree:
       1
      / \
     1   1
    / \ / \
   1 1 1  1

All nodes have same value, structure still correct
```

### Case 3: Negative Values
```cpp
vec = [-1, -2, -3, -4, -5, -6, -7]

Tree:
       -1
      /  \
    -2   -3
   / \   / \
 -4 -5 -6 -7

Negative numbers valid, algorithm works same
```

### Case 4: Large Values
```cpp
vec = [1000, 2000, 3000, 4000, 5000, 6000, 7000]

Tree structure unchanged
Large values don't affect algorithm
```

---

## Common Mistakes

### 1. Wrong Index Calculation
```cpp
// ❌ Wrong
root->left = solve(vec, i + 1);   // After i = 2*i
root->right = solve(vec, i + 2);

// If original i=1:
// i = 2*1 = 2
// left = solve(2+1) = solve(3) ✅ Correct
// right = solve(2+2) = solve(4) ✅ Correct
```

**Note:** Code is actually correct! `i = 2*i` first, then `i+1` and `i+2`

### 2. Missing Base Case
```cpp
// ❌ Wrong - No base case
node* solve(vector<int>& vec, int i) {
    node* root = newNode(vec[i]);  // Array out of bounds!
    root->left = solve(vec, 2*i + 1);
    root->right = solve(vec, 2*i + 2);
    return root;
}
```

**Problem:** Infinite recursion, array access violation

### 3. Wrong Reference
```cpp
// ❌ Wrong - Missing reference
void create_tree(node* root0, vector<int>& vec) {
    root0 = solve(vec, 0);  // Changes local copy only
}

// ✅ Correct - Using reference
void create_tree(node*& root0, vector<int>& vec) {
    root0 = solve(vec, 0);  // Updates original pointer
}
```

### 4. Memory Leaks
```cpp
// ❌ Wrong - Memory not managed
node* temp = newNode(5);
// Never deleted → memory leak

// ✅ Better - Use smart pointers or proper cleanup
unique_ptr<node> temp = make_unique<node>();
```

---

## Alternative Approaches

### Approach 1: Iterative (Using Queue)
```cpp
node* createTreeIterative(vector<int>& vec) {
    if(vec.empty()) return NULL;
    
    node* root = newNode(vec[0]);
    queue<node*> q;
    q.push(root);
    int i = 1;
    
    while(!q.empty() && i < vec.size()) {
        node* curr = q.front();
        q.pop();
        
        // Left child
        if(i < vec.size()) {
            curr->left = newNode(vec[i++]);
            q.push(curr->left);
        }
        
        // Right child
        if(i < vec.size()) {
            curr->right = newNode(vec[i++]);
            q.push(curr->right);
        }
    }
    
    return root;
}
```

**Pros:**
- No recursion stack overhead
- More intuitive for some people
- Better for very large trees

**Cons:**
- Requires extra queue space
- More code to write

---

### Approach 2: Direct Formula (Current Approach)
```cpp
node* solve(vector<int>& vec, int i) {
    if(i >= vec.size()) return NULL;
    
    node* root = newNode(vec[i]);
    root->left = solve(vec, 2*i + 1);
    root->right = solve(vec, 2*i + 2);
    
    return root;
}
```

**Optimized Version:** Works for any size, not just 7 nodes

---

## Comparison Table

| Aspect | Recursive | Iterative |
|--------|-----------|-----------|
| Code Lines | Fewer | More |
| Space (Stack) | O(log N) | O(1) |
| Space (Queue) | O(1) | O(N) |
| Readability | High | Medium |
| Debug Ease | Medium | Easy |
| Performance | Similar | Similar |

---

## Interview Tips

### 1. Explain the Index Formula
```
Parent at index i:
  Left child  = 2*i + 1
  Right child = 2*i + 2

Parent of node at j = (j-1)/2
```

### 2. Discuss Modifications
- **Variable size array?** Change `if(i >= 7)` to `if(i >= vec.size())`
- **NULL values in array?** Add check for special value (e.g., -1)
- **Return specific node?** Add search logic

### 3. Follow-up Questions
- **Q:** How to handle NULL values in array?
- **A:** Use sentinel value (-1), check before creating node

- **Q:** Print level order after construction?
- **A:** BFS traversal with queue

- **Q:** Convert to iterative?
- **A:** Use queue for level order construction

### 4. Time/Space Trade-offs
- Recursive: Less code, stack space
- Iterative: More code, queue space
- Both: O(N) time

---

## Related Problems

1. **Level Order Traversal** - Print tree level by level
2. **Serialize/Deserialize Binary Tree** - Convert tree to array and back
3. **Check Complete Binary Tree** - Verify tree structure
4. **Find Height of Tree** - Calculate tree depth
5. **Print Ancestors** - Find path from root to node

---

## Key Takeaways

| Concept | Value |
|---------|-------|
| **Problem** | Construct tree from level order array |
| **Approach** | Recursive using index formula |
| **Key Formula** | Left = 2*i+1, Right = 2*i+2 |
| **Time Complexity** | O(N) |
| **Space Complexity** | O(N) tree + O(log N) stack |
| **Base Case** | i >= array_size |
| **Trick** | i = 2*i before recursive calls |

---

## Visual Summary

```
Array → Tree Mapping

Index:  0   1   2   3   4   5   6
Array: [1,  2,  3,  4,  5,  6,  7]
        ↓   ↓   ↓   ↓   ↓   ↓   ↓
Tree:        1
           /   \
          2     3
         / \   / \
        4   5 6   7

Formula: child_index = 2 * parent_index + (1 or 2)
```

---

## Summary

**Problem:** Array se binary tree construct karo

**Solution:** Recursion with index formula
- Start from index 0
- Left child = 2*i + 1
- Right child = 2*i + 2
- Base case: i >= size

**Key Insight:** Complete binary tree ka level order array mein natural mapping hai

**Result:** O(N) time mein complete tree ready!
