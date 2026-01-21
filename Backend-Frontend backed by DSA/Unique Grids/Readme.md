# Unique Paths (Grid Navigation) - Real World Applications

## Problem Statement

### Unique Paths I
There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m-1][n-1]). The robot can only move either down or right at any point in time. Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

**Example 1:**
```
Input:  m = 3, n = 7
Output: 28
```

**Example 2:**
```
Input:  m = 3, n = 2
Output: 3
Explanation: From top-left to bottom-right, there are 3 paths:
1. Right -> Down -> Down
2. Down -> Right -> Down
3. Down -> Down -> Right
```

### Unique Paths II
Now consider if some obstacles are added to the grids. How many unique paths would there be? An obstacle is marked as 1 and empty space is marked as 0.

**Example:**
```
Input:  obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There's one obstacle in middle of 3x3 grid.
Two unique paths:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

**Constraints:**
- 1 <= m, n <= 100
- obstacleGrid[i][j] is 0 or 1

**Solution (Java):**
```java
// UNIQUE PATHS I

// Approach 1: Dynamic Programming 2D
// Time: O(m*n), Space: O(m*n)
public int uniquePaths(int m, int n) {
    int[][] dp = new int[m][n];
    
    // First row - only one way (all right)
    for (int j = 0; j < n; j++) {
        dp[0][j] = 1;
    }
    
    // First column - only one way (all down)
    for (int i = 0; i < m; i++) {
        dp[i][0] = 1;
    }
    
    // Fill rest of grid
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            // Paths = paths from top + paths from left
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }
    }
    
    return dp[m-1][n-1];
}

// Approach 2: Space Optimized (1D array)
// Time: O(m*n), Space: O(n)
public int uniquePathsOptimized(int m, int n) {
    int[] dp = new int[n];
    Arrays.fill(dp, 1); // First row all 1s
    
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[j] = dp[j] + dp[j-1];
        }
    }
    
    return dp[n-1];
}

// Approach 3: Combinatorics (Mathematical)
// Time: O(m+n), Space: O(1)
public int uniquePathsMath(int m, int n) {
    // Total moves = (m-1) down + (n-1) right = m+n-2
    // Choose (m-1) positions for down moves
    // Formula: C(m+n-2, m-1)
    
    long result = 1;
    int totalMoves = m + n - 2;
    int downMoves = m - 1;
    
    for (int i = 1; i <= downMoves; i++) {
        result = result * (totalMoves - downMoves + i) / i;
    }
    
    return (int) result;
}

// UNIQUE PATHS II (with obstacles)

// Dynamic Programming with obstacles
// Time: O(m*n), Space: O(m*n)
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    int m = obstacleGrid.length;
    int n = obstacleGrid[0].length;
    
    // If start or end is blocked
    if (obstacleGrid[0][0] == 1 || obstacleGrid[m-1][n-1] == 1) {
        return 0;
    }
    
    int[][] dp = new int[m][n];
    dp[0][0] = 1;
    
    // Fill first column
    for (int i = 1; i < m; i++) {
        dp[i][0] = (obstacleGrid[i][0] == 0 && dp[i-1][0] == 1) ? 1 : 0;
    }
    
    // Fill first row
    for (int j = 1; j < n; j++) {
        dp[0][j] = (obstacleGrid[0][j] == 0 && dp[0][j-1] == 1) ? 1 : 0;
    }
    
    // Fill rest
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (obstacleGrid[i][j] == 1) {
                dp[i][j] = 0; // Obstacle - no paths
            } else {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
    }
    
    return dp[m-1][n-1];
}
```

---

## Core Concept

This problem is NOT just about grid navigation. It teaches:

- Dynamic programming (building solutions from subproblems)
- Path counting in constrained environments
- Obstacle avoidance strategies
- Combinatorial mathematics

Any place where you need to count routes, plan navigation, calculate delivery paths, or optimize movement in constrained spaces - this pattern applies.

---

## Real-World Use Cases

### 1. Warehouse Robot Navigation: Optimal Path Counting

**Problem:** Calculate number of paths for warehouse robots moving through grid layout

**Example:** Warehouse path planner (Backend - Java)
```java
public class WarehousePathPlanner {
    
