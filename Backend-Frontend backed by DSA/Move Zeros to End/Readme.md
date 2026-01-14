# Move Zeros to End - Real World Applications

## Problem Statement

Given an integer array, move all zeros to the end while maintaining the relative order of non-zero elements. Modify the array in-place.

**Example 1:**
```
Input:  [0, 1, 0, 3, 12]
Output: [1, 3, 12, 0, 0]
```

**Example 2:**
```
Input:  [0, 0, 1]
Output: [1, 0, 0]
```

**Constraints:**
- Must modify array in-place (no extra array)
- Maintain relative order of non-zero elements
- Time Complexity: O(n)
- Space Complexity: O(1)

**Solution (Java):**
```java
public void moveZeroes(int[] nums) {
    int writeIndex = 0;
    
    // Move all non-zero elements to front
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            nums[writeIndex] = nums[i];
            writeIndex++;
        }
    }
    
    // Fill remaining positions with zeros
    for (int i = writeIndex; i < nums.length; i++) {
        nums[i] = 0;
    }
}
```

---

## Core Concept

This problem is NOT about zeros. It teaches:

- In-place array manipulation
- Partitioning elements without extra space
- Maintaining relative order while filtering
- Two-pointer technique for efficient reorganization

Any place where you need to separate active/inactive items, filter out invalid entries, or reorganize data without creating copies - this pattern applies.

---

## Real-World Use Cases

### 1. Soft Delete

**Problem:** Mark records as deleted without removing them from the array/list

**Example:** User management in a dashboard
```javascript
users = [
  { id: 1, name: "Alice", deleted: false },
  { id: 2, name: "Bob", deleted: true },    // soft deleted
  { id: 3, name: "Carol", deleted: false },
  { id: 4, name: "Dave", deleted: true }    // soft deleted
]
```

**Challenge:**
- Display active users first
- Keep deleted users at the end
- Don't create a new array (memory efficient)
- Maintain insertion order of active users

**Solution Pattern (Move Zeros Logic):**
```javascript
// Two pointer approach
let writeIndex = 0;

for (let i = 0; i < users.length; i++) {
    if (!users[i].deleted) {  // if not "zero" (deleted)
        [users[writeIndex], users[i]] = [users[i], users[writeIndex]];
        writeIndex++;
    }
}
// Result: Active users first, deleted users at end
```

**Mapping:**

| Move Zeros Problem | Soft Delete |
|-------------------|-------------|
| Non-zero elements | Active records |
| Zero elements | Deleted records |
| Move to front | Keep active first |
| Move to end | Push deleted to end |
| Relative order preserved | Insertion order maintained |

### 2. Active/Inactive Records in Tables

**Example:** Equipment maintenance list (Backend - Java)
```java
// Service method
public void reorderEquipment(List<Equipment> equipment) {
    int activeIndex = 0;
    
    for (int i = 0; i < equipment.size(); i++) {
        if (equipment.get(i).isActive()) {
            Collections.swap(equipment, activeIndex, i);
            activeIndex++;
        }
    }
    // Active equipment now at front, inactive at end
}
```

**Use Case:**
- Show active equipment first in dropdown
- Keep inactive at bottom for reference
- No database re-query needed

### 3. Valid/Invalid Form Fields (UI)

**Example:** Form validation
```javascript
formFields = [
  { name: "email", valid: true },
  { name: "phone", valid: false },    // invalid
  { name: "address", valid: true },
  { name: "zip", valid: false }       // invalid
]

// Reorder: valid fields first
let validIndex = 0;
for (let i = 0; i < formFields.length; i++) {
    if (formFields[i].valid) {
        [formFields[validIndex], formFields[i]] = [formFields[i], formFields[validIndex]];
        validIndex++;
    }
}
```

**Benefit:**
- Show valid fields first
- Display invalid fields at bottom with error messages
- Efficient UI rendering without array copies

### 4. Priority-Based Task Lists (Backend)

**Example:** Job queue processing (Java)
```java
public void processTasks(List<Task> tasks) {
    int highPriorityIndex = 0;
    
    // Move high priority to front
    for (int i = 0; i < tasks.size(); i++) {
        if (tasks.get(i).isHighPriority()) {
            Collections.swap(tasks, highPriorityIndex, i);
            highPriorityIndex++;
        }
    }
    // Process from index 0 onwards - high priority first
}
```

**Result:** Process high-priority tasks first without sorting entire array

### 5. Filtering API Responses

**Example:** Product catalog with stock status (UI)
```javascript
products = [
  { id: "P1", inStock: true },
  { id: "P2", inStock: false },
  { id: "P3", inStock: true },
  { id: "P4", inStock: false }
]

// Move in-stock products to front
let inStockIndex = 0;
for (let i = 0; i < products.length; i++) {
    if (products[i].inStock) {
        [products[inStockIndex], products[i]] = [products[i], products[inStockIndex]];
        inStockIndex++;
    }
}
```

**Memory Benefit:**
- `filter()` creates new array: O(n) extra space
- Move zeros pattern: O(1) space

### 6. Log File Processing (Backend)

**Example:** Processing log entries (Java)
```java
public void prioritizeErrorLogs(List<LogEntry> logs) {
    int errorIndex = 0;
    
    for (int i = 0; i < logs.size(); i++) {
        if (logs.get(i).isError()) {
            Collections.swap(logs, errorIndex, i);
            errorIndex++;
        }
    }
    // ERROR logs at front for quick review
}
```

**Pattern:** Move ERROR logs to front without creating filtered array

### 7. Cache Invalidation

**Example:** LRU Cache entry management
```javascript
cacheEntries = [
  { key: "user:1", valid: true },
  { key: "user:2", valid: false },   // expired
  { key: "user:3", valid: true },
  { key: "user:4", valid: false }    // expired
]

let validCount = 0;
for (let i = 0; i < cacheEntries.length; i++) {
    if (cacheEntries[i].valid) {
        [cacheEntries[validCount], cacheEntries[i]] = 
            [cacheEntries[i], cacheEntries[validCount]];
        validCount++;
    }
}

// Easy to truncate expired entries
cacheEntries.length = validCount;
```

---

## Why This Matters in Production

### Memory Efficiency
```javascript
// Bad: Creates new array (O(n) space)
const active = users.filter(u => !u.deleted);

// Good: In-place reordering (O(1) space)
moveDeletedToEnd(users);
```

### Performance
- No array copying
- No garbage collection overhead
- Fewer memory allocations
- Better for large datasets

### Practical Scenarios
- Mobile apps with limited memory
- Real-time dashboards with frequent updates
- Large datasets (10k+ records)
- Embedded systems with memory constraints

---

## Interview Tip

When explaining this problem, say:

"This pattern is useful for soft deletes, filtering active/inactive records, priority-based reordering, and any scenario where you need to partition data in-place without creating new arrays. It's memory-efficient and maintains relative order."

This demonstrates understanding of space complexity and real-world constraints.

---

## Key Takeaway

Move Zeros to End is a blueprint for in-place partitioning - separating elements by criteria without extra memory, while preserving order within each partition.