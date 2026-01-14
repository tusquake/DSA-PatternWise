# Two Sum - Real World Applications

## Problem Statement

Given an array of integers and a target value, find two numbers that add up to the target. Return the indices of these two numbers.

**Example 1:**
```
Input:  nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
Explanation: nums[0] + nums[1] = 2 + 7 = 9
```

**Example 2:**
```
Input:  nums = [3, 2, 4], target = 6
Output: [1, 2]
```

**Example 3:**
```
Input:  nums = [3, 3], target = 6
Output: [0, 1]
```

**Constraints:**
- Each input has exactly one solution
- Cannot use the same element twice
- Time Complexity: O(n)
- Space Complexity: O(n)

**Solution (Java):**
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        
        // Check if complement exists in map
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        
        // Store current number and its index
        map.put(nums[i], i);
    }
    
    return new int[] {}; // No solution found
}
```

---

## Core Concept

This problem is NOT about finding two numbers. It teaches:

- Hash map for O(1) lookups
- Complement pattern (target - current)
- Trading space for time efficiency
- Single-pass optimization

Any place where you need to find matching pairs, validate combinations, or check if complementary values exist - this pattern applies.

---

## Real-World Use Cases

### 1. E-Commerce: Product Bundling

**Problem:** Find two products that together match a customer's budget

**Example:** Gift bundle recommender (Backend - Java)
```java
public class BundleRecommender {
    
    public ProductPair findBundle(List<Product> products, double budget) {
        Map<Double, Product> priceMap = new HashMap<>();
        
        for (Product product : products) {
            double complement = budget - product.getPrice();
            
            // Check if complementary product exists
            if (priceMap.containsKey(complement)) {
                return new ProductPair(
                    priceMap.get(complement), 
                    product
                );
            }
            
            priceMap.put(product.getPrice(), product);
        }
        
        return null; // No bundle found
    }
}

// Usage: Find 2 products that cost exactly $100
ProductPair bundle = recommender.findBundle(products, 100.0);
```

**Use Case:**
- "Complete your gift under $50"
- "Buy 2 items for exactly $100"
- Bundle recommendations

### 2. Financial: Transaction Reconciliation

**Problem:** Match debit and credit transactions that cancel each other out

**Example:** Bank reconciliation system (Backend - Java)
```java
public class TransactionReconciler {
    
    public TransactionPair findMatchingTransactions(
            List<Transaction> transactions) {
        Map<Double, Transaction> amountMap = new HashMap<>();
        
        for (Transaction txn : transactions) {
            // Look for opposite amount (debit/credit pair)
            double opposite = -txn.getAmount();
            
            if (amountMap.containsKey(opposite)) {
                return new TransactionPair(
                    amountMap.get(opposite),
                    txn
                );
            }
            
            amountMap.put(txn.getAmount(), txn);
        }
        
        return null;
    }
}

// Find transactions like: +500 and -500 (should cancel out)
```

**Use Case:**
- Reconciling bank statements
- Finding matching deposits and withdrawals
- Expense matching

### 3. Inventory: Stock Level Balancing

**Problem:** Find two warehouses whose combined stock equals order quantity

**Example:** Warehouse allocation (Backend - Java)
```java
public class WarehouseAllocator {
    
    public WarehousePair findWarehouses(
            List<Warehouse> warehouses, int requiredQuantity) {
        Map<Integer, Warehouse> stockMap = new HashMap<>();
        
        for (Warehouse warehouse : warehouses) {
            int needed = requiredQuantity - warehouse.getStock();
            
            if (stockMap.containsKey(needed)) {
                return new WarehousePair(
                    stockMap.get(needed),
                    warehouse
                );
            }
            
            stockMap.put(warehouse.getStock(), warehouse);
        }
        
        return null;
    }
}

// Find 2 warehouses that together have exactly 1000 units
```

**Use Case:**
- Split shipments across warehouses
- Optimize delivery routes
- Stock balancing

### 4. Scheduling: Meeting Room Pairing

**Problem:** Find two time slots that together equal required duration

**Example:** Meeting scheduler (JavaScript)
```javascript
function findMeetingSlots(availableSlots, requiredMinutes) {
    const slotMap = new Map();
    
    for (let i = 0; i < availableSlots.length; i++) {
        const slot = availableSlots[i];
        const needed = requiredMinutes - slot.duration;
        
        // Check if complementary slot exists
        if (slotMap.has(needed)) {
            return {
                slot1: slotMap.get(needed),
                slot2: slot
            };
        }
        
        slotMap.set(slot.duration, slot);
    }
    
    return null;
}

