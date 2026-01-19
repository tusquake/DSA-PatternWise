# Climbing Stairs - Real World Applications

## Problem Statement

You are climbing a staircase. It takes n steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

**Example 1:**
```
Input:  n = 2
Output: 2
Explanation: Two ways to climb to the top:
1. 1 step + 1 step
2. 2 steps
```

**Example 2:**
```
Input:  n = 3
Output: 3
Explanation: Three ways to climb to the top:
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

**Example 3:**
```
Input:  n = 4
Output: 5
Explanation: Five ways:
1. 1+1+1+1
2. 1+1+2
3. 1+2+1
4. 2+1+1
5. 2+2
```

**Constraints:**
- 1 <= n <= 45

**Solution (Java):**
```java
// Approach 1: Dynamic Programming (Bottom-up)
// Time: O(n), Space: O(n)
public int climbStairs(int n) {
    if (n <= 2) {
        return n;
    }
    
    int[] dp = new int[n + 1];
    dp[1] = 1; // 1 way to climb 1 step
    dp[2] = 2; // 2 ways to climb 2 steps
    
    for (int i = 3; i <= n; i++) {
        // Ways to reach step i = ways to reach (i-1) + ways to reach (i-2)
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}

// Approach 2: Space Optimized (Fibonacci pattern)
// Time: O(n), Space: O(1)
public int climbStairsOptimized(int n) {
    if (n <= 2) {
        return n;
    }
    
    int prev2 = 1; // dp[i-2]
    int prev1 = 2; // dp[i-1]
    
    for (int i = 3; i <= n; i++) {
        int current = prev1 + prev2;
        prev2 = prev1;
        prev1 = current;
    }
    
    return prev1;
}

// Approach 3: Recursive (with Memoization)
// Time: O(n), Space: O(n)
public int climbStairsMemo(int n) {
    Map<Integer, Integer> memo = new HashMap<>();
    return helper(n, memo);
}

private int helper(int n, Map<Integer, Integer> memo) {
    if (n <= 2) {
        return n;
    }
    
    if (memo.containsKey(n)) {
        return memo.get(n);
    }
    
    int result = helper(n - 1, memo) + helper(n - 2, memo);
    memo.put(n, result);
    
    return result;
}

// Approach 4: Fibonacci Formula (Mathematical)
// Time: O(1), Space: O(1)
public int climbStairsMath(int n) {
    double sqrt5 = Math.sqrt(5);
    double phi = (1 + sqrt5) / 2;
    double psi = (1 - sqrt5) / 2;
    
    return (int) Math.round((Math.pow(phi, n + 1) - Math.pow(psi, n + 1)) / sqrt5);
}
```

---

## Core Concept

This problem is NOT just about climbing stairs. It teaches:

- Dynamic programming fundamentals
- Fibonacci sequence pattern
- State transition (current state depends on previous states)
- Counting combinations efficiently

Any place where you need to count ways to reach a goal, calculate possible paths, or determine combination counts - this pattern applies.

---

## Real-World Use Cases

### 1. Payment Options: Calculate Payment Combinations

**Problem:** Calculate how many ways a customer can pay a bill with different denominations

**Example:** Payment calculator (Backend - Java)
```java
public class PaymentCalculator {
    
    public int calculatePaymentWays(int amount) {
        // Can pay with $1 or $2 bills
        // How many ways to pay $amount?
        
        if (amount <= 2) {
            return amount;
        }
        
        int[] dp = new int[amount + 1];
        dp[1] = 1; // Only one $1 bill
        dp[2] = 2; // Two $1 bills OR one $2 bill
        
        for (int i = 3; i <= amount; i++) {
            // Ways to pay $i = ways using last $1 + ways using last $2
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        
        return dp[amount];
    }
    
    public List<String> showPaymentOptions(int amount) {
        List<String> options = new ArrayList<>();
        // Generate all combinations
        generateCombinations(amount, "", options);
        return options;
    }
}

// Used in: POS systems, payment gateways
// Example: Pay $5 with $1 and $2 bills
```

**Use Case:**
- Point of sale systems
- Cash register calculations
- Payment flexibility analysis

### 2. Game Level Design: Path Counting

**Problem:** Calculate number of ways to complete a level with different move types

**Example:** Game path calculator (JavaScript)
```javascript
class GamePathCalculator {
    
    calculatePaths(levelSteps) {
        // Player can move 1 space or 2 spaces per turn
        // How many ways to complete the level?
        
        if (levelSteps <= 2) {
            return levelSteps;
        }
        
        const dp = new Array(levelSteps + 1);
        dp[1] = 1;
        dp[2] = 2;
        
        for (let i = 3; i <= levelSteps; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        
        return dp[levelSteps];
    }
    
    getDifficulty(levelSteps) {
        const paths = this.calculatePaths(levelSteps);
        
        if (paths > 1000) return "EASY - Many possible paths";
        if (paths > 100) return "MEDIUM - Multiple paths";
        return "HARD - Limited paths";
    }
}

// Used in: Platformer games, puzzle games
// Super Mario, Celeste use similar mechanics
```

**Use Case:**
- Game difficulty balancing
- Level design analysis
- Speedrun route counting

### 3. Delivery Routes: Package Delivery Combinations

**Problem:** Calculate delivery route options with 1 or 2 package drops per stop

**Example:** Route optimizer (Backend - Java)
```java
public class DeliveryRouteOptimizer {
    
    public int calculateRouteOptions(int totalPackages) {
        // Can deliver 1 or 2 packages per stop
        // How many different delivery sequences?
        
        if (totalPackages <= 2) {
            return totalPackages;
        }
        
        int prev2 = 1;
        int prev1 = 2;
        
        for (int i = 3; i <= totalPackages; i++) {
            int current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    public RouteAnalysis analyzeDelivery(int packages) {
        int options = calculateRouteOptions(packages);
        
        return new RouteAnalysis(
            packages,
            options,
            options > 100 ? "High flexibility" : "Limited options"
        );
    }
}

// UPS, FedEx route planning
// Amazon delivery optimization
```

**Use Case:**
- Logistics optimization
- Delivery route planning
- Package batching strategies

### 4. Project Management: Task Completion Sequences

**Problem:** Calculate ways to complete project with tasks taking 1 or 2 days

**Example:** Project scheduler (JavaScript)
```javascript
class ProjectScheduler {
    
    calculateCompletionWays(totalDays) {
        // Tasks can take 1 day or 2 days
        // How many ways to schedule to finish in totalDays?
        
        if (totalDays <= 2) {
            return totalDays;
        }
        
        let prev2 = 1;
        let prev1 = 2;
        
        for (let i = 3; i <= totalDays; i++) {
            const current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    getScheduleFlexibility(projectDays) {
        const ways = this.calculateCompletionWays(projectDays);
        
        return {
            totalWays: ways,
            flexibility: ways > 50 ? "HIGH" : "LOW",
            recommendation: ways > 50 
                ? "Multiple scheduling options available" 
                : "Limited flexibility - tight schedule"
        };
    }
}

// Project management tools: Jira, Asana
// Sprint planning optimization
```

**Use Case:**
- Agile sprint planning
- Project timeline flexibility
- Resource allocation analysis

### 5. Network Routing: Packet Path Calculation

**Problem:** Calculate number of paths packets can take through network hops

**Example:** Network path analyzer (Backend - Java)
```java
public class NetworkPathAnalyzer {
    
    public int calculatePaths(int totalHops) {
        // Packet can traverse 1 or 2 routers per hop
        // How many different paths exist?
        
        if (totalHops <= 2) {
            return totalHops;
        }
        
        int prev2 = 1;
        int prev1 = 2;
        
        for (int i = 3; i <= totalHops; i++) {
            int current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    public NetworkAnalysis analyzeRedundancy(int hops) {
        int paths = calculatePaths(hops);
        
        return new NetworkAnalysis(
            hops,
            paths,
            paths > 10 ? "High redundancy" : "Low redundancy",
            paths // More paths = better fault tolerance
        );
    }
}

// Network engineers use for redundancy planning
// CDN path optimization
```

**Use Case:**
- Network redundancy planning
- CDN routing optimization
- Fault tolerance analysis

### 6. Financial Planning: Investment Strategy Combinations

**Problem:** Calculate investment strategy combinations with 1-year or 2-year bonds

**Example:** Investment calculator (JavaScript)
```javascript
class InvestmentCalculator {
    
    calculateStrategies(totalYears) {
        // Can invest in 1-year or 2-year bonds
        // How many different investment strategies?
        
        if (totalYears <= 2) {
            return totalYears;
        }
        
        let prev2 = 1;
        let prev1 = 2;
        
        for (let i = 3; i <= totalYears; i++) {
            const current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    getInvestmentAdvice(years) {
        const strategies = this.calculateStrategies(years);
        
        return {
            investmentPeriod: years,
            possibleStrategies: strategies,
            diversification: strategies > 20 
                ? "Excellent diversification options" 
                : "Limited diversification",
            recommendation: `${strategies} different ways to allocate investments`
        };
    }
}

// Financial advisors use for portfolio planning
// Robo-advisors strategy generation
```

**Use Case:**
- Portfolio diversification analysis
- Investment strategy planning
- Financial planning tools

---

## Why This Matters in Production

### Performance Comparison
```
Approach 1 - Recursive (No Memo): O(2^n) - VERY SLOW
- n=40 takes hours!

Approach 2 - Dynamic Programming: O(n) time, O(n) space
- n=40 takes microseconds

Approach 3 - Space Optimized: O(n) time, O(1) space
- n=40 takes microseconds, minimal memory

Approach 4 - Math Formula: O(1) time, O(1) space
- Instant for any n (but precision issues for large n)
```

### Fibonacci Pattern Recognition
```
n=1: 1 way
n=2: 2 ways
n=3: 3 ways (1+2)
n=4: 5 ways (2+3)
n=5: 8 ways (3+5)

This is Fibonacci sequence!
F(n) = F(n-1) + F(n-2)
```

### Real-World Impact
- **Payment Systems:** Calculate millions of combination options
- **Game Design:** Balance difficulty based on path count
- **Logistics:** Optimize billions of delivery routes
- **Network Design:** Ensure redundancy with path analysis

---

## Interview Tip

When explaining this problem, say:

"Climbing Stairs is a fundamental dynamic programming problem that teaches the Fibonacci pattern. While it looks like a simple stair-climbing question, the pattern applies to counting combinations in payment systems, game path calculations, delivery route optimization, project scheduling flexibility, network redundancy planning, and investment strategy counting. The key insight is recognizing that to reach step n, you must come from either step n-1 or n-2, making it dp[n] = dp[n-1] + dp[n-2]. The space-optimized O(1) solution using just two variables is production-ready and used in real-time systems processing millions of calculations."

This demonstrates pattern recognition and understanding of optimization trade-offs.

---

## Key Takeaway

Climbing Stairs is a blueprint for combination counting using dynamic programming. The Fibonacci recurrence relation (current = previous + before_previous) appears everywhere - payment combinations, game paths, delivery routes, project schedules, network redundancy, and investment strategies. The O(n) time, O(1) space solution is production-standard for counting problems.