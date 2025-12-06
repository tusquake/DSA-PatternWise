# 🚀 Two-Pointer Technique - Ultimate Cheat Sheet

A comprehensive guide to mastering the two-pointer technique for coding interviews and competitive programming.

---

## 📋 Table of Contents

- [Introduction](#introduction)
- [Pattern 1: Opposite Ends (Meet in Middle)](#pattern-1-opposite-ends-meet-in-middle)
- [Pattern 2: Sliding Window](#pattern-2-sliding-window)
- [Pattern 3: Fast-Slow Pointer](#pattern-3-fast-slow-pointer)
- [Pattern 4: Merge Pattern](#pattern-4-merge-pattern)
- [Pattern 5: Same-Array Two Pointers (Read + Write)](#pattern-5-same-array-two-pointers-read--write)
- [Pattern 6: Palindrome / Symmetry](#pattern-6-palindrome--symmetry)
- [Quick Reference Table](#quick-reference-table)
- [Pattern Selection Guide](#pattern-selection-guide)

---

## Introduction

The **two-pointer technique** is a powerful algorithmic pattern that uses two pointers to traverse data structures efficiently. It typically reduces time complexity from O(n²) to O(n) and space complexity to O(1).

### When to Use Two-Pointer Technique
- Working with sorted or partially sorted arrays
- Finding pairs, triplets, or subsequences
- Optimizing nested loops
- Working with linked lists
- String/array manipulation in-place

---

## Pattern 1: Opposite Ends (Meet in Middle)

### 🎯 When to Use
- Array is **sorted** (or can be sorted)
- Finding **pairs** or **triplets** with specific sum
- Need to find sum that is `<`, `>`, or `==` target value
- Optimizing container problems

### 🔧 How It Works
```
Initialize: left = 0, right = n-1
Move pointers inward based on condition:
  - if sum < target → left++
  - if sum > target → right--
  - if sum == target → found (or continue searching)
```

### 💡 Pointer Movement Logic
```python
left = 0
right = len(arr) - 1

while left < right:
    current_sum = arr[left] + arr[right]
    
    if current_sum < target:
        left += 1      # Need larger sum
    elif current_sum > target:
        right -= 1     # Need smaller sum
    else:
        # Found target
        return [left, right]
```

### 📝 Common Problems
- **Two Sum II** (sorted array)
- **3Sum** (find three numbers that sum to zero)
- **4Sum** (find four numbers that sum to target)
- **Container With Most Water** (maximize area)
- **Trapping Rain Water** (calculate trapped water)
- **Pair with Target Sum**
- **Remove Duplicates from Sorted Array**

### ⚡ Time Complexity
- **Time**: O(n) for single pass, O(n²) for nested (3Sum)
- **Space**: O(1)

---

## Pattern 2: Sliding Window

### 🎯 When to Use
- Keywords: **longest**, **shortest**, **at most K**, **at least K**, **window**, **substring**
- Working with strings or arrays
- Frequency or count constraints
- Need to find optimal subarray/substring

### 🔧 How It Works
```
Initialize: left = 0, right = 0
Expand window by moving right pointer
Shrink window by moving left pointer when condition violated
Track the best answer during traversal
```

### 💡 Pointer Movement Logic
```python
left = 0
result = 0
window_data = {}  # Could be set, dict, counter

for right in range(len(arr)):
    # Expand window: include arr[right]
    window_data[arr[right]] = window_data.get(arr[right], 0) + 1
    
    # Shrink window: move left while invalid
    while window_invalid():
        window_data[arr[left]] -= 1
        left += 1
    
    # Update result with valid window
    result = max(result, right - left + 1)
```

### 📝 Common Problems
- **Longest Substring Without Repeating Characters**
- **Minimum Window Substring**
- **Longest Substring with K Distinct Characters**
- **Maximum Consecutive Ones III**
- **Fruit Into Baskets**
- **Subarray Sum Equals K**
- **Longest Repeating Character Replacement**
- **Permutation in String**

### ⚡ Time Complexity
- **Time**: O(n)
- **Space**: O(k) where k is number of unique elements in window

---

## Pattern 3: Fast-Slow Pointer

### 🎯 When to Use
- Working with **linked lists**
- Detecting **cycles**
- Finding **middle element**
- Removing Nth node from end
- Checking for palindrome in linked list

### 🔧 How It Works
```
Initialize: slow = head, fast = head
Fast moves twice as fast as slow
When fast reaches end, slow is at middle
If fast meets slow, cycle exists
```

### 💡 Pointer Movement Logic
```python
slow = head
fast = head

# Find middle or detect cycle
while fast and fast.next:
    slow = slow.next        # Move 1 step
    fast = fast.next.next   # Move 2 steps
    
    if slow == fast:
        # Cycle detected
        return True

# If loop exits normally, no cycle
return False
```

### 📝 Common Problems
- **Linked List Cycle** (detect cycle)
- **Linked List Cycle II** (find cycle start)
- **Middle of Linked List**
- **Palindrome Linked List**
- **Remove Nth Node From End**
- **Happy Number**
- **Reorder List**

### ⚡ Time Complexity
- **Time**: O(n)
- **Space**: O(1)

---

## Pattern 4: Merge Pattern

### 🎯 When to Use
- Two or more **sorted** arrays/lists
- Need to **merge** while maintaining order
- Interval merging problems
- Comparing elements from multiple sources

### 🔧 How It Works
```
Initialize: i = 0, j = 0
Compare elements at both pointers
Take smaller element and advance that pointer
Handle remaining elements after one exhausts
```

### 💡 Pointer Movement Logic
```python
i, j = 0, 0
result = []

while i < len(arr1) and j < len(arr2):
    if arr1[i] <= arr2[j]:
        result.append(arr1[i])
        i += 1
    else:
        result.append(arr2[j])
        j += 1

# Append remaining elements
result.extend(arr1[i:])
result.extend(arr2[j:])
```

### 📝 Common Problems
- **Merge Sorted Array**
- **Merge Two Sorted Lists**
- **Merge K Sorted Lists**
- **Intersection of Two Arrays**
- **Merge Intervals**
- **Insert Interval**
- **Interval List Intersections**

### ⚡ Time Complexity
- **Time**: O(n + m) for two arrays
- **Space**: O(1) if modifying in-place, O(n+m) for new array

---

## Pattern 5: Same-Array Two Pointers (Read + Write)

### 🎯 When to Use
- **In-place** array modification required
- Removing duplicates or specific elements
- Moving elements (zeros to end)
- Compressing arrays
- Partitioning arrays

### 🔧 How It Works
```
Initialize: read = 0, write = 0
Read pointer scans through array
Write pointer marks position for valid elements
Copy valid elements from read to write position
```

### 💡 Pointer Movement Logic
```python
write = 0

for read in range(len(arr)):
    if is_valid(arr[read]):
        arr[write] = arr[read]
        write += 1
    # else: skip invalid element

# Final array length is write
return write
```

### 📝 Common Problems
- **Remove Duplicates from Sorted Array**
- **Remove Element**
- **Move Zeroes**
- **Sort Colors** (Dutch National Flag)
- **Remove Duplicates from Sorted Array II**
- **Partition Array**
- **Squares of Sorted Array**

### ⚡ Time Complexity
- **Time**: O(n)
- **Space**: O(1)

---

## Pattern 6: Palindrome / Symmetry

### 🎯 When to Use
- Checking if string/array is **palindrome**
- Reversing elements (vowels, specific chars)
- Comparing left-right symmetry
- Subsequence matching
- Two-way validation

### 🔧 How It Works
```
Initialize: left = 0, right = n-1
Move pointers inward
Compare elements at both ends
Skip or process based on condition
```

### 💡 Pointer Movement Logic
```python
left = 0
right = len(s) - 1

while left < right:
    # Skip non-alphanumeric (if needed)
    while left < right and not s[left].isalnum():
        left += 1
    while left < right and not s[right].isalnum():
        right -= 1
    
    # Compare
    if s[left].lower() != s[right].lower():
        return False
    
    left += 1
    right -= 1

return True
```

### 📝 Common Problems
- **Valid Palindrome**
- **Valid Palindrome II** (delete at most one char)
- **Reverse String**
- **Reverse Vowels of a String**
- **Is Subsequence**
- **Two Sum II** (variation)
- **Reverse Words in a String**

### ⚡ Time Complexity
- **Time**: O(n)
- **Space**: O(1)

---

## Quick Reference Table

| Pattern | Use When | Initialization | Movement | Time | Space |
|---------|----------|---------------|----------|------|-------|
| **Opposite Ends** | Sorted array, find pairs/sum | `l=0, r=n-1` | Move based on sum comparison | O(n) | O(1) |
| **Sliding Window** | Longest/shortest substring | `l=0, r=0` | Expand right, shrink left | O(n) | O(k) |
| **Fast-Slow** | Linked list, cycles, middle | `slow=head, fast=head` | `slow+1, fast+2` | O(n) | O(1) |
| **Merge** | Multiple sorted inputs | `i=0, j=0` | Compare & take smaller | O(n+m) | O(1) |
| **Read-Write** | In-place modifications | `read=0, write=0` | Write valid, skip invalid | O(n) | O(1) |
| **Palindrome** | Symmetry checks | `l=0, r=n-1` | Move inward, compare | O(n) | O(1) |

---

## Pattern Selection Guide

### 🔍 Decision Tree

```
Is the array sorted?
├─ YES → Consider Pattern 1 (Opposite Ends) or Pattern 4 (Merge)
└─ NO
   ├─ Need to find longest/shortest substring? → Pattern 2 (Sliding Window)
   ├─ Working with linked list? → Pattern 3 (Fast-Slow)
   ├─ Need in-place modification? → Pattern 5 (Read-Write)
   └─ Checking symmetry/palindrome? → Pattern 6 (Palindrome)
```

### 📌 Key Questions to Ask

1. **Is the input sorted?** → Opposite Ends or Merge
2. **Do I need to find a window?** → Sliding Window
3. **Am I working with a linked list?** → Fast-Slow
4. **Do I need to modify in-place?** → Read-Write
5. **Am I comparing from both ends?** → Palindrome

### 🎯 Keywords to Watch For

| Keywords | Pattern |
|----------|---------|
| sorted, pair, triplet, sum | Opposite Ends |
| longest, shortest, window, at most K | Sliding Window |
| cycle, middle, linked list | Fast-Slow |
| merge, combine, sorted lists | Merge |
| remove, move, in-place | Read-Write |
| palindrome, reverse, symmetry | Palindrome |

---

## 💡 Pro Tips

1. **Always check for edge cases**: empty arrays, single element, all same elements
2. **Draw it out**: visualize pointer movements on paper
3. **Consider sorting**: sometimes sorting first enables two-pointer approach
4. **Watch for infinite loops**: ensure pointers always move toward termination
5. **Template recognition**: recognize which pattern applies within seconds
6. **Practice variations**: each pattern has multiple problem variations

---

## 📚 Practice Strategy

### Beginner Level
- Two Sum II (Opposite Ends)
- Valid Palindrome (Palindrome)
- Merge Sorted Array (Merge)
- Remove Duplicates (Read-Write)

### Intermediate Level
- 3Sum (Opposite Ends)
- Longest Substring Without Repeating (Sliding Window)
- Linked List Cycle II (Fast-Slow)
- Container With Most Water (Opposite Ends)

### Advanced Level
- Minimum Window Substring (Sliding Window)
- Trapping Rain Water (Opposite Ends)
- Merge K Sorted Lists (Merge)
- Palindrome Linked List (Fast-Slow + Palindrome)

---

## 🎓 Summary

**Two-pointer technique is your go-to optimization when you see:**
- Sorted arrays and finding pairs/sums
- Substring/subarray optimization problems
- Linked list traversal and cycle detection
- Merging multiple sorted inputs
- In-place array modifications
- Palindrome and symmetry checks
