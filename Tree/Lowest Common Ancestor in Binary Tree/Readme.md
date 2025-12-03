# Lowest Common Ancestor (LCA) in Binary Tree

## Problem Kya Hai?

Binary tree mein **do nodes p aur q** diye hain. Unka **Lowest Common Ancestor (LCA)** find karo.

**LCA:** Sabse neeche wala node jo **p aur q dono ka parent/ancestor** hai.

---

## Examples

### Example 1:
```
Tree:
        3
       / \
      5   1
     / \ / \
    6 2 0  8

p = 5, q = 1
LCA = 3 ✓ (dono 3 ke neeche hain)
```

### Example 2:
```
Same tree
p = 5, q = 4 (4 is child of 5)
LCA = 5 ✓ (5 khud ancestor hai)
```

### Example 3:
```
Same tree
p = 6, q = 2
LCA = 5 ✓ (dono 5 ke children)
```

---

## Approach - Recursive

**Idea:** Root se search karo, teen cases handle karo

**Cases:**
1. Root khud p ya q hai → root return karo
2. p aur q **different sides** mein → root is LCA
3. p aur q **same side** mein → wo side return karo

---

## Code

### C++:
```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    // Base case: NULL or found p or q
    if (root == NULL || root == p || root == q) 
        return root;
    
    // Search in both subtrees
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    
    // Case 1: Both found in different subtrees
    if (left != NULL && right != NULL) 
        return root;
    
    // Case 2: Only one side found
    return (left != NULL) ? left : right;
}
```

### Java:
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Base case
        if (root == null || root == p || root == q) 
            return root;
        
        // Search both sides
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        
        // Both sides have results
        if (left != null && right != null) 
            return root;
        
        // Return non-null side
        return left != null ? left : right;
    }
}
```

---

## Step-by-Step Breakdown

### Step 1: Base Case
```cpp
if (root == NULL || root == p || root == q) return root;
```
- **NULL** → kuch nahi mila, return NULL
- **root == p or q** → ek target node mil gaya, return karo

### Step 2: Search Both Subtrees
```cpp
TreeNode* left = lowestCommonAncestor(root->left, p, q);
TreeNode* right = lowestCommonAncestor(root->right, p, q);
```
- Left subtree mein recursively search
- Right subtree mein recursively search

### Step 3: Analyze Results (Three Cases)
```cpp
if (left == NULL) return right;   // Case A
if (right == NULL) return left;   // Case B
else return root;                 // Case C
```

**Case A:** `left == NULL`
- Left subtree mein kuch nahi mila
- Dono nodes right side mein hain
- Return right result

**Case B:** `right == NULL`
- Right subtree mein kuch nahi mila
- Dono nodes left side mein hain
- Return left result

**Case C:** Both NOT NULL
- Left subtree mein ek node mila (p ya q)
- Right subtree mein dusra node mila (q ya p)
- Different sides → **current root is LCA**

---

## Dry Run

### Input:
```
Tree:
        3
       / \
      5   1
     /
    6

p = 5, q = 6
```

### Execution:
```
Call: LCA(3, 5, 6)
│
├─ left = LCA(5, 5, 6)
│  ├─ root == 5 (found p!)
│  └─ return 5 ✓
│
├─ right = LCA(1, 5, 6)
│  ├─ LCA(NULL) → return NULL
│  └─ return NULL
│
├─ Analysis:
│  ├─ left = 5 (not NULL)
│  ├─ right = NULL
│  └─ return left = 5 ✓

Result: 5 (5 is ancestor of 6)
```

---

## Dry Run 2

### Input:
```
Tree:
        3
       / \
      5   1
     / \
    6   2

p = 6, q = 2
```

### Execution:
```
Call: LCA(3, 6, 2)
│
├─ left = LCA(5, 6, 2)
│  │
│  ├─ left = LCA(6, 6, 2)
│  │  └─ root == 6 → return 6
│  │
│  ├─ right = LCA(2, 6, 2)
│  │  └─ root == 2 → return 2
│  │
│  ├─ left = 6 (not NULL)
│  ├─ right = 2 (not NULL)
│  └─ return 5 ✓ (both sides found!)
│
├─ right = LCA(1, 6, 2)
│  └─ return NULL
│
├─ left = 5, right = NULL
└─ return 5 ✓

Result: 5 (common ancestor of 6 and 2)
```

---

## Dry Run 3

### Input:
```
Tree:
        3
       / \
      5   1

p = 5, q = 1
```

### Execution:
```
Call: LCA(3, 5, 1)
│
├─ left = LCA(5, 5, 1)
│  └─ root == 5 → return 5
│
├─ right = LCA(1, 5, 1)
│  └─ root == 1 → return 1
│
├─ left = 5 (not NULL)
├─ right = 1 (not NULL)
└─ return 3 ✓ (different sides!)

Result: 3 (both in different subtrees)
```

---

## Why This Works?

### Key Insight:
```
When we find p or q:
- Return it immediately (base case)
- This bubbles up through recursion

Three scenarios at any node:
1. Both left and right NOT NULL → Different sides → This node is LCA
2. Only left NOT NULL → Both in left → Pass up left result
3. Only right NOT NULL → Both in right → Pass up right result
```

### Visual:
```
        ROOT
       /    \
     p?      q?    → Different sides → ROOT is LCA
     
        ROOT
       /    \
     p,q     ø     → Same side (left) → Pass left up
     
        ROOT
       /    \
      ø     p,q    → Same side (right) → Pass right up
```

---

## Complexity

**Time:** O(N)
- Visit each node at most once
- N = number of nodes

**Space:** O(H)
- Recursion stack depth
- H = height of tree
- Worst case (skewed): O(N)
- Best case (balanced): O(log N)

---

## Edge Cases

```
Both nodes same:
p = q = 5 → LCA = 5

One node is ancestor of other:
p = 5, q = 6 (6 is child of 5)
LCA = 5

Root is one of the nodes:
p = root, q = any
LCA = root

Nodes at same level:
p = 6, q = 2 (both children of 5)
LCA = 5
```

---

## Key Takeaways

| Aspect | Value |
|--------|-------|
| **Approach** | Recursive post-order |
| **Time** | O(N) |
| **Space** | O(H) recursion |
| **Key Logic** | Check both sides, return appropriately |
| **Critical** | Handle base case properly |

---

## Summary

```
Problem: Find lowest common ancestor of two nodes

Solution: Recursive search
1. Base: If NULL or found p/q → return root
2. Search: Both left and right subtrees
3. Decide:
   - Both sides found → root is LCA
   - One side found → return that side
   
Key: Post-order traversal (check children first)
```