// Find 2 slots that total 60 minutes
// Slots: [30min, 15min, 45min, 30min]
// Result: 30min + 30min = 60min
```

**Use Case:**
- Split long meetings across breaks
- Finding available time blocks
- Resource allocation

### 5. Gaming: Score Achievements

**Problem:** Find two players whose combined score reaches a target

**Example:** Team formation (JavaScript)
```javascript
function findTeamPair(players, targetScore) {
    const scoreMap = new Map();
    
    for (const player of players) {
        const needed = targetScore - player.score;
        
        if (scoreMap.has(needed)) {
            return {
                player1: scoreMap.get(needed),
                player2: player
            };
        }
        
        scoreMap.set(player.score, player);
    }
    
    return null;
}

// Find 2 players whose scores total 100 points
```

**Use Case:**
- Auto team balancing
- Achievement unlocks
- Tournament pairing

### 6. Nutrition: Meal Planning

**Problem:** Find two food items that match calorie target

**Example:** Diet planner (JavaScript)
```javascript
function findMealPair(foods, targetCalories) {
    const calorieMap = new Map();
    
    for (const food of foods) {
        const needed = targetCalories - food.calories;
        
        if (calorieMap.has(needed)) {
            return {
                food1: calorieMap.get(needed),
                food2: food
            };
        }
        
        calorieMap.set(food.calories, food);
    }
    
    return null;
}

// Find 2 foods that total exactly 500 calories
```

**Use Case:**
- Meal recommendations
- Calorie tracking apps
- Diet planning

### 7. Logistics: Package Weight Optimization

**Problem:** Find two packages that maximize truck capacity

**Example:** Load optimizer (Backend - Java)
```java
public class LoadOptimizer {
    
    public PackagePair optimizeLoad(
            List<Package> packages, double truckCapacity) {
        Map<Double, Package> weightMap = new HashMap<>();
        
        for (Package pkg : packages) {
            double remaining = truckCapacity - pkg.getWeight();
            
            if (weightMap.containsKey(remaining)) {
                return new PackagePair(
                    weightMap.get(remaining),
                    pkg
                );
            }
            
            weightMap.put(pkg.getWeight(), pkg);
        }
        
        return null;
    }
}

// Find 2 packages that exactly fill a 1000kg truck
```

**Use Case:**
- Maximizing truck capacity
- Shipping optimization
- Container loading

### 8. Chemistry/Manufacturing: Solution Mixing

**Problem:** Find two chemical concentrations that achieve target mixture

**Example:** Chemical mixer (Backend - Java)
```java
public class ChemicalMixer {
    
    public SolutionPair findMixture(
            List<Solution> solutions, double targetConcentration) {
        Map<Double, Solution> concentrationMap = new HashMap<>();
        
        for (Solution solution : solutions) {
            double needed = targetConcentration - solution.getConcentration();
            
            if (concentrationMap.containsKey(needed)) {
                return new SolutionPair(
                    concentrationMap.get(needed),
                    solution
                );
            }
            
            concentrationMap.put(solution.getConcentration(), solution);
        }
        
        return null;
    }
}
```

**Use Case:**
- Chemical formulation
- Quality control
- Recipe optimization

---

## Why This Matters in Production

### Time Complexity Comparison
```java
// Bad: Nested loops O(n²)
for (int i = 0; i < nums.length; i++) {
    for (int j = i + 1; j < nums.length; j++) {
        if (nums[i] + nums[j] == target) {
            return new int[] {i, j};
        }
    }
}

// Good: Hash map O(n)
Map<Integer, Integer> map = new HashMap<>();
for (int i = 0; i < nums.length; i++) {
    int complement = target - nums[i];
    if (map.containsKey(complement)) {
        return new int[] {map.get(complement), i};
    }
    map.put(nums[i], i);
}
```

### Performance Impact
- **1,000 items:** 1,000 operations vs 1,000,000 operations
- **10,000 items:** 10,000 operations vs 100,000,000 operations
- **Critical for:** Real-time systems, high-traffic APIs, mobile apps

### Practical Benefits
- Instant product recommendations
- Real-time transaction matching
- Fast warehouse allocation
- Responsive UI search

---

## Interview Tip

When explaining this problem, say:

"This pattern is useful for finding complementary pairs in any dataset - product bundling in e-commerce, transaction reconciliation in finance, warehouse allocation in logistics, or any scenario where you need to find two items that together meet a specific criteria. The hash map approach trades O(n) space for O(n) time, which is crucial for real-time systems."

This demonstrates understanding of time-space trade-offs and practical applications.

---

## Key Takeaway

Two Sum is a blueprint for the complement pattern - whenever you need to find matching pairs that satisfy a condition, use a hash map to store what you've seen and check for the complement in O(1) time.