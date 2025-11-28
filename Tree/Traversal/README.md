# Binary Tree Traversals (Preorder, Inorder, Postorder)

## Problem Kya Hai?

Binary tree ko 3 different orders mein traverse karna hai.

**Goal:** Tree nodes ko specific order mein visit karo aur values return karo

---

## Three Traversal Orders

| Traversal | Order | Result for Same Tree |
|-----------|-------|---------------------|
| **Preorder** | Root → Left → Right | [1, 2, 4, 5, 3] |
| **Inorder** | Left → Root → Right | [4, 2, 5, 1, 3] |
| **Postorder** | Left → Right → Root | [4, 5, 2, 3, 1] |

### Tree:
```
        1
       / \
      2   3
     / \
    4   5
```

---

## 1. Preorder (Root → Left → Right)

### Code:
```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> ans;
    preorderSolve(root, ans);
    return ans;
}

void preorderSolve(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    
    ans.push_back(root->val);         // 1. Root FIRST
    preorderSolve(root->left, ans);   // 2. Then left
    preorderSolve(root->right, ans);  // 3. Then right
}
```

### Key Point:
- `ans.push_back()` **pehle** hota hai (before recursive calls)
- Root ko pehle process karo

### Visual:
```
        1           ← Visit 1st
       / \
      2   3         ← Visit 2nd, 5th
     / \
    4   5           ← Visit 3rd, 4th

Order: 1 → 2 → 4 → 5 → 3
```

### Use Cases:
- Copy/Clone tree
- Prefix expression: `+ 3 4`
- Tree serialization

---

## 2. Inorder (Left → Root → Right)

### Code:
```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> ans;
    inorderSolve(root, ans);
    return ans;
}

void inorderSolve(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    
    inorderSolve(root->left, ans);    // 1. Left first
    ans.push_back(root->val);         // 2. Root MIDDLE
    inorderSolve(root->right, ans);   // 3. Then right
}
```

### Key Point:
- `ans.push_back()` **between** recursive calls
- Root ko middle mein process karo

### Visual:
```
        1           ← Visit 4th
       / \
      2   3         ← Visit 2nd, 5th
     / \
    4   5           ← Visit 1st, 3rd

Order: 4 → 2 → 5 → 1 → 3
```

### Use Cases:
- **BST sorted order** (most important!)
- Infix expression: `3 + 4`
- Validate BST

### Special Property:
```
BST:        50
           /  \
          30  70
         
Inorder: [30, 50, 70] ← SORTED!
```

---

## 3. Postorder (Left → Right → Root)

### Code:
```cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> ans;
    postorderSolve(root, ans);
    return ans;
}

void postorderSolve(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    
    postorderSolve(root->left, ans);   // 1. Left first
    postorderSolve(root->right, ans);  // 2. Then right
    ans.push_back(root->val);          // 3. Root LAST
}
```

### Key Point:
- `ans.push_back()` **last** (after recursive calls)
- Root ko last mein process karo

### Visual:
```
        1           ← Visit 5th (last)
       / \
      2   3         ← Visit 3rd, 4th
     / \
    4   5           ← Visit 1st, 2nd

Order: 4 → 5 → 2 → 3 → 1
```

### Use Cases:
- Delete tree (children first, parent last)
- Postfix expression: `3 4 +`
- Calculate tree height (bottom-up)

---

## Code Comparison - ONE Pattern!

```cpp
void traverse(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    
    ans.push_back(root->val);  // ← PREORDER (before)
    
    traverse(root->left, ans);
    
    ans.push_back(root->val);  // ← INORDER (middle)
    
    traverse(root->right, ans);
    
    ans.push_back(root->val);  // ← POSTORDER (after)
}
```

**Only Difference:** Position of `ans.push_back()`!

---

## Dry Run Example

### Tree:
```
        10
       /  \
      20  30
     /
    40
```

### Preorder (Root → Left → Right):
```
Visit: 10 → 20 → 40 → 30
Result: [10, 20, 40, 30]
```

### Inorder (Left → Root → Right):
```
Visit: 40 → 20 → 10 → 30
Result: [40, 20, 10, 30]
```

### Postorder (Left → Right → Root):
```
Visit: 40 → 20 → 30 → 10
Result: [40, 20, 30, 10]
```

---

## Step-by-Step Breakdown

### Variables:
```cpp
TreeNode* root;        // Current node
vector<int>& ans;      // Result (by reference)
```

### Step 1: Base Case
```cpp
if (!root) return;
```
- NULL node hai → return
- Prevents crashes
- Handles leaf children