    public int calculatePaths(int rows, int cols, int[][] obstacles) {
        // Grid with shelves (obstacles)
        // Robot can only move right or down
        
        int[][] dp = new int[rows][cols];
        
        // Start position
        dp[0][0] = obstacles[0][0] == 1 ? 0 : 1;
        
        // First row
        for (int j = 1; j < cols; j++) {
            dp[0][j] = (obstacles[0][j] == 0 && dp[0][j-1] == 1) ? 1 : 0;
        }
        
        // First column
        for (int i = 1; i < rows; i++) {
            dp[i][0] = (obstacles[i][0] == 0 && dp[i-1][0] == 1) ? 1 : 0;
        }
        
        // Fill grid
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {
                if (obstacles[i][j] == 1) {
                    dp[i][j] = 0; // Shelf blocking
                } else {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        
        return dp[rows-1][cols-1];
    }
    
    public PathAnalysis analyzeWarehouse(int rows, int cols, int[][] shelves) {
        int paths = calculatePaths(rows, cols, shelves);
        
        return new PathAnalysis(
            paths,
            paths > 10 ? "HIGH FLEXIBILITY" : "LIMITED ROUTES",
            paths == 0 ? "RECONFIGURE LAYOUT" : "LAYOUT OK"
        );
    }
}

// Amazon, Alibaba warehouse automation
// Determines routing flexibility for robots
```

**Use Case:**
- Warehouse automation
- Robot path planning
- Layout optimization

### 2. Delivery Route Planning: City Grid Navigation

**Problem:** Count possible delivery routes through city blocks

**Example:** Delivery route calculator (JavaScript)
```javascript
class DeliveryRouteCalculator {
    
    calculateRoutes(cityBlocks, roadClosures) {
        const rows = cityBlocks.length;
        const cols = cityBlocks[0].length;
        const dp = Array(rows).fill(0).map(() => Array(cols).fill(0));
        
        // Starting point
        dp[0][0] = roadClosures[0][0] === 0 ? 1 : 0;
        
        // Fill first row (only right movement)
        for (let j = 1; j < cols; j++) {
            dp[0][j] = roadClosures[0][j] === 0 && dp[0][j-1] === 1 ? 1 : 0;
        }
        
        // Fill first column (only down movement)
        for (let i = 1; i < rows; i++) {
            dp[i][0] = roadClosures[i][0] === 0 && dp[i-1][0] === 1 ? 1 : 0;
        }
        
        // Calculate paths
        for (let i = 1; i < rows; i++) {
            for (let j = 1; j < cols; j++) {
                if (roadClosures[i][j] === 1) {
                    dp[i][j] = 0; // Road closed
                } else {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        
        return dp[rows-1][cols-1];
    }
    
    optimizeDelivery(grid, closures) {
        const routes = this.calculateRoutes(grid, closures);
        
        return {
            availableRoutes: routes,
            flexibility: routes > 5 ? "HIGH" : "LOW",
            recommendation: routes === 0 
                ? "Find alternative destination" 
                : `${routes} possible routes available`
        };
    }
}

// UberEats, DoorDash route optimization
// Handles construction, traffic restrictions
```

**Use Case:**
- Food delivery routing
- Package delivery planning
- Urban logistics

### 3. Game Level Design: Path Complexity Analysis

**Problem:** Calculate number of ways to complete game level

**Example:** Game difficulty analyzer (JavaScript)
```javascript
class GameDifficultyAnalyzer {
    
    calculateCompletionPaths(levelGrid, obstacles) {
        const rows = levelGrid.length;
        const cols = levelGrid[0].length;
        const dp = Array(rows).fill(0).map(() => Array(cols).fill(0));
        
        dp[0][0] = obstacles[0][0] === 0 ? 1 : 0;
        
        for (let j = 1; j < cols; j++) {
            dp[0][j] = obstacles[0][j] === 0 && dp[0][j-1] === 1 ? 1 : 0;
        }
        
        for (let i = 1; i < rows; i++) {
            dp[i][0] = obstacles[i][0] === 0 && dp[i-1][0] === 1 ? 1 : 0;
        }
        
        for (let i = 1; i < rows; i++) {
            for (let j = 1; j < cols; j++) {
                if (obstacles[i][j] === 1) {
                    dp[i][j] = 0; // Wall or hazard
                } else {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        
        return dp[rows-1][cols-1];
    }
    
    assessDifficulty(grid, hazards) {
        const paths = this.calculateCompletionPaths(grid, hazards);
        
        let difficulty;
        if (paths === 0) difficulty = "IMPOSSIBLE";
        else if (paths === 1) difficulty = "HARD - Single path";
        else if (paths < 5) difficulty = "MEDIUM";
        else difficulty = "EASY - Multiple routes";
        
        return {
            completionPaths: paths,
            difficulty: difficulty,
            playerExperience: paths > 3 
                ? "Good replayability" 
                : "Linear experience"
        };
    }
}

// Game developers: Level complexity analysis
// Puzzle games, platformers
```

**Use Case:**
- Game difficulty balancing
- Level design validation
- Replayability analysis

### 4. Network Routing: Data Packet Path Counting

**Problem:** Count possible paths through network topology

**Example:** Network path analyzer (Backend - Java)
```java
public class NetworkPathAnalyzer {
    
    public int calculateRedundancy(int[][] networkGrid, int[][] failedNodes) {
        int rows = networkGrid.length;
        int cols = networkGrid[0].length;
        int[][] dp = new int[rows][cols];
        
        // Source node
        dp[0][0] = failedNodes[0][0] == 1 ? 0 : 1;
        
        // Initialize edges
        for (int j = 1; j < cols; j++) {
            dp[0][j] = (failedNodes[0][j] == 0 && dp[0][j-1] == 1) ? 1 : 0;
        }
        
        for (int i = 1; i < rows; i++) {
            dp[i][0] = (failedNodes[i][0] == 0 && dp[i-1][0] == 1) ? 1 : 0;
        }
        
        // Count paths
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {
                if (failedNodes[i][j] == 1) {
                    dp[i][j] = 0; // Node down
                } else {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        
        return dp[rows-1][cols-1];
    }
    
    public NetworkHealth assessHealth(int[][] topology, int[][] failures) {
        int paths = calculateRedundancy(topology, failures);
        
        return new NetworkHealth(
            paths,
            paths > 5 ? "EXCELLENT REDUNDANCY" : 
            paths > 0 ? "BASIC REDUNDANCY" : "CRITICAL - NO PATHS",
            paths > 0 ? "Network operational" : "Service disruption"
        );
    }
}

// SDN controllers, network management
// Cisco, Juniper routing analysis
```

**Use Case:**
- Network redundancy planning
- Fault tolerance assessment
- Route diversity analysis

### 5. Agricultural Planning: Field Navigation Paths

**Problem:** Calculate harvesting routes through crop fields

**Example:** Harvest path planner (JavaScript)
```javascript
class HarvestPathPlanner {
    
    calculateHarvestPaths(fieldGrid, irrigationBlocks) {
        const rows = fieldGrid.length;
        const cols = fieldGrid[0].length;
        const dp = Array(rows).fill(0).map(() => Array(cols).fill(0));
        
        dp[0][0] = irrigationBlocks[0][0] === 0 ? 1 : 0;
        
        for (let j = 1; j < cols; j++) {
            dp[0][j] = irrigationBlocks[0][j] === 0 && dp[0][j-1] === 1 ? 1 : 0;
        }
        
        for (let i = 1; i < rows; i++) {
            dp[i][0] = irrigationBlocks[i][0] === 0 && dp[i-1][0] === 1 ? 1 : 0;
        }
        
        for (let i = 1; i < rows; i++) {
            for (let j = 1; j < cols; j++) {
                if (irrigationBlocks[i][j] === 1) {
                    dp[i][j] = 0; // Irrigation system blocking
                } else {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        
        return dp[rows-1][cols-1];
    }
    
    planHarvest(field, obstacles) {
        const paths = this.calculateHarvestPaths(field, obstacles);
        
        return {
            possiblePaths: paths,
            efficiency: paths > 10 ? "HIGH - Multiple routes" : "LIMITED",
            recommendation: paths < 3 
                ? "Consider repositioning irrigation systems" 
                : "Sufficient path flexibility"
        };
    }
}

// John Deere, precision agriculture
// Autonomous harvester planning
```

**Use Case:**
- Precision agriculture
- Autonomous farming equipment
- Field optimization

### 6. Data Center Layout: Server Access Paths

**Problem:** Calculate paths through data center aisles

**Example:** Data center path calculator (Backend - Java)
```java
public class DataCenterPathCalculator {
    
    public int calculateAccessPaths(int aisleRows, int aisleCols, 
                                     int[][] equipment) {
        int[][] dp = new int[aisleRows][aisleCols];
        
        dp[0][0] = equipment[0][0] == 1 ? 0 : 1;
        
        // First row
        for (int j = 1; j < aisleCols; j++) {
            dp[0][j] = (equipment[0][j] == 0 && dp[0][j-1] == 1) ? 1 : 0;
        }
        
        // First column
        for (int i = 1; i < aisleRows; i++) {
            dp[i][0] = (equipment[i][0] == 0 && dp[i-1][0] == 1) ? 1 : 0;
        }
        
        // Calculate paths
        for (int i = 1; i < aisleRows; i++) {
            for (int j = 1; j < aisleCols; j++) {
                if (equipment[i][j] == 1) {
                    dp[i][j] = 0; // Equipment rack blocking
                } else {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        
        return dp[aisleRows-1][aisleCols-1];
    }
    
    public LayoutAnalysis analyzeLayout(int rows, int cols, int[][] racks) {
        int paths = calculateAccessPaths(rows, cols, racks);
        
        return new LayoutAnalysis(
            paths,
            paths > 3 ? "GOOD - Multiple access routes" : "LIMITED ACCESS",
            paths == 0 ? "BLOCKED - Reconfigure layout" : "Layout functional"
        );
    }
}

// Google, AWS data center planning
// Ensures maintenance access paths
```

**Use Case:**
- Data center layout planning
- Maintenance access optimization
- Fire safety compliance

---

## Why This Matters in Production

### Dynamic Programming Pattern
```
Key insight: 
To reach cell [i][j], must come from [i-1][j] (top) or [i][j-1] (left)

dp[i][j] = dp[i-1][j] + dp[i][j-1]

Base cases:
- First row: only one way (all right moves)
- First column: only one way (all down moves)

With obstacles:
- If obstacle at [i][j]: dp[i][j] = 0
```

### Space Optimization
```
2D DP: O(m*n) space - stores entire grid
1D DP: O(n) space - only stores current row

For large grids:
100x100 grid: 10,000 cells vs 100 cells
99% memory savings!
```

### Real-World Scale
- **Warehouses:** 50x50 grids, calculate paths in real-time
- **Cities:** 20x20 blocks, thousands of route calculations/day
- **Games:** Level complexity for millions of players
- **Networks:** 100+ node topologies, continuous monitoring
- **Agriculture:** Large fields, seasonal planning
- **Data Centers:** Floor planning for hundreds of racks

---

## Interview Tip

When explaining this problem, say:

"Unique Paths teaches dynamic programming for path counting in grid-based environments. The DP recurrence dp[i][j] = dp[i-1][j] + dp[i][j-1] counts paths by summing ways to arrive from adjacent cells. Unique Paths II extends this with obstacle handling by setting dp[i][j] = 0 for blocked cells. This pattern is critical in warehouse robot navigation for calculating route flexibility, delivery planning through city grids, game level design for difficulty assessment, network routing for redundancy analysis, agricultural planning for harvest optimization, and data center layout for maintenance access. The space-optimized O(n) solution is production-ready for real-time path calculations."

This demonstrates DP mastery and real-world system design thinking.

---

## Key Takeaway

Unique Paths is a blueprint for counting paths in constrained grid environments using dynamic programming. The pattern of building solutions from subproblems (paths to current cell = paths from top + paths from left) is essential for warehouse automation, delivery routing, game design, network planning, precision agriculture, and data center layout - any domain requiring path analysis in grid-based systems with movement constraints.