### Step 2: Process Root (Position varies!)
```cpp
ans.push_back(root->val);
```
- **Preorder:** Do this FIRST
- **Inorder:** Do this MIDDLE (between left/right calls)
- **Postorder:** Do this LAST

### Step 3: Traverse Left
```cpp
solve(root->left, ans);
```
- Recursively process left subtree

### Step 4: Traverse Right
```cpp
solve(root->right, ans);
```
- Recursively process right subtree

---

## Complexity Analysis

### Time Complexity: **O(N)**
- Visit each node exactly once
- N nodes = O(N)

### Space Complexity: **O(H)**
- H = Height of tree
- Recursion stack depth
- Balanced tree: O(log N)
- Skewed tree: O(N)

---

## Memory Trick

```
PRE-order:   Root BEFORE (pehle)   → [ROOT] left right
IN-order:    Root IN middle         → left [ROOT] right  
POST-order:  Root AFTER (baad)      → left right [ROOT]
```

---

## When to Use Which?

| Use Case | Traversal | Why |
|----------|-----------|-----|
| Get sorted values (BST) | **Inorder** | Left-Root-Right = sorted |
| Copy tree | **Preorder** | Create parent first |
| Delete tree | **Postorder** | Delete children first |
| Prefix notation | **Preorder** | `+ 3 4` |
| Infix notation | **Inorder** | `3 + 4` |
| Postfix notation | **Postorder** | `3 4 +` |

---

## Common Mistakes

### 1. Wrong Order
```cpp
// ❌ Wrong Preorder (This is Inorder!)
void preorder(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    preorder(root->left, ans);
    ans.push_back(root->val);  // Middle = Inorder!
    preorder(root->right, ans);
}
```

### 2. Missing Base Case
```cpp
// ❌ Wrong
void inorder(TreeNode* root, vector<int>& ans) {
    inorder(root->left, ans);  // Crashes on NULL!
    ans.push_back(root->val);
}
```

### 3. Not Using Reference
```cpp
// ❌ Wrong
void solve(TreeNode* root, vector<int> ans) { // Copy created!

// ✅ Correct
void solve(TreeNode* root, vector<int>& ans) { // Reference
```

---

## Edge Cases

### Empty Tree:
```
Input: NULL
All output: []
```

### Single Node:
```
Input:    5
Preorder:  [5]
Inorder:   [5]
Postorder: [5]
```

### Left Skewed:
```
Input:  1
       /
      2
     /
    3

Preorder:  [1, 2, 3]
Inorder:   [3, 2, 1]
Postorder: [3, 2, 1]
```

---

## Iterative Approach (Preorder)

```cpp
vector<int> preorderIterative(TreeNode* root) {
    vector<int> ans;
    if (!root) return ans;
    
    stack<TreeNode*> st;
    st.push(root);
    
    while (!st.empty()) {
        TreeNode* curr = st.top();
        st.pop();
        ans.push_back(curr->val);
        
        if (curr->right) st.push(curr->right);  // Right first
        if (curr->left) st.push(curr->left);    // Then left
    }
    
    return ans;
}
```

**Why right first?** Stack is LIFO, so left will be processed first!

---

## Interview Tips

### 1. Remember the Order
```
Preorder:  Root FIRST
Inorder:   Root MIDDLE
Postorder: Root LAST
```

### 2. BST Special Case
```
Inorder of BST = SORTED order ← Frequently asked!
```

### 3. Quick Test
```
Given: [1, 2, 4, 5, 3]
Root is 1 and appears first → Preorder ✓

Given: [4, 2, 5, 1, 3]
Root is 1 and appears middle → Inorder ✓

Given: [4, 5, 2, 3, 1]
Root is 1 and appears last → Postorder ✓
```

### 4. Follow-up Ready
- **Iterative version?** Use stack
- **O(1) space?** Morris traversal
- **Print instead of store?** Replace push_back with cout

---

## Key Takeaways

| Aspect | Value |
|--------|-------|
| **Only Difference** | Position of `ans.push_back()` |
| **Time** | O(N) for all three |
| **Space** | O(H) recursion stack |
| **BST Property** | Inorder = SORTED |
| **Delete Tree** | Use Postorder |
| **Copy Tree** | Use Preorder |

---

## Summary

```cpp
// Master pattern - just change push_back position!

void solve(TreeNode* root, vector<int>& ans) {
    if (!root) return;
    
    // Preorder:  ans.push_back(root->val);
    solve(root->left, ans);
    // Inorder:   ans.push_back(root->val);
    solve(root->right, ans);
    // Postorder: ans.push_back(root->val);
}
```
